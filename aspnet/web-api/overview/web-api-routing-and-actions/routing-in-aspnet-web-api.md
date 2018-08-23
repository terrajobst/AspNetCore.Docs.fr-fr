---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routage dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/11/2012
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 458f9a6369fe97bab33d70bf31bd470b1b0e593c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836463"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="62114-102">Routage dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="62114-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="62114-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="62114-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="62114-104">Cet article décrit comment les API Web ASP.NET achemine les requêtes HTTP aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="62114-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="62114-105">Si vous êtes familiarisé avec ASP.NET MVC, le routage d’API Web est très similaire pour le routage MVC.</span><span class="sxs-lookup"><span data-stu-id="62114-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="62114-106">La principale différence est que les API Web utilise la méthode HTTP, pas le chemin d’accès URI, pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="62114-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="62114-107">Vous pouvez également utiliser le routage de style MVC dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="62114-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="62114-108">Cet article ne suppose pas aucune connaissance d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="62114-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="62114-109">Tables de routage</span><span class="sxs-lookup"><span data-stu-id="62114-109">Routing Tables</span></span>

<span data-ttu-id="62114-110">Dans l’API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="62114-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="62114-111">Les méthodes publiques du contrôleur sont appelées *méthodes d’action* ou simplement *actions*.</span><span class="sxs-lookup"><span data-stu-id="62114-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="62114-112">Lorsque l’infrastructure API Web reçoit une demande, il achemine la demande à une action.</span><span class="sxs-lookup"><span data-stu-id="62114-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="62114-113">Pour déterminer l’action à appeler, le framework utilise un *table de routage*.</span><span class="sxs-lookup"><span data-stu-id="62114-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="62114-114">Le modèle de projet Visual Studio pour les API Web crée un itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="62114-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="62114-115">Cet itinéraire est défini dans le fichier WebApiConfig.cs, qui est placé dans l’application\_répertoire de démarrage :</span><span class="sxs-lookup"><span data-stu-id="62114-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="62114-116">Pour plus d’informations sur la **WebApiConfig** de classe, consultez [configuration ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="62114-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="62114-117">Si vous hébergez des API Web, vous devez définir la table de routage directement sur le **HttpSelfHostConfiguration** objet.</span><span class="sxs-lookup"><span data-stu-id="62114-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="62114-118">Pour plus d’informations, consultez [auto-héberger une API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="62114-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="62114-119">Chaque entrée dans la table de routage contient un *modèle d’itinéraire*.</span><span class="sxs-lookup"><span data-stu-id="62114-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="62114-120">Le modèle d’itinéraire par défaut pour l’API Web est &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="62114-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="62114-121">Dans ce modèle, &quot;api&quot; est un segment de chemin d’accès littéral et {controller} et {id} sont des variables d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="62114-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="62114-122">Lorsque l’infrastructure API Web reçoit une requête HTTP, il tente de faire correspondre l’URI par rapport à un des modèles d’itinéraire dans la table de routage.</span><span class="sxs-lookup"><span data-stu-id="62114-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="62114-123">Si aucun itinéraire ne correspond, le client reçoit une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="62114-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="62114-124">Par exemple, les URI suivants correspondent à l’itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="62114-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="62114-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="62114-125">/api/contacts</span></span>
- <span data-ttu-id="62114-126">/API/contacts/1</span><span class="sxs-lookup"><span data-stu-id="62114-126">/api/contacts/1</span></span>
- <span data-ttu-id="62114-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="62114-127">/api/products/gizmo1</span></span>

<span data-ttu-id="62114-128">Toutefois, l’URI suivant ne correspond pas, car il manque le &quot;api&quot; segment :</span><span class="sxs-lookup"><span data-stu-id="62114-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="62114-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="62114-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="62114-130">La raison pour l’utilisation de « api » dans l’itinéraire consiste à éviter les collisions avec routage ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="62114-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="62114-131">De cette façon, vous pouvez avoir &quot;/contacte&quot; accédez à un contrôleur MVC, et &quot;/api/contacts&quot; accédez à un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="62114-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="62114-132">Bien sûr, si vous n’aimez pas cette convention, vous pouvez modifier la table d’itinéraires par défaut.</span><span class="sxs-lookup"><span data-stu-id="62114-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="62114-133">Une fois qu’un itinéraire correspondant est trouvé, API Web sélectionne le contrôleur et l’action :</span><span class="sxs-lookup"><span data-stu-id="62114-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="62114-134">Pour trouver le contrôleur, API Web ajoute &quot;contrôleur&quot; à la valeur de la *{controller}* variable.</span><span class="sxs-lookup"><span data-stu-id="62114-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="62114-135">Pour rechercher l’action, API Web examine la méthode HTTP et recherche d’une action dont le nom commence par ce nom de méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="62114-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="62114-136">Par exemple, avec une demande GET, Web API recherche d’une action qui commence par &quot;obtenir... &quot;, tel que &quot;GetContact&quot; ou &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="62114-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="62114-137">Cette convention s’applique uniquement pour GET, POST, PUT et supprimer des méthodes.</span><span class="sxs-lookup"><span data-stu-id="62114-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="62114-138">Vous pouvez activer les autres méthodes HTTP à l’aide des attributs sur votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="62114-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="62114-139">Nous verrons un exemple plus tard.</span><span class="sxs-lookup"><span data-stu-id="62114-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="62114-140">Autres variables d’espace réservé dans le modèle d’itinéraire, tel que *{id},* sont mappées aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="62114-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="62114-141">Examinons un exemple.</span><span class="sxs-lookup"><span data-stu-id="62114-141">Let's look at an example.</span></span> <span data-ttu-id="62114-142">Supposons que vous définissez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="62114-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="62114-143">Voici certaines demandes HTTP possibles, ainsi que l’action qui est appelé pour chaque :</span><span class="sxs-lookup"><span data-stu-id="62114-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="62114-144">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="62114-144">HTTP Method</span></span> | <span data-ttu-id="62114-145">Chemin d’accès de l’URI</span><span class="sxs-lookup"><span data-stu-id="62114-145">URI Path</span></span> | <span data-ttu-id="62114-146">Action</span><span class="sxs-lookup"><span data-stu-id="62114-146">Action</span></span> | <span data-ttu-id="62114-147">Paramètre</span><span class="sxs-lookup"><span data-stu-id="62114-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="62114-148">GET</span><span class="sxs-lookup"><span data-stu-id="62114-148">GET</span></span> | <span data-ttu-id="62114-149">API/produits</span><span class="sxs-lookup"><span data-stu-id="62114-149">api/products</span></span> | <span data-ttu-id="62114-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="62114-150">GetAllProducts</span></span> | <span data-ttu-id="62114-151">*(aucun)*</span><span class="sxs-lookup"><span data-stu-id="62114-151">*(none)*</span></span> |
| <span data-ttu-id="62114-152">GET</span><span class="sxs-lookup"><span data-stu-id="62114-152">GET</span></span> | <span data-ttu-id="62114-153">produits/API/4</span><span class="sxs-lookup"><span data-stu-id="62114-153">api/products/4</span></span> | <span data-ttu-id="62114-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="62114-154">GetProductById</span></span> | <span data-ttu-id="62114-155">4</span><span class="sxs-lookup"><span data-stu-id="62114-155">4</span></span> |
| <span data-ttu-id="62114-156">SUPPR</span><span class="sxs-lookup"><span data-stu-id="62114-156">DELETE</span></span> | <span data-ttu-id="62114-157">produits/API/4</span><span class="sxs-lookup"><span data-stu-id="62114-157">api/products/4</span></span> | <span data-ttu-id="62114-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="62114-158">DeleteProduct</span></span> | <span data-ttu-id="62114-159">4</span><span class="sxs-lookup"><span data-stu-id="62114-159">4</span></span> |
| <span data-ttu-id="62114-160">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="62114-160">POST</span></span> | <span data-ttu-id="62114-161">API/produits</span><span class="sxs-lookup"><span data-stu-id="62114-161">api/products</span></span> | <span data-ttu-id="62114-162">*(aucune correspondance trouvée)*</span><span class="sxs-lookup"><span data-stu-id="62114-162">*(no match)*</span></span> |  |

<span data-ttu-id="62114-163">Notez que le *{id}* segment de l’URI, le cas échéant, est mappé à la *id* paramètre de l’action.</span><span class="sxs-lookup"><span data-stu-id="62114-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="62114-164">Dans cet exemple, le contrôleur définit deux méthodes GET, une avec un *id* paramètre et l’autre sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="62114-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="62114-165">Notez également que la requête POST échouera, car le contrôleur ne définit pas un &quot;Post... &quot; (méthode).</span><span class="sxs-lookup"><span data-stu-id="62114-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="62114-166">Variantes de routage</span><span class="sxs-lookup"><span data-stu-id="62114-166">Routing Variations</span></span>

<span data-ttu-id="62114-167">La section précédente décrit le mécanisme de routage de base pour l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="62114-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="62114-168">Cette section décrit certaines variantes.</span><span class="sxs-lookup"><span data-stu-id="62114-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="62114-169">Méthodes HTTP</span><span class="sxs-lookup"><span data-stu-id="62114-169">HTTP Methods</span></span>

<span data-ttu-id="62114-170">Au lieu d’utiliser la convention d’affectation de noms pour les méthodes HTTP, vous pouvez spécifier explicitement la méthode HTTP pour une action en décorant la méthode d’action avec la **HttpGet**, **HttpPut**, **HttpPost** , ou **HttpDelete** attribut.</span><span class="sxs-lookup"><span data-stu-id="62114-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="62114-171">Dans l’exemple suivant, la méthode FindProduct est mappée aux demandes GET :</span><span class="sxs-lookup"><span data-stu-id="62114-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="62114-172">Pour autoriser plusieurs méthodes HTTP pour une action, ou pour autoriser des méthodes HTTP autres que GET, PUT, POST et DELETE, utilisez le **AcceptVerbs** attribut, qui prend une liste de méthodes HTTP.</span><span class="sxs-lookup"><span data-stu-id="62114-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="62114-173">Routage par nom d’Action</span><span class="sxs-lookup"><span data-stu-id="62114-173">Routing by Action Name</span></span>

<span data-ttu-id="62114-174">Avec le modèle de routage par défaut, les API Web utilise la méthode HTTP pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="62114-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="62114-175">Toutefois, vous pouvez également créer un itinéraire où le nom d’action est inclus dans l’URI :</span><span class="sxs-lookup"><span data-stu-id="62114-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="62114-176">Dans ce modèle d’itinéraire, le *{action}* noms de paramètres de la méthode d’action sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="62114-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="62114-177">Avec ce style de routage, utilisez les attributs pour spécifier les méthodes HTTP autorisées.</span><span class="sxs-lookup"><span data-stu-id="62114-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="62114-178">Par exemple, supposons que votre contrôleur est la méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="62114-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="62114-179">Dans ce cas, une demande GET pour « api/produits/détails/1 » serait mapper à la méthode de détails.</span><span class="sxs-lookup"><span data-stu-id="62114-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="62114-180">Ce style de routage est similaire à ASP.NET MVC et peut être approprié pour une API de style RPC.</span><span class="sxs-lookup"><span data-stu-id="62114-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="62114-181">Vous pouvez remplacer le nom d’action à l’aide de la **ActionName** attribut.</span><span class="sxs-lookup"><span data-stu-id="62114-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="62114-182">Dans l’exemple suivant, il existe deux actions qui mappent aux &quot;produits/api/miniature/*id*. Prend en charge GET et l’autre prend en charge POST :</span><span class="sxs-lookup"><span data-stu-id="62114-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="62114-183">Non-Actions</span><span class="sxs-lookup"><span data-stu-id="62114-183">Non-Actions</span></span>

<span data-ttu-id="62114-184">Pour éviter une méthode appelée en tant qu’action, utilisez la **NonAction** attribut.</span><span class="sxs-lookup"><span data-stu-id="62114-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="62114-185">Cela signale à l’infrastructure que la méthode n’est pas une action, même si elle correspondrait sinon les règles de routage.</span><span class="sxs-lookup"><span data-stu-id="62114-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="62114-186">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="62114-186">Further Reading</span></span>

<span data-ttu-id="62114-187">Cette rubrique fourni une vue d’ensemble du routage.</span><span class="sxs-lookup"><span data-stu-id="62114-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="62114-188">Pour plus d’informations, consultez [routage et sélection d’Action](routing-and-action-selection.md), qui décrit exactement comment le framework correspond à un URI à un itinéraire, sélectionne un contrôleur, puis sélectionne l’action à appeler.</span><span class="sxs-lookup"><span data-stu-id="62114-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
