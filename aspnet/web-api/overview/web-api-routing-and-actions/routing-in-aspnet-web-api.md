---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routage dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207301"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="34634-102">Routage dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="34634-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="34634-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="34634-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="34634-104">Cet article décrit comment les API Web ASP.NET achemine les requêtes HTTP aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="34634-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="34634-105">Si vous êtes familiarisé avec ASP.NET MVC, le routage d’API Web est très similaire pour le routage MVC.</span><span class="sxs-lookup"><span data-stu-id="34634-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="34634-106">La principale différence est que les API Web utilise le verbe HTTP, pas le chemin d’accès URI, pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="34634-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="34634-107">Vous pouvez également utiliser le routage de style MVC dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="34634-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="34634-108">Cet article ne suppose pas aucune connaissance d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34634-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="34634-109">Tables de routage</span><span class="sxs-lookup"><span data-stu-id="34634-109">Routing Tables</span></span>

<span data-ttu-id="34634-110">Dans l’API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="34634-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="34634-111">Les méthodes publiques du contrôleur sont appelées *méthodes d’action* ou simplement *actions*.</span><span class="sxs-lookup"><span data-stu-id="34634-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="34634-112">Lorsque l’infrastructure API Web reçoit une demande, il achemine la demande à une action.</span><span class="sxs-lookup"><span data-stu-id="34634-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="34634-113">Pour déterminer l’action à appeler, le framework utilise un *table de routage*.</span><span class="sxs-lookup"><span data-stu-id="34634-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="34634-114">Le modèle de projet Visual Studio pour les API Web crée un itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="34634-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="34634-115">Cet itinéraire est défini dans le *WebApiConfig.cs* fichier, qui est placé dans le *application\_Démarrer* directory :</span><span class="sxs-lookup"><span data-stu-id="34634-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="34634-116">Pour plus d’informations sur la `WebApiConfig` de classe, consultez [configuration ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="34634-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="34634-117">Si vous hébergez des API Web, vous devez définir la table de routage directement sur le `HttpSelfHostConfiguration` objet.</span><span class="sxs-lookup"><span data-stu-id="34634-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="34634-118">Pour plus d’informations, consultez [auto-héberger une API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="34634-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="34634-119">Chaque entrée dans la table de routage contient un *modèle d’itinéraire*.</span><span class="sxs-lookup"><span data-stu-id="34634-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="34634-120">Le modèle d’itinéraire par défaut pour l’API Web est &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="34634-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="34634-121">Dans ce modèle, &quot;api&quot; est un segment de chemin d’accès littéral et {controller} et {id} sont des variables d’espace réservé.</span><span class="sxs-lookup"><span data-stu-id="34634-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="34634-122">Lorsque l’infrastructure API Web reçoit une requête HTTP, il tente de faire correspondre l’URI par rapport à un des modèles d’itinéraire dans la table de routage.</span><span class="sxs-lookup"><span data-stu-id="34634-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="34634-123">Si aucun itinéraire ne correspond, le client reçoit une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="34634-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="34634-124">Par exemple, les URI suivants correspondent à l’itinéraire par défaut :</span><span class="sxs-lookup"><span data-stu-id="34634-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="34634-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="34634-125">/api/contacts</span></span>
- <span data-ttu-id="34634-126">/API/contacts/1</span><span class="sxs-lookup"><span data-stu-id="34634-126">/api/contacts/1</span></span>
- <span data-ttu-id="34634-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="34634-127">/api/products/gizmo1</span></span>

<span data-ttu-id="34634-128">Toutefois, l’URI suivant ne correspond pas, car il manque le &quot;api&quot; segment :</span><span class="sxs-lookup"><span data-stu-id="34634-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="34634-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="34634-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="34634-130">La raison pour l’utilisation de « api » dans l’itinéraire consiste à éviter les collisions avec routage ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34634-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="34634-131">De cette façon, vous pouvez avoir &quot;/contacte&quot; accédez à un contrôleur MVC, et &quot;/api/contacts&quot; accédez à un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="34634-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="34634-132">Bien sûr, si vous n’aimez pas cette convention, vous pouvez modifier la table d’itinéraires par défaut.</span><span class="sxs-lookup"><span data-stu-id="34634-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="34634-133">Une fois qu’un itinéraire correspondant est trouvé, API Web sélectionne le contrôleur et l’action :</span><span class="sxs-lookup"><span data-stu-id="34634-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="34634-134">Pour trouver le contrôleur, API Web ajoute &quot;contrôleur&quot; à la valeur de la *{controller}* variable.</span><span class="sxs-lookup"><span data-stu-id="34634-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="34634-135">Pour rechercher l’action, API Web examine le verbe HTTP et recherche d’une action dont le nom commence par ce nom du verbe HTTP.</span><span class="sxs-lookup"><span data-stu-id="34634-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="34634-136">Par exemple, avec une demande GET, API Web se présente pour le préfixe une action &quot;obtenir&quot;, tel que &quot;GetContact&quot; ou &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="34634-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="34634-137">Cette convention s’applique uniquement pour GET, POST, PUT, DELETE, HEAD, OPTIONS et verbes des correctifs.</span><span class="sxs-lookup"><span data-stu-id="34634-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="34634-138">Vous pouvez activer les autres verbes HTTP en utilisant des attributs sur votre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="34634-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="34634-139">Nous verrons un exemple plus tard.</span><span class="sxs-lookup"><span data-stu-id="34634-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="34634-140">Autres variables d’espace réservé dans le modèle d’itinéraire, tel que *{id},* sont mappées aux paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="34634-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="34634-141">Examinons un exemple.</span><span class="sxs-lookup"><span data-stu-id="34634-141">Let's look at an example.</span></span> <span data-ttu-id="34634-142">Supposons que vous définissez le contrôleur suivant :</span><span class="sxs-lookup"><span data-stu-id="34634-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="34634-143">Voici certaines demandes HTTP possibles, ainsi que l’action qui est appelé pour chaque :</span><span class="sxs-lookup"><span data-stu-id="34634-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="34634-144">Verbe HTTP</span><span class="sxs-lookup"><span data-stu-id="34634-144">HTTP Verb</span></span> | <span data-ttu-id="34634-145">Chemin d’accès de l’URI</span><span class="sxs-lookup"><span data-stu-id="34634-145">URI Path</span></span> | <span data-ttu-id="34634-146">Action</span><span class="sxs-lookup"><span data-stu-id="34634-146">Action</span></span> | <span data-ttu-id="34634-147">Paramètre</span><span class="sxs-lookup"><span data-stu-id="34634-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="34634-148">GET</span><span class="sxs-lookup"><span data-stu-id="34634-148">GET</span></span> | <span data-ttu-id="34634-149">API/produits</span><span class="sxs-lookup"><span data-stu-id="34634-149">api/products</span></span> | <span data-ttu-id="34634-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="34634-150">GetAllProducts</span></span> | <span data-ttu-id="34634-151">*(aucun)*</span><span class="sxs-lookup"><span data-stu-id="34634-151">*(none)*</span></span> |
| <span data-ttu-id="34634-152">GET</span><span class="sxs-lookup"><span data-stu-id="34634-152">GET</span></span> | <span data-ttu-id="34634-153">produits/API/4</span><span class="sxs-lookup"><span data-stu-id="34634-153">api/products/4</span></span> | <span data-ttu-id="34634-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="34634-154">GetProductById</span></span> | <span data-ttu-id="34634-155">4</span><span class="sxs-lookup"><span data-stu-id="34634-155">4</span></span> |
| <span data-ttu-id="34634-156">SUPPR</span><span class="sxs-lookup"><span data-stu-id="34634-156">DELETE</span></span> | <span data-ttu-id="34634-157">produits/API/4</span><span class="sxs-lookup"><span data-stu-id="34634-157">api/products/4</span></span> | <span data-ttu-id="34634-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="34634-158">DeleteProduct</span></span> | <span data-ttu-id="34634-159">4</span><span class="sxs-lookup"><span data-stu-id="34634-159">4</span></span> |
| <span data-ttu-id="34634-160">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="34634-160">POST</span></span> | <span data-ttu-id="34634-161">API/produits</span><span class="sxs-lookup"><span data-stu-id="34634-161">api/products</span></span> | <span data-ttu-id="34634-162">*(aucune correspondance trouvée)*</span><span class="sxs-lookup"><span data-stu-id="34634-162">*(no match)*</span></span> |  |

<span data-ttu-id="34634-163">Notez que le *{id}* segment de l’URI, le cas échéant, est mappé à la *id* paramètre de l’action.</span><span class="sxs-lookup"><span data-stu-id="34634-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="34634-164">Dans cet exemple, le contrôleur définit deux méthodes GET, une avec un *id* paramètre et l’autre sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="34634-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="34634-165">Notez également que la requête POST échouera, car le contrôleur ne définit pas un &quot;Post... &quot; (méthode).</span><span class="sxs-lookup"><span data-stu-id="34634-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="34634-166">Variantes de routage</span><span class="sxs-lookup"><span data-stu-id="34634-166">Routing Variations</span></span>

<span data-ttu-id="34634-167">La section précédente décrit le mécanisme de routage de base pour l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="34634-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="34634-168">Cette section décrit certaines variantes.</span><span class="sxs-lookup"><span data-stu-id="34634-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="34634-169">Verbes HTTP</span><span class="sxs-lookup"><span data-stu-id="34634-169">HTTP verbs</span></span>

<span data-ttu-id="34634-170">Au lieu d’utiliser la convention d’affectation de noms pour les verbes HTTP, vous pouvez spécifier explicitement le verbe HTTP pour une action en décorant la méthode d’action avec l’un des attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="34634-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="34634-171">Dans l’exemple suivant, la `FindProduct` méthode est mappée aux demandes GET :</span><span class="sxs-lookup"><span data-stu-id="34634-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="34634-172">Pour autoriser plusieurs verbes HTTP pour une action, ou pour autoriser les verbes HTTP autres que GET, PUT, POST, DELETE, HEAD, OPTIONS et PATCH, utilisez le `[AcceptVerbs]` attribut, qui prend une liste des verbes HTTP.</span><span class="sxs-lookup"><span data-stu-id="34634-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="34634-173">Routage par nom d’Action</span><span class="sxs-lookup"><span data-stu-id="34634-173">Routing by Action Name</span></span>

<span data-ttu-id="34634-174">Avec le modèle de routage par défaut, les API Web utilise le verbe HTTP pour sélectionner l’action.</span><span class="sxs-lookup"><span data-stu-id="34634-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="34634-175">Toutefois, vous pouvez également créer un itinéraire où le nom d’action est inclus dans l’URI :</span><span class="sxs-lookup"><span data-stu-id="34634-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="34634-176">Dans ce modèle d’itinéraire, le *{action}* noms de paramètres de la méthode d’action sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="34634-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="34634-177">Avec ce style de routage, utilisez les attributs pour spécifier les verbes HTTP autorisés.</span><span class="sxs-lookup"><span data-stu-id="34634-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="34634-178">Par exemple, supposons que votre contrôleur est la méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="34634-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="34634-179">Dans ce cas, une demande GET pour « api/produits/détails/1 » serait mappé à la `Details` (méthode).</span><span class="sxs-lookup"><span data-stu-id="34634-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="34634-180">Ce style de routage est similaire à ASP.NET MVC et peut être approprié pour une API de style RPC.</span><span class="sxs-lookup"><span data-stu-id="34634-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="34634-181">Vous pouvez remplacer le nom d’action en utilisant le `[ActionName]` attribut.</span><span class="sxs-lookup"><span data-stu-id="34634-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="34634-182">Dans l’exemple suivant, il existe deux actions qui mappent aux &quot;produits/api/miniature/*id*. Prend en charge GET et l’autre prend en charge POST :</span><span class="sxs-lookup"><span data-stu-id="34634-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="34634-183">Non-Actions</span><span class="sxs-lookup"><span data-stu-id="34634-183">Non-Actions</span></span>

<span data-ttu-id="34634-184">Pour éviter une méthode appelée en tant qu’action, utilisez la `[NonAction]` attribut.</span><span class="sxs-lookup"><span data-stu-id="34634-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="34634-185">Cela signale à l’infrastructure que la méthode n’est pas une action, même si elle correspondrait sinon les règles de routage.</span><span class="sxs-lookup"><span data-stu-id="34634-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="34634-186">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="34634-186">Further Reading</span></span>

<span data-ttu-id="34634-187">Cette rubrique fourni une vue d’ensemble du routage.</span><span class="sxs-lookup"><span data-stu-id="34634-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="34634-188">Pour plus d’informations, consultez [routage et sélection d’Action](routing-and-action-selection.md), qui décrit exactement comment le framework correspond à un URI à un itinéraire, sélectionne un contrôleur, puis sélectionne l’action à appeler.</span><span class="sxs-lookup"><span data-stu-id="34634-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
