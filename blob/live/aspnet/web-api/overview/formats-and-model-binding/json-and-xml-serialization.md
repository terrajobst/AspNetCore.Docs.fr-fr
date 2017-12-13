---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: "JSON et sérialisation XML dans ASP.NET Web API | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 7aafe4823d3a6090fae4a63f1a66fb2670ecb025
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="cfded-102">JSON et sérialisation XML dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cfded-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="cfded-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cfded-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cfded-104">Cet article décrit les formateurs JSON et XML dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cfded-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="cfded-105">Dans ASP.NET Web API, un *formateur de type de média* est un objet qui peut :</span><span class="sxs-lookup"><span data-stu-id="cfded-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="cfded-106">Objets CLR de la lecture à partir d’un HTTP de corps du message</span><span class="sxs-lookup"><span data-stu-id="cfded-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="cfded-107">Écrire des objets CLR dans un corps de message HTTP</span><span class="sxs-lookup"><span data-stu-id="cfded-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="cfded-108">API Web fournit les formateurs de type de média pour JSON et XML.</span><span class="sxs-lookup"><span data-stu-id="cfded-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="cfded-109">Le framework ajoute ces formateurs au pipeline par défaut.</span><span class="sxs-lookup"><span data-stu-id="cfded-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="cfded-110">Les clients peuvent demander JSON ou XML dans l’en-tête Accept de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="cfded-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="cfded-111">Sommaire</span><span class="sxs-lookup"><span data-stu-id="cfded-111">Contents</span></span>

- [<span data-ttu-id="cfded-112">Formateur de Type de média JSON</span><span class="sxs-lookup"><span data-stu-id="cfded-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="cfded-113">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cfded-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="cfded-114">Dates</span><span class="sxs-lookup"><span data-stu-id="cfded-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="cfded-115">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cfded-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="cfded-116">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="cfded-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="cfded-117">Objets anonymes et faiblement typée</span><span class="sxs-lookup"><span data-stu-id="cfded-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="cfded-118">Formateur de Type de média XML</span><span class="sxs-lookup"><span data-stu-id="cfded-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="cfded-119">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cfded-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="cfded-120">Dates</span><span class="sxs-lookup"><span data-stu-id="cfded-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="cfded-121">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cfded-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="cfded-122">Sérialiseurs XML par Type de paramètre</span><span class="sxs-lookup"><span data-stu-id="cfded-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="cfded-123">Supprimer le JSON ou un formateur XML</span><span class="sxs-lookup"><span data-stu-id="cfded-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="cfded-124">La gestion des références d’objet circulaires</span><span class="sxs-lookup"><span data-stu-id="cfded-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="cfded-125">Test de la sérialisation d’objets</span><span class="sxs-lookup"><span data-stu-id="cfded-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="cfded-126">Formateur de Type de média JSON</span><span class="sxs-lookup"><span data-stu-id="cfded-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="cfded-127">Mise en forme de JSON est fournie par le **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="cfded-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="cfded-128">Par défaut, **JsonMediaTypeFormatter** utilise le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) bibliothèque pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="cfded-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="cfded-129">Json.NET est un projet de tiers open source.</span><span class="sxs-lookup"><span data-stu-id="cfded-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="cfded-130">Si vous préférez, vous pouvez configurer le **JsonMediaTypeFormatter** classe à utiliser le **DataContractJsonSerializer** au lieu de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="cfded-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="cfded-131">Pour ce faire, définissez la **UseDataContractJsonSerializer** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="cfded-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="cfded-132">Sérialisation JSON</span><span class="sxs-lookup"><span data-stu-id="cfded-132">JSON Serialization</span></span>

<span data-ttu-id="cfded-133">Cette section décrit certains des comportements spécifiques du module de formatage JSON, à l’aide de la valeur par défaut [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) sérialiseur.</span><span class="sxs-lookup"><span data-stu-id="cfded-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="cfded-134">Il n’est pas destiné à être une documentation complète de la bibliothèque de Json.NET ; Pour plus d’informations, consultez la [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="cfded-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="cfded-135">Éléments sérialisés ?</span><span class="sxs-lookup"><span data-stu-id="cfded-135">What Gets Serialized?</span></span>

<span data-ttu-id="cfded-136">Par défaut, toutes les propriétés et champs publics sont inclus dans l’objet JSON sérialisé.</span><span class="sxs-lookup"><span data-stu-id="cfded-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="cfded-137">Pour omettre une propriété ou un champ, la décorer avec le **JsonIgnore** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="cfded-138">Si vous préférez un &quot;participer&quot; approche, décorez la classe avec le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="cfded-139">Si cet attribut est présent, les membres sont ignorés, sauf si elles ont le **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="cfded-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="cfded-140">Vous pouvez également utiliser **DataMember** pour sérialiser les membres privés.</span><span class="sxs-lookup"><span data-stu-id="cfded-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="cfded-141">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cfded-141">Read-Only Properties</span></span>

