---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Sérialisation JSON et XML dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400919"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="33064-102">Sérialisation JSON et XML dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="33064-102">JSON and XML Serialization in ASP.NET Web API</span></span>
====================
<span data-ttu-id="33064-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="33064-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="33064-104">Cet article décrit les formateurs JSON et XML dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="33064-104">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="33064-105">Dans l’API Web ASP.NET, un *formateur de type de média* est un objet qui peut :</span><span class="sxs-lookup"><span data-stu-id="33064-105">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="33064-106">Objets CLR de la lecture à partir de HTTP de corps du message</span><span class="sxs-lookup"><span data-stu-id="33064-106">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="33064-107">Écrire des objets CLR dans un corps de message HTTP</span><span class="sxs-lookup"><span data-stu-id="33064-107">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="33064-108">API Web fournit les formateurs de type de média pour JSON et XML.</span><span class="sxs-lookup"><span data-stu-id="33064-108">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="33064-109">Le framework insère ces formateurs dans le pipeline par défaut.</span><span class="sxs-lookup"><span data-stu-id="33064-109">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="33064-110">Les clients peuvent demander au format JSON ou XML dans l’en-tête Accept de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="33064-110">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="33064-111">Sommaire</span><span class="sxs-lookup"><span data-stu-id="33064-111">Contents</span></span>

- [<span data-ttu-id="33064-112">Formateur de Type de média JSON</span><span class="sxs-lookup"><span data-stu-id="33064-112">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="33064-113">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="33064-113">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="33064-114">Dates</span><span class="sxs-lookup"><span data-stu-id="33064-114">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="33064-115">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="33064-115">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="33064-116">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="33064-116">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="33064-117">Objets anonymes et faiblement typé</span><span class="sxs-lookup"><span data-stu-id="33064-117">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="33064-118">Formateur de Type de média XML</span><span class="sxs-lookup"><span data-stu-id="33064-118">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="33064-119">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="33064-119">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="33064-120">Dates</span><span class="sxs-lookup"><span data-stu-id="33064-120">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="33064-121">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="33064-121">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="33064-122">Sérialiseurs XML par Type de paramètre</span><span class="sxs-lookup"><span data-stu-id="33064-122">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="33064-123">Supprimer le JSON ou un formateur XML</span><span class="sxs-lookup"><span data-stu-id="33064-123">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="33064-124">Gestion des références d’objet circulaires</span><span class="sxs-lookup"><span data-stu-id="33064-124">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="33064-125">Sérialisation des objets de test</span><span class="sxs-lookup"><span data-stu-id="33064-125">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="33064-126">Formateur de Type de média JSON</span><span class="sxs-lookup"><span data-stu-id="33064-126">JSON Media-Type Formatter</span></span>

<span data-ttu-id="33064-127">Mise en forme de JSON est fournie par le **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="33064-127">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="33064-128">Par défaut, **JsonMediaTypeFormatter** utilise le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) bibliothèque pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="33064-128">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="33064-129">Json.NET est un projet open source de tiers.</span><span class="sxs-lookup"><span data-stu-id="33064-129">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="33064-130">Si vous préférez, vous pouvez configurer le **JsonMediaTypeFormatter** classe à utiliser le **DataContractJsonSerializer** au lieu de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="33064-130">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="33064-131">Pour ce faire, définissez la **UseDataContractJsonSerializer** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="33064-131">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="33064-132">Sérialisation JSON</span><span class="sxs-lookup"><span data-stu-id="33064-132">JSON Serialization</span></span>

