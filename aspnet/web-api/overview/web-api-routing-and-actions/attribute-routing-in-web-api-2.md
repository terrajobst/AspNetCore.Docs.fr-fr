---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attribut de routage dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 52164fde1a2277bb9ef07f2a5476ca78b71a8e52
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802755"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="bd96e-102">Routage par attributs dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bd96e-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bd96e-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bd96e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bd96e-104">*Routage* est comment l’API Web correspond à un URI à une action.</span><span class="sxs-lookup"><span data-stu-id="bd96e-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="bd96e-105">Web API 2 prend en charge un nouveau type de routage, appelé *routage par attributs*.</span><span class="sxs-lookup"><span data-stu-id="bd96e-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="bd96e-106">Comme son nom l’indique, le routage par attributs utilise les attributs pour définir des itinéraires.</span><span class="sxs-lookup"><span data-stu-id="bd96e-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="bd96e-107">Routage par attributs vous donne davantage de contrôle sur les URI dans votre API web.</span><span class="sxs-lookup"><span data-stu-id="bd96e-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="bd96e-108">Par exemple, vous pouvez facilement créer des URI qui décrivent les hiérarchies de ressources.</span><span class="sxs-lookup"><span data-stu-id="bd96e-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="bd96e-109">Le style antérieures de routage, appelé conventionnelle routage, est toujours intégralement pris en charge.</span><span class="sxs-lookup"><span data-stu-id="bd96e-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="bd96e-110">En fait, vous pouvez combiner ces deux techniques dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="bd96e-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="bd96e-111">Cette rubrique montre comment activer le routage par attributs et décrit les différentes options de routage par attributs.</span><span class="sxs-lookup"><span data-stu-id="bd96e-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="bd96e-112">Pour obtenir un didacticiel de bout en bout qui utilise le routage par attributs, consultez [créer une API REST avec le routage par attributs dans Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="bd96e-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="bd96e-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="bd96e-113">Prerequisites</span></span>

<span data-ttu-id="bd96e-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="bd96e-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="bd96e-115">Vous pouvez également utiliser Gestionnaire de Package NuGet pour installer les packages nécessaires.</span><span class="sxs-lookup"><span data-stu-id="bd96e-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="bd96e-116">À partir de la **outils** menu dans Visual Studio, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="bd96e-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bd96e-117">Entrez la commande suivante dans la fenêtre de Console du Gestionnaire de Package :</span><span class="sxs-lookup"><span data-stu-id="bd96e-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="bd96e-118">Pourquoi routage par attributs ?</span><span class="sxs-lookup"><span data-stu-id="bd96e-118">Why Attribute Routing?</span></span>

<span data-ttu-id="bd96e-119">La première version de l’API Web utilisé *conventionnelle* routage.</span><span class="sxs-lookup"><span data-stu-id="bd96e-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="bd96e-120">Dans ce type de routage, vous définissez un ou plusieurs modèles d’itinéraire, qui sont essentiellement des paramétrables de chaînes.</span><span class="sxs-lookup"><span data-stu-id="bd96e-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="bd96e-121">Lorsque l’infrastructure reçoit une demande, elle correspond à l’URI par rapport au modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="bd96e-122">(Pour plus d’informations sur le routage basé sur une convention, consultez [routage dans ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bd96e-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="bd96e-123">Un avantage du routage basé sur une convention est que les modèles sont définis dans un emplacement unique, et les règles de routage sont appliqués de manière cohérente sur tous les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="bd96e-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="bd96e-124">Malheureusement, le routage basé sur une convention rend difficile prendre en charge de certains modèles d’URI sont courantes dans les API RESTful.</span><span class="sxs-lookup"><span data-stu-id="bd96e-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="bd96e-125">Par exemple, ressources contiennent souvent des ressources enfants : les clients ont passé des commandes, films ont des acteurs, la documentation ont des auteurs et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="bd96e-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="bd96e-126">Il est naturel pour créer des URI qui reflètent ces relations :</span><span class="sxs-lookup"><span data-stu-id="bd96e-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="bd96e-127">Ce type d’URI est difficile de créer à l’aide du routage basé sur une convention.</span><span class="sxs-lookup"><span data-stu-id="bd96e-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="bd96e-128">Bien qu’il est possible, les résultats ne l’adaptons pas correctement si vous avez de nombreux contrôleurs ou les types de ressources.</span><span class="sxs-lookup"><span data-stu-id="bd96e-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="bd96e-129">Avec le routage par attributs, il est facile de définir un itinéraire pour cet URI.</span><span class="sxs-lookup"><span data-stu-id="bd96e-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="bd96e-130">Vous ajoutez simplement un attribut à l’action du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="bd96e-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="bd96e-131">Voici d’autres modèles de cet attribut routage rend facile.</span><span class="sxs-lookup"><span data-stu-id="bd96e-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="bd96e-132">**Contrôle de version**</span><span class="sxs-lookup"><span data-stu-id="bd96e-132">**API versioning**</span></span>

<span data-ttu-id="bd96e-133">Dans cet exemple, « / api/v1/products » serait routé vers un contrôleur différent de « / v2/api/produits ».</span><span class="sxs-lookup"><span data-stu-id="bd96e-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="bd96e-134">**Segments URI surchargés**</span><span class="sxs-lookup"><span data-stu-id="bd96e-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="bd96e-135">Dans cet exemple, « 1 » est un numéro de commande, mais « en attente » correspond à une collection.</span><span class="sxs-lookup"><span data-stu-id="bd96e-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="bd96e-136">**Plusieurs types de paramètres**</span><span class="sxs-lookup"><span data-stu-id="bd96e-136">**Multiple parameter types**</span></span>

<span data-ttu-id="bd96e-137">Dans cet exemple, « 1 » est un numéro de commande, mais « 2013/06/16 » spécifie une date.</span><span class="sxs-lookup"><span data-stu-id="bd96e-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="bd96e-138">L’activation de routage par attributs</span><span class="sxs-lookup"><span data-stu-id="bd96e-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="bd96e-139">Pour activer le routage par attributs, appelez **MapHttpAttributeRoutes** lors de la configuration.</span><span class="sxs-lookup"><span data-stu-id="bd96e-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="bd96e-140">Cette méthode d’extension est définie dans le **System.Web.Http.HttpConfigurationExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="bd96e-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="bd96e-141">Routage par attributs peut être combiné avec [conventionnelle](routing-in-aspnet-web-api.md) routage.</span><span class="sxs-lookup"><span data-stu-id="bd96e-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="bd96e-142">Pour définir des itinéraires reposant sur une convention, appelez le **MapHttpRoute** (méthode).</span><span class="sxs-lookup"><span data-stu-id="bd96e-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="bd96e-143">Pour plus d’informations sur la configuration des API Web, consultez [configuration ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bd96e-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="bd96e-144">Remarque : La migration à partir de Web API 1</span><span class="sxs-lookup"><span data-stu-id="bd96e-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="bd96e-145">Avant d’API Web 2, les modèles de projet API Web généré code similaire à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="bd96e-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="bd96e-146">Si le routage par attributs est activé, ce code lève une exception.</span><span class="sxs-lookup"><span data-stu-id="bd96e-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="bd96e-147">Si vous mettez à niveau un projet API Web existant pour utiliser le routage par attributs, veillez à mettre à jour de ce code de configuration à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="bd96e-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="bd96e-148">Pour plus d’informations, consultez [configuration des API Web avec hébergement ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="bd96e-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="bd96e-149">Ajout d’attributs de Route</span><span class="sxs-lookup"><span data-stu-id="bd96e-149">Adding Route Attributes</span></span>

<span data-ttu-id="bd96e-150">Voici un exemple d’un itinéraire défini à l’aide d’un attribut :</span><span class="sxs-lookup"><span data-stu-id="bd96e-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="bd96e-151">La chaîne &quot;clients / {customerId} / commandes&quot; est le modèle d’URI pour l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="bd96e-152">API Web a essaie de correspondre à l’URI de demande pour le modèle.</span><span class="sxs-lookup"><span data-stu-id="bd96e-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="bd96e-153">Dans cet exemple, « customers » et « orders » sont des segments de littéral, et « {customerId} » est un paramètre de variable.</span><span class="sxs-lookup"><span data-stu-id="bd96e-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="bd96e-154">Les URI suivants correspondrait à ce modèle :</span><span class="sxs-lookup"><span data-stu-id="bd96e-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="bd96e-155">Vous pouvez limiter la correspondance à l’aide de [contraintes](#constraints), comme décrit plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="bd96e-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="bd96e-156">Notez que le &quot;{customerId}&quot; paramètre dans le modèle d’itinéraire correspond au nom de la *customerId* paramètre dans la méthode.</span><span class="sxs-lookup"><span data-stu-id="bd96e-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="bd96e-157">Lors de l’API Web appelle l’action du contrôleur, il essaie de lier les paramètres d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="bd96e-158">Par exemple, si l’URI est `http://example.com/customers/1/orders`, API Web essaie de lier la valeur « 1 » pour le *customerId* paramètre dans l’action.</span><span class="sxs-lookup"><span data-stu-id="bd96e-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="bd96e-159">Un modèle d’URI peut avoir plusieurs paramètres :</span><span class="sxs-lookup"><span data-stu-id="bd96e-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="bd96e-160">Toutes les méthodes de contrôleur qui n’ont pas un attribut d’itinéraire utilisent le routage basé sur une convention.</span><span class="sxs-lookup"><span data-stu-id="bd96e-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="bd96e-161">De cette façon, vous pouvez combiner les deux types de routage dans le même projet.</span><span class="sxs-lookup"><span data-stu-id="bd96e-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="bd96e-162">Méthodes HTTP</span><span class="sxs-lookup"><span data-stu-id="bd96e-162">HTTP Methods</span></span>

<span data-ttu-id="bd96e-163">API Web sélectionne également les actions en fonction de la méthode HTTP de la requête (GET, POST, etc.).</span><span class="sxs-lookup"><span data-stu-id="bd96e-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="bd96e-164">Par défaut, les API Web recherche une correspondance non sensible à avec le début du nom de la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="bd96e-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="bd96e-165">Par exemple, une méthode de contrôleur nommée `PutCustomers` correspond à une demande HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="bd96e-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="bd96e-166">Vous pouvez remplacer cette convention en décorant la méthode avec n’importe quel les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="bd96e-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="bd96e-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="bd96e-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="bd96e-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="bd96e-168">**[HttpGet]**</span></span>
- <span data-ttu-id="bd96e-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="bd96e-169">**[HttpHead]**</span></span>
- <span data-ttu-id="bd96e-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="bd96e-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="bd96e-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="bd96e-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="bd96e-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="bd96e-172">**[HttpPost]**</span></span>
- <span data-ttu-id="bd96e-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="bd96e-173">**[HttpPut]**</span></span>

<span data-ttu-id="bd96e-174">L’exemple suivant mappe la méthode CreateBook aux requêtes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="bd96e-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="bd96e-175">Pour toutes les autres méthodes HTTP, y compris les méthodes non standards, utilisent la **AcceptVerbs** attribut, qui prend une liste de méthodes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd96e-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="bd96e-176">Préfixes d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="bd96e-176">Route Prefixes</span></span>

<span data-ttu-id="bd96e-177">Souvent, les itinéraires dans un contrôleur commencent toutes par le même préfixe.</span><span class="sxs-lookup"><span data-stu-id="bd96e-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="bd96e-178">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bd96e-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="bd96e-179">Vous pouvez définir un préfixe commun pour l’ensemble du contrôleur à l’aide de la **[RoutePrefix]** attribut :</span><span class="sxs-lookup"><span data-stu-id="bd96e-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="bd96e-180">Utiliser un tilde (~) sur l’attribut de méthode pour remplacer le préfixe d’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="bd96e-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="bd96e-181">Le préfixe d’itinéraire peut inclure des paramètres :</span><span class="sxs-lookup"><span data-stu-id="bd96e-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="bd96e-182">Contraintes de routage</span><span class="sxs-lookup"><span data-stu-id="bd96e-182">Route Constraints</span></span>

<span data-ttu-id="bd96e-183">Contraintes de routage vous permettent de limiter la correspondance des paramètres dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="bd96e-184">La syntaxe générale est &quot;{ : contrainte de paramètre}&quot;.</span><span class="sxs-lookup"><span data-stu-id="bd96e-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="bd96e-185">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bd96e-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="bd96e-186">Ici, le premier itinéraire est uniquement activée si le &quot;id&quot; segment de l’URI est un entier.</span><span class="sxs-lookup"><span data-stu-id="bd96e-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="bd96e-187">Sinon, le deuxième itinéraire est choisi.</span><span class="sxs-lookup"><span data-stu-id="bd96e-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="bd96e-188">Le tableau suivant répertorie les contraintes qui sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="bd96e-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="bd96e-189">Contrainte</span><span class="sxs-lookup"><span data-stu-id="bd96e-189">Constraint</span></span> | <span data-ttu-id="bd96e-190">Description</span><span class="sxs-lookup"><span data-stu-id="bd96e-190">Description</span></span> | <span data-ttu-id="bd96e-191">Exemple</span><span class="sxs-lookup"><span data-stu-id="bd96e-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bd96e-192">alpha</span><span class="sxs-lookup"><span data-stu-id="bd96e-192">alpha</span></span> | <span data-ttu-id="bd96e-193">Correspondances en majuscules ou minuscules de l’alphabet Latin (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="bd96e-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="bd96e-194">{alpha : x}</span><span class="sxs-lookup"><span data-stu-id="bd96e-194">{x:alpha}</span></span> |
| <span data-ttu-id="bd96e-195">bool</span><span class="sxs-lookup"><span data-stu-id="bd96e-195">bool</span></span> | <span data-ttu-id="bd96e-196">Correspond à une valeur booléenne.</span><span class="sxs-lookup"><span data-stu-id="bd96e-196">Matches a Boolean value.</span></span> | <span data-ttu-id="bd96e-197">{x : bool}</span><span class="sxs-lookup"><span data-stu-id="bd96e-197">{x:bool}</span></span> |
| <span data-ttu-id="bd96e-198">datetime</span><span class="sxs-lookup"><span data-stu-id="bd96e-198">datetime</span></span> | <span data-ttu-id="bd96e-199">Correspond à un **DateTime** valeur.</span><span class="sxs-lookup"><span data-stu-id="bd96e-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="bd96e-200">{x : datetime}</span><span class="sxs-lookup"><span data-stu-id="bd96e-200">{x:datetime}</span></span> |
| <span data-ttu-id="bd96e-201">decimal</span><span class="sxs-lookup"><span data-stu-id="bd96e-201">decimal</span></span> | <span data-ttu-id="bd96e-202">Correspond à une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="bd96e-202">Matches a decimal value.</span></span> | <span data-ttu-id="bd96e-203">{x : decimal}</span><span class="sxs-lookup"><span data-stu-id="bd96e-203">{x:decimal}</span></span> |
| <span data-ttu-id="bd96e-204">double</span><span class="sxs-lookup"><span data-stu-id="bd96e-204">double</span></span> | <span data-ttu-id="bd96e-205">Correspond à une valeur à virgule flottante 64 bits.</span><span class="sxs-lookup"><span data-stu-id="bd96e-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="bd96e-206">{x : double}</span><span class="sxs-lookup"><span data-stu-id="bd96e-206">{x:double}</span></span> |
| <span data-ttu-id="bd96e-207">float</span><span class="sxs-lookup"><span data-stu-id="bd96e-207">float</span></span> | <span data-ttu-id="bd96e-208">Correspond à une valeur à virgule flottante 32 bits.</span><span class="sxs-lookup"><span data-stu-id="bd96e-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="bd96e-209">{x : float}</span><span class="sxs-lookup"><span data-stu-id="bd96e-209">{x:float}</span></span> |
| <span data-ttu-id="bd96e-210">GUID</span><span class="sxs-lookup"><span data-stu-id="bd96e-210">guid</span></span> | <span data-ttu-id="bd96e-211">Correspond à une valeur GUID.</span><span class="sxs-lookup"><span data-stu-id="bd96e-211">Matches a GUID value.</span></span> | <span data-ttu-id="bd96e-212">{x : guid}</span><span class="sxs-lookup"><span data-stu-id="bd96e-212">{x:guid}</span></span> |
| <span data-ttu-id="bd96e-213">int</span><span class="sxs-lookup"><span data-stu-id="bd96e-213">int</span></span> | <span data-ttu-id="bd96e-214">Correspond à une valeur d’entier 32 bits.</span><span class="sxs-lookup"><span data-stu-id="bd96e-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="bd96e-215">{x : int}</span><span class="sxs-lookup"><span data-stu-id="bd96e-215">{x:int}</span></span> |
| <span data-ttu-id="bd96e-216">length</span><span class="sxs-lookup"><span data-stu-id="bd96e-216">length</span></span> | <span data-ttu-id="bd96e-217">Correspond à une chaîne avec la longueur spécifiée ou dans une plage spécifiée de longueurs.</span><span class="sxs-lookup"><span data-stu-id="bd96e-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="bd96e-218">{x : length(6)} {x : length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="bd96e-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="bd96e-219">long</span><span class="sxs-lookup"><span data-stu-id="bd96e-219">long</span></span> | <span data-ttu-id="bd96e-220">Correspond à une valeur d’entier 64 bits.</span><span class="sxs-lookup"><span data-stu-id="bd96e-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="bd96e-221">{x : long}</span><span class="sxs-lookup"><span data-stu-id="bd96e-221">{x:long}</span></span> |
| <span data-ttu-id="bd96e-222">max</span><span class="sxs-lookup"><span data-stu-id="bd96e-222">max</span></span> | <span data-ttu-id="bd96e-223">Correspond à un entier avec une valeur maximale.</span><span class="sxs-lookup"><span data-stu-id="bd96e-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="bd96e-224">{x : max(10)}</span><span class="sxs-lookup"><span data-stu-id="bd96e-224">{x:max(10)}</span></span> |
| <span data-ttu-id="bd96e-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="bd96e-225">maxlength</span></span> | <span data-ttu-id="bd96e-226">Correspond à une chaîne avec une longueur maximale.</span><span class="sxs-lookup"><span data-stu-id="bd96e-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="bd96e-227">{x : maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="bd96e-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="bd96e-228">min</span><span class="sxs-lookup"><span data-stu-id="bd96e-228">min</span></span> | <span data-ttu-id="bd96e-229">Correspond à un entier avec une valeur minimale.</span><span class="sxs-lookup"><span data-stu-id="bd96e-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="bd96e-230">{x : min(10)}</span><span class="sxs-lookup"><span data-stu-id="bd96e-230">{x:min(10)}</span></span> |
| <span data-ttu-id="bd96e-231">minLength</span><span class="sxs-lookup"><span data-stu-id="bd96e-231">minlength</span></span> | <span data-ttu-id="bd96e-232">Correspond à une chaîne avec une longueur minimale.</span><span class="sxs-lookup"><span data-stu-id="bd96e-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="bd96e-233">{x : minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="bd96e-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="bd96e-234">range</span><span class="sxs-lookup"><span data-stu-id="bd96e-234">range</span></span> | <span data-ttu-id="bd96e-235">Correspond à un entier compris dans une plage de valeurs.</span><span class="sxs-lookup"><span data-stu-id="bd96e-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="bd96e-236">{x : range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="bd96e-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="bd96e-237">regex</span><span class="sxs-lookup"><span data-stu-id="bd96e-237">regex</span></span> | <span data-ttu-id="bd96e-238">Correspond à une expression régulière.</span><span class="sxs-lookup"><span data-stu-id="bd96e-238">Matches a regular expression.</span></span> | <span data-ttu-id="bd96e-239">{x : regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="bd96e-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="bd96e-240">Notez que certaines des contraintes, telles que &quot;min&quot;, acceptent des arguments entre parenthèses.</span><span class="sxs-lookup"><span data-stu-id="bd96e-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="bd96e-241">Vous pouvez appliquer plusieurs contraintes à un paramètre, séparé par un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="bd96e-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="bd96e-242">Contraintes d’itinéraire personnalisé</span><span class="sxs-lookup"><span data-stu-id="bd96e-242">Custom Route Constraints</span></span>

<span data-ttu-id="bd96e-243">Vous pouvez créer des contraintes de routage personnalisées en implémentant la **IHttpRouteConstraint** interface.</span><span class="sxs-lookup"><span data-stu-id="bd96e-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="bd96e-244">Par exemple, la contrainte suivante restreint un paramètre à une valeur entière non nulle.</span><span class="sxs-lookup"><span data-stu-id="bd96e-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="bd96e-245">Le code suivant montre comment inscrire la contrainte :</span><span class="sxs-lookup"><span data-stu-id="bd96e-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="bd96e-246">Maintenant, vous pouvez appliquer la contrainte dans vos routes :</span><span class="sxs-lookup"><span data-stu-id="bd96e-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="bd96e-247">Vous pouvez également remplacer l’intégralité de **DefaultInlineConstraintResolver** classe en implémentant la **IInlineConstraintResolver** interface.</span><span class="sxs-lookup"><span data-stu-id="bd96e-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="bd96e-248">Cela remplace toutes les contraintes intégrées, à moins que votre implémentation de **IInlineConstraintResolver** spécifiquement les ajoute.</span><span class="sxs-lookup"><span data-stu-id="bd96e-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="bd96e-249">Paramètres d’URI facultatif et les valeurs par défaut</span><span class="sxs-lookup"><span data-stu-id="bd96e-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="bd96e-250">Vous pouvez rendre un paramètre d’URI facultatif en ajoutant un point d’interrogation pour le paramètre d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="bd96e-251">Si un paramètre d’itinéraire est facultatif, vous devez définir une valeur par défaut pour le paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="bd96e-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="bd96e-252">Dans cet exemple, `/api/books/locale/1033` et `/api/books/locale` retournent la même ressource.</span><span class="sxs-lookup"><span data-stu-id="bd96e-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="bd96e-253">Vous pouvez également spécifier une valeur par défaut dans le modèle d’itinéraire, comme suit :</span><span class="sxs-lookup"><span data-stu-id="bd96e-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="bd96e-254">Il est presque identique à l’exemple précédent, mais il existe une légère différence de comportement lorsque la valeur par défaut est appliquée.</span><span class="sxs-lookup"><span data-stu-id="bd96e-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="bd96e-255">Dans le premier exemple (« {lcid ?} »), la valeur par défaut 1033 est affectée directement au paramètre de méthode, donc le paramètre aura cette valeur exacte.</span><span class="sxs-lookup"><span data-stu-id="bd96e-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="bd96e-256">Dans le deuxième exemple (« {lcid = 1033} »), la valeur par défaut de « 1033 » passe par le processus de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="bd96e-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="bd96e-257">Le binder de modèle par défaut convertira « 1033 » à la valeur numérique 1033.</span><span class="sxs-lookup"><span data-stu-id="bd96e-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="bd96e-258">Toutefois, vous pouvez connecter dans un classeur de modèles personnalisés, ce qui peut faire quelque chose de différent.</span><span class="sxs-lookup"><span data-stu-id="bd96e-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="bd96e-259">(Dans la plupart des cas, sauf si vous avez des classeurs de modèles personnalisés dans votre pipeline, les deux formes sera équivalentes.)</span><span class="sxs-lookup"><span data-stu-id="bd96e-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="bd96e-260">Noms de routes</span><span class="sxs-lookup"><span data-stu-id="bd96e-260">Route Names</span></span>

<span data-ttu-id="bd96e-261">Dans l’API Web, chaque routage possède un nom.</span><span class="sxs-lookup"><span data-stu-id="bd96e-261">In Web API, every route has a name.</span></span> <span data-ttu-id="bd96e-262">Les noms de routes sont utiles pour générer des liens, afin que vous pouvez inclure un lien dans une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd96e-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="bd96e-263">Pour spécifier le nom d’itinéraire, définissez le **nom** propriété sur l’attribut.</span><span class="sxs-lookup"><span data-stu-id="bd96e-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="bd96e-264">L’exemple suivant montre comment définir le nom d’itinéraire et également comment utiliser le nom d’itinéraire lors de la génération d’un lien.</span><span class="sxs-lookup"><span data-stu-id="bd96e-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="bd96e-265">Ordre de l’itinéraire</span><span class="sxs-lookup"><span data-stu-id="bd96e-265">Route Order</span></span>

<span data-ttu-id="bd96e-266">Lorsque le framework tente de faire correspondre un URI avec un itinéraire, il évalue les itinéraires dans un ordre particulier.</span><span class="sxs-lookup"><span data-stu-id="bd96e-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="bd96e-267">Pour spécifier l’ordre, définissez le **RouteOrder** propriété sur l’attribut d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="bd96e-268">Des valeurs plus faibles sont évalués en premier.</span><span class="sxs-lookup"><span data-stu-id="bd96e-268">Lower values are evaluated first.</span></span> <span data-ttu-id="bd96e-269">La valeur d’ordre par défaut est égale à zéro.</span><span class="sxs-lookup"><span data-stu-id="bd96e-269">The default order value is zero.</span></span>

<span data-ttu-id="bd96e-270">Voici comment le classement total est déterminé :</span><span class="sxs-lookup"><span data-stu-id="bd96e-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="bd96e-271">Comparer la **RouteOrder** propriété de l’attribut d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="bd96e-272">Examinez chaque segment d’URI dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="bd96e-273">Pour chaque segment, commande comme suit :</span><span class="sxs-lookup"><span data-stu-id="bd96e-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="bd96e-274">Segments de littéral.</span><span class="sxs-lookup"><span data-stu-id="bd96e-274">Literal segments.</span></span>
    2. <span data-ttu-id="bd96e-275">Paramètres d’itinéraire avec des contraintes.</span><span class="sxs-lookup"><span data-stu-id="bd96e-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="bd96e-276">Paramètres d’itinéraire sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="bd96e-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="bd96e-277">Segments de paramètre de caractère générique avec des contraintes.</span><span class="sxs-lookup"><span data-stu-id="bd96e-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="bd96e-278">Segments de paramètre de caractère générique sans contraintes.</span><span class="sxs-lookup"><span data-stu-id="bd96e-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="bd96e-279">Dans le cas d’égalité, les itinéraires sont classés par une comparaison de chaînes ordinale respectant la casse ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) du modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="bd96e-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="bd96e-280">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="bd96e-280">Here is an example.</span></span> <span data-ttu-id="bd96e-281">Supposons que vous définissez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="bd96e-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="bd96e-282">Ces itinéraires sont classés comme suit.</span><span class="sxs-lookup"><span data-stu-id="bd96e-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="bd96e-283">commandes/détails</span><span class="sxs-lookup"><span data-stu-id="bd96e-283">orders/details</span></span>
2. <span data-ttu-id="bd96e-284">commandes / {id}</span><span class="sxs-lookup"><span data-stu-id="bd96e-284">orders/{id}</span></span>
3. <span data-ttu-id="bd96e-285">commandes / {customerName}</span><span class="sxs-lookup"><span data-stu-id="bd96e-285">orders/{customerName}</span></span>
4. <span data-ttu-id="bd96e-286">commandes / {\*date}</span><span class="sxs-lookup"><span data-stu-id="bd96e-286">orders/{\*date}</span></span>
5. <span data-ttu-id="bd96e-287">commandes / en attente</span><span class="sxs-lookup"><span data-stu-id="bd96e-287">orders/pending</span></span>

<span data-ttu-id="bd96e-288">Notez que « détails » sont un segment littéral et apparaît avant « {id} », mais « en attente » apparaît dernier, car le **RouteOrder** propriété est 1.</span><span class="sxs-lookup"><span data-stu-id="bd96e-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="bd96e-289">(Cet exemple suppose qu’il n’est aucun client nommé « détails » ou « en attente ».</span><span class="sxs-lookup"><span data-stu-id="bd96e-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="bd96e-290">En général, essayez d’éviter des itinéraires ambigus.</span><span class="sxs-lookup"><span data-stu-id="bd96e-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="bd96e-291">Dans cet exemple, un meilleur modèle d’itinéraire pour `GetByCustomer` est « clients / {customerName} »)</span><span class="sxs-lookup"><span data-stu-id="bd96e-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