<span data-ttu-id="cfded-142">Propriétés en lecture seule sont sérialisées par défaut.</span><span class="sxs-lookup"><span data-stu-id="cfded-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="cfded-143">dates</span><span class="sxs-lookup"><span data-stu-id="cfded-143">Dates</span></span>

<span data-ttu-id="cfded-144">Par défaut, Json.NET écrit des dates [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span><span class="sxs-lookup"><span data-stu-id="cfded-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="cfded-145">Les dates au format UTC (Coordinated Universal Time) sont écrites avec un suffixe « Z ».</span><span class="sxs-lookup"><span data-stu-id="cfded-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="cfded-146">Les dates en heure locale incluent un décalage de fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="cfded-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="cfded-147">Exemple :</span><span class="sxs-lookup"><span data-stu-id="cfded-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="cfded-148">Par défaut, Json.NET conserve le fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="cfded-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="cfded-149">Vous pouvez le remplacer en définissant la propriété DateTimeZoneHandling :</span><span class="sxs-lookup"><span data-stu-id="cfded-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="cfded-150">Si vous préférez utiliser [format de date Microsoft JSON](https://msdn.microsoft.com/en-us/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu de la norme ISO 8601, définissez la **DateFormatHandling** propriété sur les paramètres du sérialiseur :</span><span class="sxs-lookup"><span data-stu-id="cfded-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/en-us/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="cfded-151">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cfded-151">Indenting</span></span>

<span data-ttu-id="cfded-152">Pour écrire le code JSON, définissez la **mise en forme** à **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="cfded-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="cfded-153">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="cfded-153">Camel Casing</span></span>

<span data-ttu-id="cfded-154">Pour écrire des noms de propriété JSON avec une casse mixte, sans modifier votre modèle de données, définissez la **CamelCasePropertyNamesContractResolver** sur le sérialiseur de :</span><span class="sxs-lookup"><span data-stu-id="cfded-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="cfded-155">Objets anonymes et faiblement typée</span><span class="sxs-lookup"><span data-stu-id="cfded-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="cfded-156">Une méthode d’action peut retourner un objet anonyme et sa sérialisation au format JSON.</span><span class="sxs-lookup"><span data-stu-id="cfded-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="cfded-157">Exemple :</span><span class="sxs-lookup"><span data-stu-id="cfded-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="cfded-158">Le corps du message de réponse contient le texte JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="cfded-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="cfded-159">Si votre site web API reçoit faiblement structuré objets JSON à partir de clients, vous pouvez désérialiser le corps de la demande à un **Newtonsoft.Json.Linq.JObject** type.</span><span class="sxs-lookup"><span data-stu-id="cfded-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="cfded-160">Toutefois, il est généralement préférable d’utiliser des objets de données fortement typées.</span><span class="sxs-lookup"><span data-stu-id="cfded-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="cfded-161">Puis vous n’avez pas besoin analyser les données vous-même, et que vous obtenez les avantages de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="cfded-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="cfded-162">Le sérialiseur XML ne prend pas en charge les types anonymes ou **JObject** instances.</span><span class="sxs-lookup"><span data-stu-id="cfded-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="cfded-163">Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML provenant du pipeline, comme décrit plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="cfded-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="cfded-164">Formateur de Type de média XML</span><span class="sxs-lookup"><span data-stu-id="cfded-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="cfded-165">Mise en forme XML est fournie par le **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="cfded-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="cfded-166">Par défaut, **XmlMediaTypeFormatter** utilise le **DataContractSerializer** classe pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="cfded-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="cfded-167">Si vous préférez, vous pouvez configurer le **XmlMediaTypeFormatter** à utiliser le **XmlSerializer** au lieu du **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="cfded-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="cfded-168">Pour ce faire, définissez la **/usexmlserializer** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="cfded-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="cfded-169">Le **XmlSerializer** classe prend en charge un ensemble plus restreint de types à **DataContractSerializer**, mais offre plus de contrôle sur le code XML résultant.</span><span class="sxs-lookup"><span data-stu-id="cfded-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="cfded-170">Envisagez d’utiliser **XmlSerializer** si vous avez besoin correspondre à un schéma XML existant.</span><span class="sxs-lookup"><span data-stu-id="cfded-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="cfded-171">Sérialisation XML</span><span class="sxs-lookup"><span data-stu-id="cfded-171">XML Serialization</span></span>

<span data-ttu-id="cfded-172">Cette section décrit certains des comportements spécifiques du module de formatage XML, à l’aide de la valeur par défaut **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="cfded-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="cfded-173">Par défaut, le DataContractSerializer se comporte comme suit :</span><span class="sxs-lookup"><span data-stu-id="cfded-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="cfded-174">Tous les champs et les propriétés en lecture/écriture publique sont sérialisés.</span><span class="sxs-lookup"><span data-stu-id="cfded-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="cfded-175">Pour omettre une propriété ou un champ, la décorer avec le **IgnoreDataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="cfded-176">Les membres privés et protégés ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cfded-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="cfded-177">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cfded-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="cfded-178">(Toutefois, le contenu d’une propriété de collection en lecture seule est sérialisé.)</span><span class="sxs-lookup"><span data-stu-id="cfded-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="cfded-179">Noms de classes et membres sont écrites dans le fichier XML exactement comme elles apparaissent dans la déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="cfded-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="cfded-180">Un espace de noms XML par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="cfded-180">A default XML namespace is used.</span></span>

<span data-ttu-id="cfded-181">Si vous avez besoin de mieux contrôler la sérialisation, vous pouvez la décorer la classe avec le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="cfded-182">Lorsque cet attribut est présent, la classe est sérialisée comme suit :</span><span class="sxs-lookup"><span data-stu-id="cfded-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="cfded-183">&quot;S’abonner&quot; approche : propriétés et les champs par défaut ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cfded-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="cfded-184">Pour sérialiser une propriété ou un champ, la décorer avec le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="cfded-185">Pour sérialiser un membre privé ou protégé, la décorer avec le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="cfded-186">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cfded-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="cfded-187">Pour modifier la façon dont le nom de classe s’affiche dans le document XML, définissez la *nom* paramètre dans le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="cfded-188">Pour modifier la façon dont un nom de membre s’affiche dans le document XML, définissez la *nom* paramètre dans le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="cfded-189">Pour modifier l’espace de noms XML, définissez la *Namespace* paramètre dans le **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="cfded-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="cfded-190">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cfded-190">Read-Only Properties</span></span>

<span data-ttu-id="cfded-191">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cfded-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="cfded-192">Si une propriété en lecture seule a un champ de stockage privé, vous pouvez marquer le champ privé avec les **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="cfded-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="cfded-193">Cette approche exige la **DataContract** attribut sur la classe.</span><span class="sxs-lookup"><span data-stu-id="cfded-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="cfded-194">dates</span><span class="sxs-lookup"><span data-stu-id="cfded-194">Dates</span></span>

<span data-ttu-id="cfded-195">Dates sont écrites au format ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="cfded-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="cfded-196">Par exemple, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="cfded-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="cfded-197">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cfded-197">Indenting</span></span>

<span data-ttu-id="cfded-198">Pour écrire le code XML, définissez la **retrait** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="cfded-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="cfded-199">Sérialiseurs XML par Type de paramètre</span><span class="sxs-lookup"><span data-stu-id="cfded-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="cfded-200">Vous pouvez définir des sérialiseurs XML différents pour différents types CLR.</span><span class="sxs-lookup"><span data-stu-id="cfded-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="cfded-201">Par exemple, vous pouvez avoir un objet de données particulier qui nécessite **XmlSerializer** pour la compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="cfded-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="cfded-202">Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.</span><span class="sxs-lookup"><span data-stu-id="cfded-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="cfded-203">Pour définir un sérialiseur XML pour un type particulier, appelez **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="cfded-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="cfded-204">Vous pouvez spécifier une **XmlSerializer** ou tout objet qui dérive de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="cfded-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="cfded-205">Supprimer le JSON ou un formateur XML</span><span class="sxs-lookup"><span data-stu-id="cfded-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="cfded-206">Vous pouvez supprimer le module de formatage JSON ou le formateur XML à partir de la liste des modules de formatage, si vous ne souhaitez pas les utiliser.</span><span class="sxs-lookup"><span data-stu-id="cfded-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="cfded-207">Les principales raisons à cela sont :</span><span class="sxs-lookup"><span data-stu-id="cfded-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="cfded-208">Pour limiter les réponses de l’API web à un type de média spécifique.</span><span class="sxs-lookup"><span data-stu-id="cfded-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="cfded-209">Par exemple, vous pouvez décider de prendre en charge uniquement les réponses JSON et de supprimer le formateur XML.</span><span class="sxs-lookup"><span data-stu-id="cfded-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="cfded-210">Pour remplacer le formateur par défaut par un formateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="cfded-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="cfded-211">Par exemple, vous pouvez remplacer le module de formatage JSON avec votre propre implémentation personnalisée d’un formateur JSON.</span><span class="sxs-lookup"><span data-stu-id="cfded-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="cfded-212">Le code suivant montre comment supprimer les modules de formatage par défaut.</span><span class="sxs-lookup"><span data-stu-id="cfded-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="cfded-213">L’appeler à partir de votre **Application\_Démarrer** méthode, définie dans Global.asax.</span><span class="sxs-lookup"><span data-stu-id="cfded-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="cfded-214">La gestion des références d’objet circulaires</span><span class="sxs-lookup"><span data-stu-id="cfded-214">Handling Circular Object References</span></span>

<span data-ttu-id="cfded-215">Par défaut, les formateurs JSON et XML écrivent tous les objets sous forme de valeurs.</span><span class="sxs-lookup"><span data-stu-id="cfded-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="cfded-216">Si les deux propriétés font référence au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialise l’objet à deux reprises.</span><span class="sxs-lookup"><span data-stu-id="cfded-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="cfded-217">Ceci est un problème particulier si votre graphique d’objet contient des cycles, étant donné que le sérialiseur lève une exception lorsqu’il détecte une boucle dans le graphique.</span><span class="sxs-lookup"><span data-stu-id="cfded-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="cfded-218">Pensez aux modèles d’objet suivants et le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="cfded-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="cfded-219">Appel de cette action provoque le formateur à levé une exception, ce qui se traduit par un état code 500 (erreur interne du serveur) réponse au client.</span><span class="sxs-lookup"><span data-stu-id="cfded-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="cfded-220">Pour conserver les références d’objet dans JSON, ajoutez le code suivant à **Application\_Démarrer** méthode dans le fichier Global.asax :</span><span class="sxs-lookup"><span data-stu-id="cfded-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="cfded-221">L’action du contrôleur retournent désormais JSON qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="cfded-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="cfded-222">Notez que le sérialiseur ajoute un &quot;$id&quot; propriété pour les deux objets.</span><span class="sxs-lookup"><span data-stu-id="cfded-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="cfded-223">En outre, il détecte que la propriété Employee.Department crée une boucle, elle remplace la valeur par une référence d’objet : {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="cfded-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="cfded-224">Références d’objet ne sont pas standard dans JSON.</span><span class="sxs-lookup"><span data-stu-id="cfded-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="cfded-225">Avant d’utiliser cette fonctionnalité, considérez si vos clients seront en mesure d’analyser les résultats.</span><span class="sxs-lookup"><span data-stu-id="cfded-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="cfded-226">Il peut être préférable de supprimer les cycles du graphique.</span><span class="sxs-lookup"><span data-stu-id="cfded-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="cfded-227">Par exemple, le lien d’employé au service n’est pas vraiment nécessaire dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="cfded-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="cfded-228">Pour conserver les références d’objet dans XML, vous avez deux options.</span><span class="sxs-lookup"><span data-stu-id="cfded-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="cfded-229">L’option la plus simple consiste à ajouter `[DataContract(IsReference=true)]` à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="cfded-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="cfded-230">Le *IsReference* paramètre permet les références d’objet.</span><span class="sxs-lookup"><span data-stu-id="cfded-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="cfded-231">N’oubliez pas que **DataContract** effectue la sérialisation de participer, donc vous devrez également ajouter **DataMember** aux propriétés d’attributs :</span><span class="sxs-lookup"><span data-stu-id="cfded-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="cfded-232">Maintenant le formateur produira XML semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="cfded-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="cfded-233">Si vous souhaitez éviter d’attributs sur votre classe de modèle, il existe une autre option : créer un nouveau spécifiques au type **DataContractSerializer** de l’instance et définissez *preserveObjectReferences* à **true**  dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="cfded-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="cfded-234">Puis, définissez cette instance comme un sérialiseur par type sur le formateur de type de média XML.</span><span class="sxs-lookup"><span data-stu-id="cfded-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="cfded-235">Le code suivant montre comment procéder :</span><span class="sxs-lookup"><span data-stu-id="cfded-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="cfded-236">Test de la sérialisation d’objets</span><span class="sxs-lookup"><span data-stu-id="cfded-236">Testing Object Serialization</span></span>

<span data-ttu-id="cfded-237">Lorsque vous concevez votre API web, il est utile tester la façon dont vos objets de données seront sérialisés.</span><span class="sxs-lookup"><span data-stu-id="cfded-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="cfded-238">Pour cela, sans création d’un contrôleur ou d’appel d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="cfded-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