<span data-ttu-id="33064-133">Cette section décrit certains des comportements spécifiques du module de formatage JSON, à l’aide de la valeur par défaut [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) sérialiseur.</span><span class="sxs-lookup"><span data-stu-id="33064-133">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="33064-134">Cela n’est pas censée être une documentation complète de la bibliothèque de Json.NET ; Pour plus d’informations, consultez le [Documentation Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="33064-134">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="33064-135">Éléments sérialisés ?</span><span class="sxs-lookup"><span data-stu-id="33064-135">What Gets Serialized?</span></span>

<span data-ttu-id="33064-136">Par défaut, toutes les propriétés et champs publics sont inclus dans le JSON sérialisé.</span><span class="sxs-lookup"><span data-stu-id="33064-136">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="33064-137">Pour omettre une propriété ou un champ, la décorer avec le **JsonIgnore** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-137">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="33064-138">Si vous préférez un &quot;participer&quot; approche, décorer la classe avec le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-138">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="33064-139">Si cet attribut est présent, les membres sont ignorés sauf s’ils ont le **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="33064-139">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="33064-140">Vous pouvez également utiliser **DataMember** pour sérialiser des membres privés.</span><span class="sxs-lookup"><span data-stu-id="33064-140">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="33064-141">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="33064-141">Read-Only Properties</span></span>

<span data-ttu-id="33064-142">Propriétés en lecture seule sont sérialisées par défaut.</span><span class="sxs-lookup"><span data-stu-id="33064-142">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="33064-143">Dates</span><span class="sxs-lookup"><span data-stu-id="33064-143">Dates</span></span>

<span data-ttu-id="33064-144">Par défaut, Json.NET écrit des dates [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span><span class="sxs-lookup"><span data-stu-id="33064-144">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="33064-145">Les dates au format UTC (Coordinated Universal Time) sont écrites avec un suffixe « Z ».</span><span class="sxs-lookup"><span data-stu-id="33064-145">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="33064-146">Dates en heure locale incluent un décalage de fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="33064-146">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="33064-147">Exemple :</span><span class="sxs-lookup"><span data-stu-id="33064-147">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="33064-148">Par défaut, Json.NET conserve le fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="33064-148">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="33064-149">Vous pouvez le remplacer en définissant la propriété DateTimeZoneHandling :</span><span class="sxs-lookup"><span data-stu-id="33064-149">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="33064-150">Si vous préférez utiliser [format de date Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu de la norme ISO 8601, définir le **DateFormatHandling** propriété sur les paramètres du sérialiseur :</span><span class="sxs-lookup"><span data-stu-id="33064-150">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="33064-151">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="33064-151">Indenting</span></span>

<span data-ttu-id="33064-152">Pour écrire le JSON mis en retrait, définissez le **mise en forme** à **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="33064-152">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="33064-153">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="33064-153">Camel Casing</span></span>

<span data-ttu-id="33064-154">Pour écrire des noms de propriété JSON avec une casse mixte, sans modifier votre modèle de données, définissez la **CamelCasePropertyNamesContractResolver** sur le sérialiseur :</span><span class="sxs-lookup"><span data-stu-id="33064-154">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="33064-155">Objets anonymes et faiblement typé</span><span class="sxs-lookup"><span data-stu-id="33064-155">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="33064-156">Une méthode d’action peut retourner un objet anonyme et sa sérialisation au format JSON.</span><span class="sxs-lookup"><span data-stu-id="33064-156">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="33064-157">Exemple :</span><span class="sxs-lookup"><span data-stu-id="33064-157">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="33064-158">Le corps du message de réponse contient le code JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="33064-158">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="33064-159">Si votre site web API reçoit faiblement structurées objets JSON à partir de clients, vous pouvez désérialiser le corps de la demande à un **Newtonsoft.Json.Linq.JObject** type.</span><span class="sxs-lookup"><span data-stu-id="33064-159">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="33064-160">Toutefois, il est généralement préférable d’utiliser des objets de données fortement typées.</span><span class="sxs-lookup"><span data-stu-id="33064-160">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="33064-161">Puis vous n’avez pas besoin analyser les données vous-même, et vous offre les avantages de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="33064-161">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="33064-162">Le sérialiseur XML ne prend pas en charge les types anonymes ou **JObject** instances.</span><span class="sxs-lookup"><span data-stu-id="33064-162">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="33064-163">Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML provenant du pipeline, comme décrit plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="33064-163">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="33064-164">Formateur de Type de média XML</span><span class="sxs-lookup"><span data-stu-id="33064-164">XML Media-Type Formatter</span></span>

<span data-ttu-id="33064-165">Mise en forme XML est fournie par le **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="33064-165">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="33064-166">Par défaut, **XmlMediaTypeFormatter** utilise le **DataContractSerializer** classe pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="33064-166">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="33064-167">Si vous préférez, vous pouvez configurer le **XmlMediaTypeFormatter** à utiliser le **XmlSerializer** au lieu du **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="33064-167">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="33064-168">Pour ce faire, définissez la **/usexmlserializer** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="33064-168">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="33064-169">Le **XmlSerializer** classe prend en charge un ensemble plus restreint de types que **DataContractSerializer**, mais offre davantage de contrôle sur le document XML obtenu.</span><span class="sxs-lookup"><span data-stu-id="33064-169">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="33064-170">Envisagez d’utiliser **XmlSerializer** si vous avez besoin faire correspondre un schéma XML existant.</span><span class="sxs-lookup"><span data-stu-id="33064-170">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="33064-171">Sérialisation XML</span><span class="sxs-lookup"><span data-stu-id="33064-171">XML Serialization</span></span>

<span data-ttu-id="33064-172">Cette section décrit certains des comportements spécifiques du formateur XML, à l’aide de la valeur par défaut **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="33064-172">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="33064-173">Par défaut, le DataContractSerializer se comporte comme suit :</span><span class="sxs-lookup"><span data-stu-id="33064-173">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="33064-174">Tous les champs et propriétés de lecture/écriture publique sont sérialisés.</span><span class="sxs-lookup"><span data-stu-id="33064-174">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="33064-175">Pour omettre une propriété ou un champ, la décorer avec le **IgnoreDataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-175">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="33064-176">Membres privés et protégés ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="33064-176">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="33064-177">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="33064-177">Read-only properties are not serialized.</span></span> <span data-ttu-id="33064-178">(Toutefois, le contenu d’une propriété de collection en lecture seule est sérialisé.)</span><span class="sxs-lookup"><span data-stu-id="33064-178">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="33064-179">Noms de membre et de classe sont écrits dans le code XML exactement comme elles apparaissent dans la déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="33064-179">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="33064-180">Un espace de noms XML par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="33064-180">A default XML namespace is used.</span></span>

<span data-ttu-id="33064-181">Si vous avez besoin de mieux contrôler la sérialisation, vous pouvez décorer la classe avec le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-181">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="33064-182">Lorsque cet attribut est présent, la classe est sérialisée comme suit :</span><span class="sxs-lookup"><span data-stu-id="33064-182">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="33064-183">&quot;Participer&quot; approche : propriétés et les champs ne sont pas sérialisés par défaut.</span><span class="sxs-lookup"><span data-stu-id="33064-183">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="33064-184">Pour sérialiser une propriété ou un champ, la décorer avec le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-184">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="33064-185">Pour sérialiser un membre privé ou protégé, la décorer avec le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-185">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="33064-186">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="33064-186">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="33064-187">Pour modifier la façon dont le nom de classe s’affiche dans le code XML, définissez le *nom* paramètre dans le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-187">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="33064-188">Pour modifier la façon dont un nom de membre s’affiche dans le code XML, définissez le *nom* paramètre dans le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-188">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="33064-189">Pour modifier l’espace de noms XML, définissez le *Namespace* paramètre dans le **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="33064-189">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="33064-190">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="33064-190">Read-Only Properties</span></span>

<span data-ttu-id="33064-191">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="33064-191">Read-only properties are not serialized.</span></span> <span data-ttu-id="33064-192">Si une propriété en lecture seule a un champ de stockage privé, vous pouvez marquer le champ privé avec les **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="33064-192">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="33064-193">Cette approche nécessite la **DataContract** attribut sur la classe.</span><span class="sxs-lookup"><span data-stu-id="33064-193">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="33064-194">Dates</span><span class="sxs-lookup"><span data-stu-id="33064-194">Dates</span></span>

<span data-ttu-id="33064-195">Les dates sont écrites au format ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="33064-195">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="33064-196">Par exemple, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="33064-196">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="33064-197">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="33064-197">Indenting</span></span>

<span data-ttu-id="33064-198">Pour écrire du code XML mis en retrait, définir le **mettre en retrait** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="33064-198">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="33064-199">Sérialiseurs XML par Type de paramètre</span><span class="sxs-lookup"><span data-stu-id="33064-199">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="33064-200">Vous pouvez définir différents sérialiseurs XML pour les différents types CLR.</span><span class="sxs-lookup"><span data-stu-id="33064-200">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="33064-201">Par exemple, vous pouvez avoir un objet de données particulière nécessite **XmlSerializer** pour la compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="33064-201">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="33064-202">Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.</span><span class="sxs-lookup"><span data-stu-id="33064-202">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="33064-203">Pour définir un sérialiseur XML pour un type particulier, appelez **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="33064-203">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="33064-204">Vous pouvez spécifier un **XmlSerializer** ou n’importe quel objet qui dérive de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="33064-204">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="33064-205">Supprimer le JSON ou un formateur XML</span><span class="sxs-lookup"><span data-stu-id="33064-205">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="33064-206">Vous pouvez supprimer le formateur JSON ou le formateur XML à partir de la liste des formateurs, si vous ne souhaitez pas les utiliser.</span><span class="sxs-lookup"><span data-stu-id="33064-206">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="33064-207">Les principales raisons à cela sont :</span><span class="sxs-lookup"><span data-stu-id="33064-207">The main reasons to do this are:</span></span>

- <span data-ttu-id="33064-208">Pour restreindre les réponses de l’API web à un type de média spécifique.</span><span class="sxs-lookup"><span data-stu-id="33064-208">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="33064-209">Par exemple, vous pouvez décider prendre en charge uniquement les réponses JSON et supprimer le formateur XML.</span><span class="sxs-lookup"><span data-stu-id="33064-209">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="33064-210">Pour remplacer le formateur par défaut par un formateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="33064-210">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="33064-211">Par exemple, vous pouvez remplacer le formateur JSON avec votre propre implémentation personnalisée d’un module de formatage JSON.</span><span class="sxs-lookup"><span data-stu-id="33064-211">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="33064-212">Le code suivant montre comment supprimer les formateurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="33064-212">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="33064-213">Appeler à partir votre **Application\_Démarrer** méthode, définie dans Global.asax.</span><span class="sxs-lookup"><span data-stu-id="33064-213">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="33064-214">Gestion des références d’objet circulaires</span><span class="sxs-lookup"><span data-stu-id="33064-214">Handling Circular Object References</span></span>

<span data-ttu-id="33064-215">Par défaut, les formateurs JSON et XML écrivent tous les objets en tant que valeurs.</span><span class="sxs-lookup"><span data-stu-id="33064-215">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="33064-216">Si les deux propriétés font référence au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialise l’objet à deux reprises.</span><span class="sxs-lookup"><span data-stu-id="33064-216">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="33064-217">Il s’agit une problématique en particulier si votre graphique d’objet contient des cycles, étant donné que le sérialiseur lève une exception lorsqu’il détecte une boucle dans le graphique.</span><span class="sxs-lookup"><span data-stu-id="33064-217">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="33064-218">Tenez compte des modèles d’objet suivants et contrôleur.</span><span class="sxs-lookup"><span data-stu-id="33064-218">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="33064-219">Appel de cette action provoque le formateur à levé une exception, ce qui se traduit par un état code 500 (erreur interne du serveur) réponse au client.</span><span class="sxs-lookup"><span data-stu-id="33064-219">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="33064-220">Pour conserver les références d’objet dans JSON, ajoutez le code suivant à **Application\_Démarrer** méthode dans le fichier Global.asax :</span><span class="sxs-lookup"><span data-stu-id="33064-220">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="33064-221">L’action du contrôleur retournent désormais JSON ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="33064-221">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="33064-222">Notez que le sérialiseur ajoute un &quot;$id&quot; propriété pour les deux objets.</span><span class="sxs-lookup"><span data-stu-id="33064-222">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="33064-223">En outre, il détecte que la propriété Employee.Department crée une boucle, elle remplace la valeur par une référence d’objet : {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="33064-223">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="33064-224">Références d’objet ne sont pas standard au format JSON.</span><span class="sxs-lookup"><span data-stu-id="33064-224">Object references are not standard in JSON.</span></span> <span data-ttu-id="33064-225">Avant d’utiliser cette fonctionnalité, réfléchissez aux si vos clients seront en mesure d’analyser les résultats.</span><span class="sxs-lookup"><span data-stu-id="33064-225">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="33064-226">Il peut être préférable de ne supprimer cycles du graphique.</span><span class="sxs-lookup"><span data-stu-id="33064-226">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="33064-227">Par exemple, le lien d’employé au département n’est pas vraiment nécessaire dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="33064-227">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="33064-228">Pour conserver les références d’objet dans XML, vous avez deux options.</span><span class="sxs-lookup"><span data-stu-id="33064-228">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="33064-229">L’option la plus simple consiste à ajouter `[DataContract(IsReference=true)]` à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="33064-229">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="33064-230">Le *IsReference* paramètre permet de références d’objet.</span><span class="sxs-lookup"><span data-stu-id="33064-230">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="33064-231">N’oubliez pas que **DataContract** rend sérialisation participer, donc vous devrez également ajouter **DataMember** attributs aux propriétés :</span><span class="sxs-lookup"><span data-stu-id="33064-231">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="33064-232">Maintenant le formateur produira XML semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="33064-232">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="33064-233">Si vous souhaitez éviter les attributs sur votre classe de modèle, il existe une autre option : créer un nouveau spécifiques au type **DataContractSerializer** d’instance et définissez *preserveObjectReferences* à **true**  dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="33064-233">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="33064-234">Puis définissez cette instance comme un sérialiseur par type sur le formateur de type de média XML.</span><span class="sxs-lookup"><span data-stu-id="33064-234">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="33064-235">Le code suivant montre comment procéder :</span><span class="sxs-lookup"><span data-stu-id="33064-235">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="33064-236">Sérialisation des objets de test</span><span class="sxs-lookup"><span data-stu-id="33064-236">Testing Object Serialization</span></span>

<span data-ttu-id="33064-237">Lorsque vous concevez votre API web, il est utile tester comment vos objets de données seront sérialisés.</span><span class="sxs-lookup"><span data-stu-id="33064-237">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="33064-238">Vous pouvez faire cela sans création d’un contrôleur ou d’appel d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="33064-238">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
