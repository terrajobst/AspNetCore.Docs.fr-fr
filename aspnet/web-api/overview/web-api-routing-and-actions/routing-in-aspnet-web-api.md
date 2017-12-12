---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routage dans ASP.NET Web API | Documents Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="284b8-102">Routage dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="284b8-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="284b8-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="284b8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="284b8-104">Cet article décrit comment ASP.NET Web API achemine les requêtes HTTP aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="284b8-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="284b8-105">Si vous êtes familiarisé avec ASP.NET MVC, API Web routage est très similaire pour le routage MVC.</span><span class="sxs-lookup"><span data-stu-id="284b8-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="284b8-106">La principale différence est que les API Web utilise la méthode HTTP, pas le chemin d’accès URI, pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="284b8-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="284b8-107">Vous pouvez également utiliser le routage MVC-style dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="284b8-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="284b8-108">Cet article n’assume pas toutes les connaissances d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="284b8-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="284b8-109">Tables de routage</span><span class="sxs-lookup"><span data-stu-id="284b8-109">Routing Tables</span></span>

<span data-ttu-id="284b8-110">Dans ASP.NET Web API, un *contrôleur* est une classe qui gère les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="284b8-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="284b8-111">Les méthodes publiques du contrôleur sont appelés *méthodes d’action* ou simplement *actions*.</span><span class="sxs-lookup"><span data-stu-id="284b8-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="284b8-112">Lorsque l’infrastructure API Web reçoit une demande, il achemine la demande à une action.</span><span class="sxs-lookup"><span data-stu-id="284b8-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="284b8-113">Pour déterminer l’action à appeler, le framework utilise un *table de routage*.</span><span class="sxs-lookup"><span data-stu-id="284b8-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="284b8-114">Le modèle de projet Visual Studio pour l’API Web crée un itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="284b8-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="284b8-115">Cet itinéraire est défini dans le fichier WebApiConfig.cs, qui est placé dans l’application\_répertoire de démarrage :</span><span class="sxs-lookup"><span data-stu-id="284b8-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="284b8-116">Pour plus d’informations sur la **WebApiConfig** de classe, consultez [configuration ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="284b8-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="284b8-117">Si vous hébergez des API Web, vous devez définir la table de routage directement sur le **HttpSelfHostConfiguration** objet.</span><span class="sxs-lookup"><span data-stu-id="284b8-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="284b8-118">Pour plus d’informations, consultez [auto-hébergement une API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="284b8-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="284b8-119">Chaque entrée de la table de routage contient un *modèle d’itinéraire*.</span><span class="sxs-lookup"><span data-stu-id="284b8-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="284b8-120">Le modèle d’itinéraire par défaut pour l’API Web est &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="284b8-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="284b8-121">Dans ce modèle, &quot;api&quot; est un segment de chemin d’accès littéral et {controller} et {id} sont des variables de l’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="284b8-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="284b8-122">Lorsque l’infrastructure API Web reçoit une requête HTTP, il essaie de faire correspondre l’URI sur l’un des modèles d’itinéraire dans la table de routage.</span><span class="sxs-lookup"><span data-stu-id="284b8-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="284b8-123">Si aucun itinéraire ne correspond, le client reçoit une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="284b8-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="284b8-124">Par exemple, les URI suivants correspondent à l’itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="284b8-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="284b8-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="284b8-125">/api/contacts</span></span>
- <span data-ttu-id="284b8-126">/API/contacts/1</span><span class="sxs-lookup"><span data-stu-id="284b8-126">/api/contacts/1</span></span>
- <span data-ttu-id="284b8-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="284b8-127">/api/products/gizmo1</span></span>

<span data-ttu-id="284b8-128">Toutefois, l’URI suivant ne correspond pas, car il lui manque le &quot;api&quot; segment :</span><span class="sxs-lookup"><span data-stu-id="284b8-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="284b8-129">contacts/1</span><span class="sxs-lookup"><span data-stu-id="284b8-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="284b8-130">La raison de l’utilisation de « api » dans l’itinéraire est à éviter des collisions avec le routage ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="284b8-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="284b8-131">De cette façon, vous pouvez avoir &quot;/contacts&quot; accédez à un contrôleur MVC, et &quot;/api/contacts&quot; atteindre le contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="284b8-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="284b8-132">Bien entendu, si vous n’aimez pas cette convention, vous pouvez modifier la table d’itinéraires par défaut.</span><span class="sxs-lookup"><span data-stu-id="284b8-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="284b8-133">Une fois qu’un itinéraire correspondant est trouvé, API Web sélectionne le contrôleur et l’action :</span><span class="sxs-lookup"><span data-stu-id="284b8-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="284b8-134">Pour rechercher le contrôleur, API Web ajoute &quot;contrôleur&quot; à la valeur de la *{controller}* variable.</span><span class="sxs-lookup"><span data-stu-id="284b8-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="284b8-135">Pour trouver l’action, API Web ressemble à la méthode HTTP et recherche d’une action dont le nom commence par ce nom de méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="284b8-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="284b8-136">Par exemple, API Web avec une demande GET, recherche une action qui commence par &quot;obtenir... &quot;, tel que &quot;GetContact&quot; ou &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="284b8-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="284b8-137">Cette convention s’applique uniquement GET, POST, PUT et supprimer les méthodes.</span><span class="sxs-lookup"><span data-stu-id="284b8-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="284b8-138">Vous pouvez activer les autres méthodes HTTP à l’aide des attributs sur votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="284b8-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="284b8-139">Nous allons voir un exemple plus tard.</span><span class="sxs-lookup"><span data-stu-id="284b8-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="284b8-140">Autres variables d’espace réservé dans le modèle d’itinéraire, tel que *{id},* sont mappées aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="284b8-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="284b8-141">Examinons un exemple.</span><span class="sxs-lookup"><span data-stu-id="284b8-141">Let's look at an example.</span></span> <span data-ttu-id="284b8-142">Supposons que vous définissez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="284b8-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="284b8-143">Voici certaines demandes HTTP possibles, ainsi que l’action qui est appelé pour chaque :</span><span class="sxs-lookup"><span data-stu-id="284b8-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="284b8-144">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="284b8-144">HTTP Method</span></span> | <span data-ttu-id="284b8-145">Chemin d’accès URI</span><span class="sxs-lookup"><span data-stu-id="284b8-145">URI Path</span></span> | <span data-ttu-id="284b8-146">Action</span><span class="sxs-lookup"><span data-stu-id="284b8-146">Action</span></span> | <span data-ttu-id="284b8-147">Paramètre</span><span class="sxs-lookup"><span data-stu-id="284b8-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="284b8-148">GET</span><span class="sxs-lookup"><span data-stu-id="284b8-148">GET</span></span> | <span data-ttu-id="284b8-149">API/produits</span><span class="sxs-lookup"><span data-stu-id="284b8-149">api/products</span></span> | <span data-ttu-id="284b8-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="284b8-150">GetAllProducts</span></span> | <span data-ttu-id="284b8-151">*(aucun)*</span><span class="sxs-lookup"><span data-stu-id="284b8-151">*(none)*</span></span> |
| <span data-ttu-id="284b8-152">GET</span><span class="sxs-lookup"><span data-stu-id="284b8-152">GET</span></span> | <span data-ttu-id="284b8-153">API/produits/4</span><span class="sxs-lookup"><span data-stu-id="284b8-153">api/products/4</span></span> | <span data-ttu-id="284b8-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="284b8-154">GetProductById</span></span> | <span data-ttu-id="284b8-155">4</span><span class="sxs-lookup"><span data-stu-id="284b8-155">4</span></span> |
| <span data-ttu-id="284b8-156">SUPPR</span><span class="sxs-lookup"><span data-stu-id="284b8-156">DELETE</span></span> | <span data-ttu-id="284b8-157">API/produits/4</span><span class="sxs-lookup"><span data-stu-id="284b8-157">api/products/4</span></span> | <span data-ttu-id="284b8-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="284b8-158">DeleteProduct</span></span> | <span data-ttu-id="284b8-159">4</span><span class="sxs-lookup"><span data-stu-id="284b8-159">4</span></span> |
| <span data-ttu-id="284b8-160">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="284b8-160">POST</span></span> | <span data-ttu-id="284b8-161">API/produits</span><span class="sxs-lookup"><span data-stu-id="284b8-161">api/products</span></span> | <span data-ttu-id="284b8-162">*(aucune correspondance)*</span><span class="sxs-lookup"><span data-stu-id="284b8-162">*(no match)*</span></span> |  |

<span data-ttu-id="284b8-163">Notez que la *{id}* segment de l’URI, le cas échéant, est mappé à la *id* paramètre de l’action.</span><span class="sxs-lookup"><span data-stu-id="284b8-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="284b8-164">Dans cet exemple, le contrôleur définit deux méthodes GET, une avec un *id* paramètre et sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="284b8-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="284b8-165">Notez également que la requête POST échouera, car le contrôleur ne définit pas un &quot;Post... &quot; (méthode).</span><span class="sxs-lookup"><span data-stu-id="284b8-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="284b8-166">Variantes de routage</span><span class="sxs-lookup"><span data-stu-id="284b8-166">Routing Variations</span></span>

<span data-ttu-id="284b8-167">La section précédente décrit le mécanisme de routage de base pour l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="284b8-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="284b8-168">Cette section décrit des variantes.</span><span class="sxs-lookup"><span data-stu-id="284b8-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="284b8-169">Méthodes HTTP</span><span class="sxs-lookup"><span data-stu-id="284b8-169">HTTP Methods</span></span>

<span data-ttu-id="284b8-170">Au lieu d’utiliser la convention d’affectation de noms pour les méthodes HTTP, vous pouvez spécifier explicitement la méthode HTTP pour une action en décorant la méthode d’action avec les **HttpGet**, **HttpPut**, **HttpPost** , ou **HttpDelete** attribut.</span><span class="sxs-lookup"><span data-stu-id="284b8-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="284b8-171">Dans l’exemple suivant, la méthode FindProduct est mappée à des demandes GET :</span><span class="sxs-lookup"><span data-stu-id="284b8-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="284b8-172">Pour autoriser plusieurs méthodes HTTP pour une action, ou pour autoriser des méthodes HTTP autre que GET, PUT, POST et DELETE, utilisez le **AcceptVerbs** attribut, qui prend une liste de méthodes HTTP.</span><span class="sxs-lookup"><span data-stu-id="284b8-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="284b8-173">Routage par nom d’Action</span><span class="sxs-lookup"><span data-stu-id="284b8-173">Routing by Action Name</span></span>

<span data-ttu-id="284b8-174">Le modèle de routage par défaut, les API Web utilise la méthode HTTP pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="284b8-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="284b8-175">Toutefois, vous pouvez également créer un itinéraire où le nom d’action est inclus dans l’URI :</span><span class="sxs-lookup"><span data-stu-id="284b8-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="284b8-176">Dans ce modèle d’itinéraire, le *{action}* noms de paramètres de la méthode d’action sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="284b8-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="284b8-177">Avec ce style de routage, utilisez des attributs pour spécifier les méthodes HTTP autorisées.</span><span class="sxs-lookup"><span data-stu-id="284b8-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="284b8-178">Par exemple, supposons que votre contrôleur est la méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="284b8-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="284b8-179">Dans ce cas, une demande GET pour « api/produits/détails/1 » serait mappés à la méthode de détails.</span><span class="sxs-lookup"><span data-stu-id="284b8-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="284b8-180">Ce style de routage est similaire à ASP.NET MVC et peut s’avérer nécessaire d’API de style RPC.</span><span class="sxs-lookup"><span data-stu-id="284b8-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="284b8-181">Vous pouvez remplacer le nom d’action à l’aide de la **ActionName** attribut.</span><span class="sxs-lookup"><span data-stu-id="284b8-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="284b8-182">Dans l’exemple suivant, il existe deux actions correspondant aux &quot;api/produits/miniature/*id*. Prend en charge GET, et l’autre prend en charge POST :</span><span class="sxs-lookup"><span data-stu-id="284b8-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="284b8-183">Non-Actions</span><span class="sxs-lookup"><span data-stu-id="284b8-183">Non-Actions</span></span>

<span data-ttu-id="284b8-184">Pour éviter une méthode appelée en tant qu’action, utilisez la **NonAction** attribut.</span><span class="sxs-lookup"><span data-stu-id="284b8-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="284b8-185">Cela signale à l’infrastructure que la méthode n’est pas une action, même si elle correspondrait sinon les règles de routage.</span><span class="sxs-lookup"><span data-stu-id="284b8-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="284b8-186">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="284b8-186">Further Reading</span></span>

<span data-ttu-id="284b8-187">Cette rubrique fourni une vue d’ensemble du routage.</span><span class="sxs-lookup"><span data-stu-id="284b8-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="284b8-188">Pour plus d’informations, consultez [routage et sélection d’Action](routing-and-action-selection.md), qui décrit exactement comment le framework correspond à un URI à un itinéraire, sélectionne un contrôleur et sélectionne ensuite l’action à appeler.</span><span class="sxs-lookup"><span data-stu-id="284b8-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
