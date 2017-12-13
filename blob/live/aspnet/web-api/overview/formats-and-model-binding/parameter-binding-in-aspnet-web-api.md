---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "Paramètre de liaison dans l’API Web ASP.NET | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ad052570fb2f168da657cd1263d8342a59d4cab0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="bbf31-102">Paramètre de liaison dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bbf31-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="bbf31-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bbf31-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bbf31-104">Lors de l’API Web appelle une méthode sur un contrôleur, il doit définir des valeurs pour les paramètres, un processus appelé *liaison*.</span><span class="sxs-lookup"><span data-stu-id="bbf31-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="bbf31-105">Cet article décrit la manière dont les API Web lie les paramètres, et comment vous pouvez personnaliser le processus de liaison.</span><span class="sxs-lookup"><span data-stu-id="bbf31-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="bbf31-106">Par défaut, les API Web utilise les règles suivantes pour lier les paramètres :</span><span class="sxs-lookup"><span data-stu-id="bbf31-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="bbf31-107">Si le paramètre est un type « simple », API Web essaie d’obtenir la valeur de l’URI.</span><span class="sxs-lookup"><span data-stu-id="bbf31-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="bbf31-108">Exemples de types simples .NET [les types primitifs](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, et ainsi de suite), ainsi que **TimeSpan**, **DateTime**, **Guid**, **décimal**, et **chaîne**, *plus* tout type avec un convertisseur de type qui peut être converti à partir d’une chaîne.</span><span class="sxs-lookup"><span data-stu-id="bbf31-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="bbf31-109">(Plus d’informations sur les convertisseurs de type plus tard.)</span><span class="sxs-lookup"><span data-stu-id="bbf31-109">(More about type converters later.)</span></span>
- <span data-ttu-id="bbf31-110">Pour les types complexes, API Web tente de lire la valeur du corps du message, à l’aide un [formateur de type de média](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="bbf31-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="bbf31-111">Par exemple, voici une méthode de contrôleur d’API Web classique :</span><span class="sxs-lookup"><span data-stu-id="bbf31-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="bbf31-112">Le *id* paramètre est un &quot;simple&quot; tapez, afin de l’API Web essaie d’obtenir la valeur de l’URI de requête.</span><span class="sxs-lookup"><span data-stu-id="bbf31-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="bbf31-113">Le *élément* paramètre est un type complexe, afin de l’API Web utilise un formateur de type de média à lire la valeur dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="bbf31-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="bbf31-114">Pour obtenir une valeur de l’URI, une API Web recherche dans les données d’itinéraire et de la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="bbf31-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="bbf31-115">Les données d’itinéraire sont remplies lorsque le système de routage analyse l’URI et l’associe à un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbf31-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="bbf31-116">Pour plus d’informations, consultez [routage et sélection de l’Action](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="bbf31-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="bbf31-117">Dans le reste de cet article, je vais montrer comment vous pouvez personnaliser le processus de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="bbf31-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="bbf31-118">Pour les types complexes, toutefois, envisagez d’utiliser les formateurs de type de média autant que possible.</span><span class="sxs-lookup"><span data-stu-id="bbf31-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="bbf31-119">Un principe clé du protocole HTTP est que les ressources sont envoyées dans le corps du message, à l’aide de la négociation de contenu pour spécifier la représentation sous forme de la ressource.</span><span class="sxs-lookup"><span data-stu-id="bbf31-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="bbf31-120">Formateurs de type de média ont été conçus dans ce but précis.</span><span class="sxs-lookup"><span data-stu-id="bbf31-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="bbf31-121">À l’aide de [FromUri]</span><span class="sxs-lookup"><span data-stu-id="bbf31-121">Using [FromUri]</span></span>

<span data-ttu-id="bbf31-122">Pour forcer l’API Web pour lire un type complexe à partir de l’URI, ajoutez le **[FromUri]** au paramètre.</span><span class="sxs-lookup"><span data-stu-id="bbf31-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="bbf31-123">L’exemple suivant définit un `GeoPoint` type, ainsi que d’une méthode de contrôleur qui obtient le `GeoPoint` à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="bbf31-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="bbf31-124">Le client peut placer les valeurs de Latitude et Longitude dans la chaîne de requête et API Web les utilise pour construire un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="bbf31-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="bbf31-125">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bbf31-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="bbf31-126">À l’aide de [FromBody]</span><span class="sxs-lookup"><span data-stu-id="bbf31-126">Using [FromBody]</span></span>

<span data-ttu-id="bbf31-127">Pour forcer l’API Web pour lire un type simple à partir du corps de la demande, vous devez ajouter le **[FromBody]** au paramètre :</span><span class="sxs-lookup"><span data-stu-id="bbf31-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="bbf31-128">Dans cet exemple, API Web utilisera un formateur de type de média à lire la valeur de *nom* à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="bbf31-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="bbf31-129">Voici un exemple de demande client.</span><span class="sxs-lookup"><span data-stu-id="bbf31-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="bbf31-130">Lorsqu’un paramètre a [FromBody], API Web utilise l’en-tête Content-Type pour sélectionner un module de formatage.</span><span class="sxs-lookup"><span data-stu-id="bbf31-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="bbf31-131">Dans cet exemple, le type de contenu est &quot;application/json&quot; et le corps de la demande est une chaîne JSON brute (pas un objet JSON).</span><span class="sxs-lookup"><span data-stu-id="bbf31-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="bbf31-132">Au moins un paramètre est autorisé à lire le corps du message.</span><span class="sxs-lookup"><span data-stu-id="bbf31-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="bbf31-133">Par conséquent, cela ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="bbf31-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="bbf31-134">La raison de cette règle est que le corps de la demande peut être stocké dans un flux non mis en mémoire tampon qui peut être lu uniquement une seule fois.</span><span class="sxs-lookup"><span data-stu-id="bbf31-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="bbf31-135">Convertisseurs de type</span><span class="sxs-lookup"><span data-stu-id="bbf31-135">Type Converters</span></span>

<span data-ttu-id="bbf31-136">Vous pouvez apporter les API Web traiter une classe comme un type simple (de sorte que les API Web tente de se lier à partir de l’URI) en créant un **TypeConverter** et en fournissant une conversion de chaîne.</span><span class="sxs-lookup"><span data-stu-id="bbf31-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="bbf31-137">Le code suivant illustre un `GeoPoint` classe qui représente un point géographique, plus une **TypeConverter** qui convertit des chaînes en `GeoPoint` instances.</span><span class="sxs-lookup"><span data-stu-id="bbf31-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="bbf31-138">Le `GeoPoint` classe est décorée avec un **[TypeConverter]** attribut pour spécifier le convertisseur de type.</span><span class="sxs-lookup"><span data-stu-id="bbf31-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="bbf31-139">(Cet exemple a été inspiré par billet de blog de Mike Stall [comment lier à des objets personnalisés dans les signatures d’action dans MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="bbf31-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="bbf31-140">Maintenant l’API Web traitera `GeoPoint` comme un type simple, ce qui signifie qu’il tente de se lier `GeoPoint` paramètres à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="bbf31-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="bbf31-141">Vous n’avez pas besoin d’inclure **[FromUri]** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="bbf31-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="bbf31-142">Le client peut appeler la méthode avec un URI comme suit :</span><span class="sxs-lookup"><span data-stu-id="bbf31-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="bbf31-143">Classeurs de modèles</span><span class="sxs-lookup"><span data-stu-id="bbf31-143">Model Binders</span></span>

<span data-ttu-id="bbf31-144">Une option plus souple à un convertisseur de type consiste à créer un classeur de modèles personnalisés.</span><span class="sxs-lookup"><span data-stu-id="bbf31-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="bbf31-145">Avec un classeur de modèles, vous avez accès à des éléments tels que la requête HTTP, la description de l’action et les valeurs brutes à partir des données d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bbf31-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="bbf31-146">Pour créer un classeur de modèles, vous devez implémenter la **classeur de modèles** interface.</span><span class="sxs-lookup"><span data-stu-id="bbf31-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="bbf31-147">Cette interface définit une méthode unique, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="bbf31-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="bbf31-148">Voici un classeur de modèles pour `GeoPoint` objets.</span><span class="sxs-lookup"><span data-stu-id="bbf31-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="bbf31-149">Un classeur de modèles Obtient les valeurs d’entrée brutes à partir d’un *fournisseur de valeur*.</span><span class="sxs-lookup"><span data-stu-id="bbf31-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="bbf31-150">Cette conception sépare les deux fonctions distinctes :</span><span class="sxs-lookup"><span data-stu-id="bbf31-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="bbf31-151">Le fournisseur de valeur prend la requête HTTP et remplit un dictionnaire de paires clé-valeur.</span><span class="sxs-lookup"><span data-stu-id="bbf31-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="bbf31-152">Le classeur de modèles utilise ce dictionnaire pour remplir le modèle.</span><span class="sxs-lookup"><span data-stu-id="bbf31-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="bbf31-153">Le fournisseur de valeur par défaut dans l’API Web obtient les valeurs de données d’itinéraire et de la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="bbf31-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="bbf31-154">Par exemple, si l’URI est `http://localhost/api/values/1?location=48,-122`, le fournisseur de valeur crée les paires clé-valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="bbf31-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="bbf31-155">ID = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="bbf31-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="bbf31-156">emplacement = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="bbf31-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="bbf31-157">(Je suis en supposant que le modèle d’itinéraire par défaut, qui est &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="bbf31-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="bbf31-158">Le nom du paramètre à lier est stocké dans le **ModelBindingContext.ModelName** propriété.</span><span class="sxs-lookup"><span data-stu-id="bbf31-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="bbf31-159">Le classeur de modèles recherche une clé avec cette valeur dans le dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="bbf31-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="bbf31-160">Si la valeur existe et qu’il peut être convertie en un `GeoPoint`, le classeur de modèles affecte la valeur liée à la **ModelBindingContext.Model** propriété.</span><span class="sxs-lookup"><span data-stu-id="bbf31-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="bbf31-161">Notez que le classeur de modèles n’est pas limité à une conversion de type simple.</span><span class="sxs-lookup"><span data-stu-id="bbf31-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="bbf31-162">Dans cet exemple, le classeur de modèles recherche tout d’abord dans une table des emplacements connus, et en cas d’échec, il utilise la conversion de type.</span><span class="sxs-lookup"><span data-stu-id="bbf31-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="bbf31-163">**Définir le classeur de modèles**</span><span class="sxs-lookup"><span data-stu-id="bbf31-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="bbf31-164">Il existe plusieurs façons de définir un classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="bbf31-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="bbf31-165">Tout d’abord, vous pouvez ajouter un **[ModelBinder]** au paramètre.</span><span class="sxs-lookup"><span data-stu-id="bbf31-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="bbf31-166">Vous pouvez également ajouter un **[ModelBinder]** pour le type d’attribut.</span><span class="sxs-lookup"><span data-stu-id="bbf31-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="bbf31-167">API Web utilise le classeur de modèles spécifié pour tous les paramètres de ce type.</span><span class="sxs-lookup"><span data-stu-id="bbf31-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="bbf31-168">Enfin, vous pouvez ajouter un fournisseur de classeur de modèles à la **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="bbf31-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="bbf31-169">Un fournisseur de classeur de modèles est simplement une classe de fabrique qui crée un classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="bbf31-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="bbf31-170">Vous pouvez créer un fournisseur en dérivant de la [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="bbf31-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="bbf31-171">Toutefois, si votre classeur de modèles gère un type unique, il est plus facile à utiliser la fonction intégrée **SimpleModelBinderProvider**, qui est conçu à cet effet.</span><span class="sxs-lookup"><span data-stu-id="bbf31-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="bbf31-172">Le code suivant montre comment procéder.</span><span class="sxs-lookup"><span data-stu-id="bbf31-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="bbf31-173">Avec un fournisseur de la liaison de modèle, vous devez toujours ajouter la **[ModelBinder]** au paramètre pour indiquer les API Web qu’il doit utiliser un classeur de modèles et pas un formateur de type de média.</span><span class="sxs-lookup"><span data-stu-id="bbf31-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="bbf31-174">Toutefois, vous ne devez désormais spécifier le type de classeur de modèles dans l’attribut :</span><span class="sxs-lookup"><span data-stu-id="bbf31-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="bbf31-175">Fournisseurs de valeur</span><span class="sxs-lookup"><span data-stu-id="bbf31-175">Value Providers</span></span>

<span data-ttu-id="bbf31-176">Un classeur de modèles Obtient des valeurs à partir d’un fournisseur de valeur indiqué.</span><span class="sxs-lookup"><span data-stu-id="bbf31-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="bbf31-177">Pour écrire un fournisseur de valeur personnalisée, vous devez implémenter la **IValueProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="bbf31-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="bbf31-178">Voici un exemple qui extrait les valeurs dans les cookies dans la demande :</span><span class="sxs-lookup"><span data-stu-id="bbf31-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="bbf31-179">Vous devez également créer une fabrique de fournisseur de valeur en dérivant de la **ValueProviderFactory** classe.</span><span class="sxs-lookup"><span data-stu-id="bbf31-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="bbf31-180">Ajouter la fabrique de fournisseur de valeur pour le **HttpConfiguration** comme suit.</span><span class="sxs-lookup"><span data-stu-id="bbf31-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="bbf31-181">API Web compose tous les fournisseurs de valeur, par conséquent, lorsqu’un classeur de modèles appelle **ValueProvider.GetValue**, le classeur de modèles reçoit la valeur du premier fournisseur de valeurs qui est en mesure de générer.</span><span class="sxs-lookup"><span data-stu-id="bbf31-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="bbf31-182">Vous pouvez également définir la fabrique de fournisseur de valeur au niveau du paramètre à l’aide de la **ValueProvider** d’attribut, comme suit :</span><span class="sxs-lookup"><span data-stu-id="bbf31-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="bbf31-183">Cela indique les API Web à utiliser la liaison de modèle avec la fabrique de fournisseur de valeur spécifiée et ne doit ne pas utiliser un des autres fournisseurs de valeur enregistrée.</span><span class="sxs-lookup"><span data-stu-id="bbf31-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="bbf31-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="bbf31-184">HttpParameterBinding</span></span>

<span data-ttu-id="bbf31-185">Classeurs de modèles sont une instance spécifique d’un mécanisme plus général.</span><span class="sxs-lookup"><span data-stu-id="bbf31-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="bbf31-186">Si vous examinez la **[ModelBinder]** attribut, vous verrez qu’il dérive abstraite **ParameterBindingAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="bbf31-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="bbf31-187">Cette classe définit une méthode unique, **GetBinding**, qui retourne un **HttpParameterBinding** objet :</span><span class="sxs-lookup"><span data-stu-id="bbf31-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="bbf31-188">Un **HttpParameterBinding** est responsable de la liaison d’un paramètre à une valeur.</span><span class="sxs-lookup"><span data-stu-id="bbf31-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="bbf31-189">Dans le cas de **[ModelBinder]**, l’attribut retourne un **HttpParameterBinding** implémentation utilise une **classeur de modèles** pour effectuer la liaison réelle.</span><span class="sxs-lookup"><span data-stu-id="bbf31-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="bbf31-190">Vous pouvez également implémenter votre propre **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="bbf31-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="bbf31-191">Par exemple, supposons que vous souhaitez obtenir l’eTag à partir `if-match` et `if-none-match` en-têtes dans la demande.</span><span class="sxs-lookup"><span data-stu-id="bbf31-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="bbf31-192">Nous allons commencer en définissant une classe pour représenter les ETags.</span><span class="sxs-lookup"><span data-stu-id="bbf31-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="bbf31-193">Nous allons également définir une énumération pour indiquer s’il faut obtenir l’ETag à partir de la `if-match` en-tête ou le `if-none-match` en-tête.</span><span class="sxs-lookup"><span data-stu-id="bbf31-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="bbf31-194">Voici une **HttpParameterBinding** qui obtient l’ETag à partir de l’en-tête de votre choix et le lie à un paramètre de type ETag :</span><span class="sxs-lookup"><span data-stu-id="bbf31-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="bbf31-195">Le **ExecuteBindingAsync** méthode effectue la liaison.</span><span class="sxs-lookup"><span data-stu-id="bbf31-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="bbf31-196">Dans cette méthode, ajoutez la valeur de paramètre lié à la **ActionArgument** dictionnaire dans le **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="bbf31-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="bbf31-197">Si votre **ExecuteBindingAsync** méthode lit le corps du message de demande, remplacez le **WillReadBody** propriété pour retourner true.</span><span class="sxs-lookup"><span data-stu-id="bbf31-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="bbf31-198">Le corps de la demande peut être un flux non tamponné qui ne peut être lu qu’une seule fois, afin de l’API Web applique une règle qu’au maximum une liaison peut lire le corps du message.</span><span class="sxs-lookup"><span data-stu-id="bbf31-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="bbf31-199">Pour appliquer une personnalisée **HttpParameterBinding**, vous pouvez définir un attribut qui dérive de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="bbf31-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="bbf31-200">Pour `ETagParameterBinding`, nous allons définir deux attributs, un pour `if-match` en-têtes et l’autre pour `if-none-match` en-têtes.</span><span class="sxs-lookup"><span data-stu-id="bbf31-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="bbf31-201">Les deux dérivent de la classe de base abstraite.</span><span class="sxs-lookup"><span data-stu-id="bbf31-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="bbf31-202">Voici une méthode de contrôleur qui utilise le `[IfNoneMatch]` attribut.</span><span class="sxs-lookup"><span data-stu-id="bbf31-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="bbf31-203">En outre **ParameterBindingAttribute**, il existe un autre crochet pour l’ajout d’un personnalisé **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="bbf31-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="bbf31-204">Sur le **HttpConfiguration** objet, le **ParameterBindingRules** propriété est une collection de fonctions anomymous de type (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="bbf31-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="bbf31-205">Par exemple, vous pouvez ajouter une règle qui utilise de n’importe quel paramètre ETag sur une méthode GET `ETagParameterBinding` avec `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="bbf31-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="bbf31-206">La fonction doit retourner `null` pour les paramètres lorsque la liaison n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="bbf31-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="bbf31-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="bbf31-207">IActionValueBinder</span></span>

<span data-ttu-id="bbf31-208">Tout le processus de liaison de paramètre est contrôlé par un service enfichable, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="bbf31-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="bbf31-209">L’implémentation par défaut de **IActionValueBinder** effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="bbf31-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="bbf31-210">Recherchez un **ParameterBindingAttribute** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="bbf31-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="bbf31-211">Cela inclut les **[FromBody]**, **[FromUri]**, et **[ModelBinder]**, ou les attributs personnalisés.</span><span class="sxs-lookup"><span data-stu-id="bbf31-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="bbf31-212">Sinon, recherchez dans **HttpConfiguration.ParameterBindingRules** pour une fonction qui retourne une valeur non null **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="bbf31-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="bbf31-213">Sinon, utilisez les règles par défaut décrite précédemment.</span><span class="sxs-lookup"><span data-stu-id="bbf31-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="bbf31-214">Si le type de paramètre est « simple » ou un convertisseur de type, lier à partir de l’URI.</span><span class="sxs-lookup"><span data-stu-id="bbf31-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="bbf31-215">Cela équivaut à placer le **[FromUri]** attribut sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="bbf31-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="bbf31-216">Sinon, essayez de lire le paramètre dans le corps du message.</span><span class="sxs-lookup"><span data-stu-id="bbf31-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="bbf31-217">Cela équivaut à placer **[FromBody]** sur le paramètre.</span><span class="sxs-lookup"><span data-stu-id="bbf31-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="bbf31-218">Si vous le souhaitez, vous pouvez remplacer l’ensemble de **IActionValueBinder** service avec une implémentation personnalisée.</span><span class="sxs-lookup"><span data-stu-id="bbf31-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bbf31-219">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bbf31-219">Additional Resources</span></span>

[<span data-ttu-id="bbf31-220">Exemple de liaison de paramètre personnalisé</span><span class="sxs-lookup"><span data-stu-id="bbf31-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="bbf31-221">Mike Stall écrit une série de bon de billets de blog sur la liaison de paramètre d’API Web :</span><span class="sxs-lookup"><span data-stu-id="bbf31-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="bbf31-222">Fonctionnement des API Web sur la liaison de paramètre</span><span class="sxs-lookup"><span data-stu-id="bbf31-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="bbf31-223">Liaison de paramètre de Style de MVC pour API Web</span><span class="sxs-lookup"><span data-stu-id="bbf31-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="bbf31-224">La liaison à des objets personnalisés dans les signatures d’action dans l’API MVC/Web</span><span class="sxs-lookup"><span data-stu-id="bbf31-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="bbf31-225">Comment créer un fournisseur de valeur personnalisée dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="bbf31-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="bbf31-226">Liaison de paramètre d’API Web sous le capot</span><span class="sxs-lookup"><span data-stu-id="bbf31-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
