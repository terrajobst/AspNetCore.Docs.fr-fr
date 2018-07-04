---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Liaison de paramètre dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377807"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="4d508-102">Liaison de paramètre dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4d508-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="4d508-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4d508-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4d508-104">Lors de l’API Web appelle une méthode sur un contrôleur, il doit définir des valeurs pour les paramètres, un processus appelé *liaison*.</span><span class="sxs-lookup"><span data-stu-id="4d508-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="4d508-105">Cet article décrit la manière dont les API Web lie les paramètres, et comment vous pouvez personnaliser le processus de liaison.</span><span class="sxs-lookup"><span data-stu-id="4d508-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="4d508-106">Par défaut, les API Web utilise les règles suivantes pour lier les paramètres :</span><span class="sxs-lookup"><span data-stu-id="4d508-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="4d508-107">Si le paramètre est un type « simple », API Web tente d’obtenir la valeur de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4d508-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="4d508-108">Exemples de types simples .NET [types primitifs](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, et ainsi de suite), ainsi que **TimeSpan**, **DateTime**, **Guid**, **décimal**, et **chaîne**, *plus* tout type avec un convertisseur de type qui peut être converti à partir d’une chaîne.</span><span class="sxs-lookup"><span data-stu-id="4d508-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="4d508-109">(Plus d’informations sur les convertisseurs de type ultérieurement.)</span><span class="sxs-lookup"><span data-stu-id="4d508-109">(More about type converters later.)</span></span>
- <span data-ttu-id="4d508-110">Pour les types complexes, les API Web tente de lire la valeur du corps du message, à l’aide un [formateur de type de média](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="4d508-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="4d508-111">Par exemple, voici une méthode de contrôleur d’API Web typique :</span><span class="sxs-lookup"><span data-stu-id="4d508-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="4d508-112">Le *id* paramètre est un &quot;simple&quot; type, afin de l’API Web tente d’obtenir la valeur de l’URI de demande.</span><span class="sxs-lookup"><span data-stu-id="4d508-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="4d508-113">Le *élément* paramètre est un type complexe, afin de l’API Web utilise un formateur de type de média pour lire la valeur à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="4d508-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="4d508-114">Pour obtenir une valeur à partir de l’URI, Web API recherche dans les données d’itinéraire et la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="4d508-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="4d508-115">Les données d’itinéraire sont remplies lorsque le système de routage analyse l’URI et la fait correspondre à un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="4d508-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="4d508-116">Pour plus d’informations, consultez [routage et sélection d’Action](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="4d508-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="4d508-117">Dans le reste de cet article, vous expliquerai comment vous pouvez personnaliser le processus de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="4d508-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="4d508-118">Pour les types complexes, toutefois, envisagez d’utiliser des formateurs de type de média autant que possible.</span><span class="sxs-lookup"><span data-stu-id="4d508-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="4d508-119">Un principe clé du protocole HTTP est que les ressources sont envoyés dans le corps du message, à l’aide de la négociation de contenu pour spécifier la représentation sous forme de la ressource.</span><span class="sxs-lookup"><span data-stu-id="4d508-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="4d508-120">Formateurs de type de média ont été conçus dans ce but précis.</span><span class="sxs-lookup"><span data-stu-id="4d508-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="4d508-121">À l’aide de [FromUri]</span><span class="sxs-lookup"><span data-stu-id="4d508-121">Using [FromUri]</span></span>

<span data-ttu-id="4d508-122">Pour forcer l’API Web pour lire un type complexe à partir de l’URI, ajoutez le **[FromUri]** au paramètre d’attribut.</span><span class="sxs-lookup"><span data-stu-id="4d508-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="4d508-123">L’exemple suivant définit un `GeoPoint` type, ainsi que d’une méthode de contrôleur qui obtient le `GeoPoint` à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4d508-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="4d508-124">Le client peut placer les valeurs de Latitude et Longitude dans la chaîne de requête et API Web les utilise pour construire un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="4d508-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="4d508-125">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4d508-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="4d508-126">À l’aide de [FromBody]</span><span class="sxs-lookup"><span data-stu-id="4d508-126">Using [FromBody]</span></span>

<span data-ttu-id="4d508-127">Pour forcer l’API Web pour lire un type simple à partir du corps de demande, ajoutez le **[FromBody]** au paramètre d’attribut :</span><span class="sxs-lookup"><span data-stu-id="4d508-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="4d508-128">Dans cet exemple, API Web utilisera un formateur de type de média à lire la valeur de *nom* corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="4d508-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="4d508-129">Voici un exemple de demande client.</span><span class="sxs-lookup"><span data-stu-id="4d508-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="4d508-130">Lorsqu’un paramètre a [FromBody], API Web utilise l’en-tête Content-Type pour sélectionner un formateur.</span><span class="sxs-lookup"><span data-stu-id="4d508-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="4d508-131">Dans cet exemple, le type de contenu est &quot;application/json&quot; et le corps de la demande est une chaîne JSON brute (pas un objet JSON).</span><span class="sxs-lookup"><span data-stu-id="4d508-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="4d508-132">Au moins un paramètre est autorisé à lire à partir du corps du message.</span><span class="sxs-lookup"><span data-stu-id="4d508-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="4d508-133">Par conséquent, cela ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="4d508-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="4d508-134">La raison de cette règle est que le corps de la demande peut être stocké dans un flux non mis en mémoire tampon qui peut uniquement être lu qu’une seule fois.</span><span class="sxs-lookup"><span data-stu-id="4d508-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="4d508-135">Convertisseurs de type</span><span class="sxs-lookup"><span data-stu-id="4d508-135">Type Converters</span></span>

<span data-ttu-id="4d508-136">Vous pouvez rendre les API Web considérer une classe comme un type simple (afin que les API Web essaie de lier à partir de l’URI) en créant un **TypeConverter** et en fournissant une conversion de chaîne.</span><span class="sxs-lookup"><span data-stu-id="4d508-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="4d508-137">Le code suivant montre un `GeoPoint` classe qui représente un point géographique, plus une **TypeConverter** qui convertit des chaînes en `GeoPoint` instances.</span><span class="sxs-lookup"><span data-stu-id="4d508-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="4d508-138">Le `GeoPoint` classe est décorée avec un **[TypeConverter]** attribut pour spécifier le convertisseur de type.</span><span class="sxs-lookup"><span data-stu-id="4d508-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="4d508-139">(Cet exemple a été inspiré par billet de blog de Mike Stall [comment lier à des objets personnalisés dans les signatures d’action dans MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="4d508-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="4d508-140">Maintenant l’API Web traitera `GeoPoint` comme un type simple, ce qui signifie qu’il tente de lier `GeoPoint` paramètres à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4d508-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="4d508-141">Vous n’avez pas besoin d’inclure **[FromUri]** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="4d508-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="4d508-142">Le client peut appeler la méthode avec un URI comme ceci :</span><span class="sxs-lookup"><span data-stu-id="4d508-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="4d508-143">Classeurs de modèles</span><span class="sxs-lookup"><span data-stu-id="4d508-143">Model Binders</span></span>

<span data-ttu-id="4d508-144">Une option plus flexible qu’un convertisseur de type consiste à créer un classeur de modèles personnalisés.</span><span class="sxs-lookup"><span data-stu-id="4d508-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="4d508-145">Avec un classeur de modèles, vous avez accès à des éléments tels que la requête HTTP, la description de l’action et les valeurs brutes à partir des données d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="4d508-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="4d508-146">Pour créer un classeur de modèles, implémentez le **IModelBinder** interface.</span><span class="sxs-lookup"><span data-stu-id="4d508-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="4d508-147">Cette interface définit une méthode unique, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="4d508-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="4d508-148">Voici un binder de modèle pour `GeoPoint` objets.</span><span class="sxs-lookup"><span data-stu-id="4d508-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="4d508-149">Un classeur de modèles Obtient des valeurs d’entrée brutes à partir d’un *fournisseur de valeurs*.</span><span class="sxs-lookup"><span data-stu-id="4d508-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="4d508-150">Cette conception sépare les deux fonctions distinctes :</span><span class="sxs-lookup"><span data-stu-id="4d508-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="4d508-151">Le fournisseur de valeur prend la requête HTTP et remplit un dictionnaire de paires clé-valeur.</span><span class="sxs-lookup"><span data-stu-id="4d508-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="4d508-152">Le classeur de modèles utilise ce dictionnaire pour remplir le modèle.</span><span class="sxs-lookup"><span data-stu-id="4d508-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="4d508-153">Le fournisseur de valeur par défaut dans l’API Web obtient les valeurs de données d’itinéraire et la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="4d508-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="4d508-154">Par exemple, si l’URI est `http://localhost/api/values/1?location=48,-122`, le fournisseur de valeurs crée les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="4d508-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="4d508-155">ID = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="4d508-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="4d508-156">emplacement = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="4d508-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="4d508-157">(Je suppose que le modèle d’itinéraire par défaut, qui est &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="4d508-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="4d508-158">Le nom du paramètre à lier est stocké dans le **ModelBindingContext.ModelName** propriété.</span><span class="sxs-lookup"><span data-stu-id="4d508-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="4d508-159">Le binder de modèle recherche une clé avec cette valeur dans le dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="4d508-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="4d508-160">Si la valeur existe et peut être convertie en un `GeoPoint`, le binder de modèle affecte la valeur liée à la **ModelBindingContext.Model** propriété.</span><span class="sxs-lookup"><span data-stu-id="4d508-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="4d508-161">Notez que le binder de modèle n’est pas limité à une conversion de type simple.</span><span class="sxs-lookup"><span data-stu-id="4d508-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="4d508-162">Dans cet exemple, le binder de modèle recherche d’abord dans une table des emplacements connus, et si cette tentative échoue, il utilise la conversion de type.</span><span class="sxs-lookup"><span data-stu-id="4d508-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="4d508-163">**Définissant le Binder de modèle**</span><span class="sxs-lookup"><span data-stu-id="4d508-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="4d508-164">Il existe plusieurs façons de définir un classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="4d508-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="4d508-165">Tout d’abord, vous pouvez ajouter un **[ModelBinder]** au paramètre d’attribut.</span><span class="sxs-lookup"><span data-stu-id="4d508-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="4d508-166">Vous pouvez également ajouter un **[ModelBinder]** pour le type d’attribut.</span><span class="sxs-lookup"><span data-stu-id="4d508-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="4d508-167">API Web utilisera le binder de modèle spécifiée pour tous les paramètres de ce type.</span><span class="sxs-lookup"><span data-stu-id="4d508-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="4d508-168">Enfin, vous pouvez ajouter un fournisseur de classeur de modèles pour la **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="4d508-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="4d508-169">Un fournisseur de classeur de modèles est simplement une classe de fabrique qui crée un classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="4d508-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="4d508-170">Vous pouvez créer un fournisseur en dérivant le [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="4d508-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="4d508-171">Toutefois, si votre classeur de modèles gère un type unique, il est plus facile à utiliser la fonction intégrée **SimpleModelBinderProvider**, qui est conçu à cet effet.</span><span class="sxs-lookup"><span data-stu-id="4d508-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="4d508-172">Le code suivant montre comment procéder.</span><span class="sxs-lookup"><span data-stu-id="4d508-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="4d508-173">Avec un fournisseur de la liaison de modèle, vous devez encore y ajouter la **[ModelBinder]** au paramètre pour indiquer les API Web qu’il doit utiliser un classeur de modèles et pas un formateur de type de média.</span><span class="sxs-lookup"><span data-stu-id="4d508-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="4d508-174">Mais maintenant vous n’avez pas besoin de spécifier le type de classeur de modèles dans l’attribut :</span><span class="sxs-lookup"><span data-stu-id="4d508-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="4d508-175">Fournisseurs de valeurs</span><span class="sxs-lookup"><span data-stu-id="4d508-175">Value Providers</span></span>

<span data-ttu-id="4d508-176">J’ai mentionné qu’un binder de modèle obtient des valeurs à partir d’un fournisseur de valeur.</span><span class="sxs-lookup"><span data-stu-id="4d508-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="4d508-177">Pour écrire un fournisseur de valeur personnalisée, vous devez implémenter le **IValueProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="4d508-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="4d508-178">Voici un exemple qui extrait les valeurs dans les cookies dans la demande :</span><span class="sxs-lookup"><span data-stu-id="4d508-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="4d508-179">Vous devez également créer une fabrique de fournisseur de valeur en dérivant de la **ValueProviderFactory** classe.</span><span class="sxs-lookup"><span data-stu-id="4d508-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="4d508-180">Ajouter la fabrique de fournisseur de valeur pour le **HttpConfiguration** comme suit.</span><span class="sxs-lookup"><span data-stu-id="4d508-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="4d508-181">API Web compose tous les fournisseurs de valeur, par conséquent, lorsqu’un classeur de modèles appelle **ValueProvider.GetValue**, le binder de modèle reçoit la valeur du premier fournisseur de valeurs qui est en mesure de générer.</span><span class="sxs-lookup"><span data-stu-id="4d508-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="4d508-182">Vous pouvez également définir la fabrique de fournisseur de valeur au niveau du paramètre à l’aide de la **ValueProvider** d’attribut, comme suit :</span><span class="sxs-lookup"><span data-stu-id="4d508-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="4d508-183">Ce code indique à des API Web à utiliser la liaison de modèle avec la fabrique de fournisseur de valeur spécifiée et non à utiliser une des autres fournisseurs de valeurs enregistrés.</span><span class="sxs-lookup"><span data-stu-id="4d508-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="4d508-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="4d508-184">HttpParameterBinding</span></span>

<span data-ttu-id="4d508-185">Classeurs de modèles sont une instance spécifique d’un mécanisme plus général.</span><span class="sxs-lookup"><span data-stu-id="4d508-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="4d508-186">Si vous examinez le **[ModelBinder]** attribut, vous verrez qu’il dérive de la classe abstraite **ParameterBindingAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="4d508-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="4d508-187">Cette classe définit une méthode unique, **GetBinding**, qui retourne un **HttpParameterBinding** objet :</span><span class="sxs-lookup"><span data-stu-id="4d508-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="4d508-188">Un **HttpParameterBinding** est responsable de la liaison d’un paramètre à une valeur.</span><span class="sxs-lookup"><span data-stu-id="4d508-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="4d508-189">Dans le cas de **[ModelBinder]**, l’attribut retourne un **HttpParameterBinding** implémentation qui utilise un **IModelBinder** pour effectuer la liaison réelle.</span><span class="sxs-lookup"><span data-stu-id="4d508-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="4d508-190">Vous pouvez également implémenter votre propre **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="4d508-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="4d508-191">Par exemple, supposons que vous souhaitez obtenir l’eTag à partir `if-match` et `if-none-match` en-têtes dans la demande.</span><span class="sxs-lookup"><span data-stu-id="4d508-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="4d508-192">Nous allons commencer en définissant une classe pour représenter des ETags.</span><span class="sxs-lookup"><span data-stu-id="4d508-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="4d508-193">Nous allons également définir une énumération pour indiquer s’il faut obtenir l’ETag à partir de la `if-match` en-tête ou le `if-none-match` en-tête.</span><span class="sxs-lookup"><span data-stu-id="4d508-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="4d508-194">Voici une **HttpParameterBinding** qui obtient l’ETag à partir de l’en-tête de votre choix et le lie à un paramètre de type ETag :</span><span class="sxs-lookup"><span data-stu-id="4d508-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="4d508-195">Le **ExecuteBindingAsync** méthode effectue la liaison.</span><span class="sxs-lookup"><span data-stu-id="4d508-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="4d508-196">Dans cette méthode, ajoutez la valeur de paramètre lié à la **ActionArgument** dictionnaire dans le **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="4d508-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="4d508-197">Si votre **ExecuteBindingAsync** méthode lit le corps du message de demande, remplacez le **WillReadBody** propriété pour retourner true.</span><span class="sxs-lookup"><span data-stu-id="4d508-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="4d508-198">Le corps de la demande peut être un flux non tamponné peut être lu uniquement une seule fois, afin de l’API Web applique une règle qu’au maximum une liaison peut lire le corps du message.</span><span class="sxs-lookup"><span data-stu-id="4d508-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="4d508-199">Pour appliquer un personnalisé **HttpParameterBinding**, vous pouvez définir un attribut qui dérive de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="4d508-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="4d508-200">Pour `ETagParameterBinding`, nous allons définir deux attributs, un pour `if-match` en-têtes et l’autre pour `if-none-match` en-têtes.</span><span class="sxs-lookup"><span data-stu-id="4d508-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="4d508-201">Les deux dérivent de la classe de base abstraite.</span><span class="sxs-lookup"><span data-stu-id="4d508-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="4d508-202">Voici une méthode de contrôleur qui utilise le `[IfNoneMatch]` attribut.</span><span class="sxs-lookup"><span data-stu-id="4d508-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="4d508-203">En outre **ParameterBindingAttribute**, il existe un autre raccordement pour l’ajout d’un personnalisé **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="4d508-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="4d508-204">Sur le **HttpConfiguration** objet, le **ParameterBindingRules** propriété est une collection de fonctions anomymous de type (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="4d508-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="4d508-205">Par exemple, vous pouvez ajouter une règle qui utilise n’importe quel paramètre ETag sur une méthode GET `ETagParameterBinding` avec `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="4d508-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="4d508-206">La fonction doit retourner `null` pour les paramètres où la liaison n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="4d508-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="4d508-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="4d508-207">IActionValueBinder</span></span>

<span data-ttu-id="4d508-208">Tout le processus de liaison de paramètre est contrôlé par un service enfichable, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="4d508-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="4d508-209">L’implémentation par défaut de **IActionValueBinder** effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="4d508-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="4d508-210">Recherchez un **ParameterBindingAttribute** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="4d508-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="4d508-211">Cela inclut les **[FromBody]**, **[FromUri]**, et **[ModelBinder]**, ou les attributs personnalisés.</span><span class="sxs-lookup"><span data-stu-id="4d508-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="4d508-212">Sinon, recherchez dans **HttpConfiguration.ParameterBindingRules** pour une fonction qui retourne une valeur non null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="4d508-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="4d508-213">Sinon, utilisez les règles par défaut que j’ai décrites précédemment.</span><span class="sxs-lookup"><span data-stu-id="4d508-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="4d508-214">Si le type de paramètre est « simple » ou a un convertisseur de type, lier à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4d508-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="4d508-215">Cela équivaut à placer le **[FromUri]** attribut sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="4d508-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="4d508-216">Sinon, essayez de lire le paramètre du corps du message.</span><span class="sxs-lookup"><span data-stu-id="4d508-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="4d508-217">Cela équivaut à placer **[FromBody]** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="4d508-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="4d508-218">Si vous le souhaitiez, vous pourriez remplacer l’intégralité de **IActionValueBinder** service avec une implémentation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="4d508-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d508-219">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4d508-219">Additional Resources</span></span>

[<span data-ttu-id="4d508-220">Exemple de liaison de paramètre personnalisé</span><span class="sxs-lookup"><span data-stu-id="4d508-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="4d508-221">Mike Stall a écrit une bonne série de billets de blog sur la liaison de paramètre d’API Web :</span><span class="sxs-lookup"><span data-stu-id="4d508-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="4d508-222">Fonctionnement des API Web sur la liaison de paramètre</span><span class="sxs-lookup"><span data-stu-id="4d508-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="4d508-223">Liaison de paramètre de Style MVC pour API Web</span><span class="sxs-lookup"><span data-stu-id="4d508-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="4d508-224">Comment lier à des objets personnalisés dans les signatures d’action dans MVC/API Web</span><span class="sxs-lookup"><span data-stu-id="4d508-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="4d508-225">Comment créer un fournisseur de valeur personnalisée dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="4d508-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="4d508-226">Liaison de paramètre d’API Web sous le capot</span><span class="sxs-lookup"><span data-stu-id="4d508-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
