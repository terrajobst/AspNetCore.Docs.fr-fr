---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routage et sélection d’Action dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 2421d3dd134e9188faf6933b076fb9d22a119617
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832828"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="f2066-102">Routage et sélection d’Action dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f2066-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f2066-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f2066-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f2066-104">Cet article décrit comment les API Web ASP.NET achemine une requête HTTP à une action particulière sur un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="f2066-105">Pour obtenir une vue d’ensemble du routage, consultez [routage dans ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f2066-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="f2066-106">Cet article examine les détails du processus de routage.</span><span class="sxs-lookup"><span data-stu-id="f2066-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="f2066-107">Si vous créez un projet d’API Web et trouver certaines demandes n’obtenez pas acheminé comme vous le souhaitez, j’espère que cet article vous aide à.</span><span class="sxs-lookup"><span data-stu-id="f2066-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="f2066-108">Routage comprend trois phases principales :</span><span class="sxs-lookup"><span data-stu-id="f2066-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="f2066-109">Correspondance avec l’URI à un modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f2066-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="f2066-110">Sélection d’un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-110">Selecting a controller.</span></span>
3. <span data-ttu-id="f2066-111">En sélectionnant une action.</span><span class="sxs-lookup"><span data-stu-id="f2066-111">Selecting an action.</span></span>

<span data-ttu-id="f2066-112">Vous pouvez remplacer certaines parties du processus avec vos propres comportements personnalisés.</span><span class="sxs-lookup"><span data-stu-id="f2066-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="f2066-113">Dans cet article, je décris le comportement par défaut.</span><span class="sxs-lookup"><span data-stu-id="f2066-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="f2066-114">À la fin, j’ai Notez les emplacements où vous pouvez personnaliser le comportement.</span><span class="sxs-lookup"><span data-stu-id="f2066-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="f2066-115">Modèles de routes</span><span class="sxs-lookup"><span data-stu-id="f2066-115">Route Templates</span></span>

<span data-ttu-id="f2066-116">Un modèle d’itinéraire est similaire à un chemin d’accès de l’URI, mais il peut avoir des valeurs d’espace réservé, indiquées par des accolades :</span><span class="sxs-lookup"><span data-stu-id="f2066-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="f2066-117">Lorsque vous créez un itinéraire, vous pouvez fournir des valeurs par défaut pour certains ou tous les espaces réservés :</span><span class="sxs-lookup"><span data-stu-id="f2066-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="f2066-118">Vous pouvez également fournir des contraintes qui limitent la façon dont un segment d’URI peut correspondre à un espace réservé :</span><span class="sxs-lookup"><span data-stu-id="f2066-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="f2066-119">Le framework essaie les segments dans le chemin d’accès URI au modèle.</span><span class="sxs-lookup"><span data-stu-id="f2066-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="f2066-120">Littéraux dans le modèle doivent correspondre exactement.</span><span class="sxs-lookup"><span data-stu-id="f2066-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="f2066-121">Un espace réservé correspond à n’importe quelle valeur, sauf si vous spécifiez des contraintes.</span><span class="sxs-lookup"><span data-stu-id="f2066-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="f2066-122">Le framework ne correspond pas à d’autres parties de l’URI, telles que le nom d’hôte ou les paramètres de requête.</span><span class="sxs-lookup"><span data-stu-id="f2066-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="f2066-123">Le framework sélectionne le premier itinéraire dans la table de routage qui correspond à l’URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="f2066-124">Il existe deux espaces réservés spéciaux : « {controller} » et « {action} ».</span><span class="sxs-lookup"><span data-stu-id="f2066-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="f2066-125">« {controller} » fournit le nom du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="f2066-126">« {action} » fournit le nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="f2066-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="f2066-127">Dans l’API Web, la convention habituelle consiste à omettre des « {action} ».</span><span class="sxs-lookup"><span data-stu-id="f2066-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="f2066-128">feuille de temps</span><span class="sxs-lookup"><span data-stu-id="f2066-128">Defaults</span></span>

<span data-ttu-id="f2066-129">Si vous fournissez des valeurs par défaut, l’itinéraire correspond un URI qui n’a pas de ces segments.</span><span class="sxs-lookup"><span data-stu-id="f2066-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="f2066-130">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f2066-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="f2066-131">L’URI «`http://localhost/api/products`» correspond à cet itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f2066-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="f2066-132">Le segment « {category} » est affecté à la valeur par défaut « tous ».</span><span class="sxs-lookup"><span data-stu-id="f2066-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="f2066-133">Dictionnaire d’itinéraires</span><span class="sxs-lookup"><span data-stu-id="f2066-133">Route Dictionary</span></span>

<span data-ttu-id="f2066-134">Si le framework trouve une correspondance pour un URI, il crée un dictionnaire qui contient la valeur pour chaque espace réservé.</span><span class="sxs-lookup"><span data-stu-id="f2066-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="f2066-135">Les clés sont les noms d’espace réservé, non compris entre accolades.</span><span class="sxs-lookup"><span data-stu-id="f2066-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="f2066-136">Les valeurs sont extraites dans le chemin d’accès URI ou dans les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="f2066-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="f2066-137">Le dictionnaire est stocké dans le **IHttpRouteData** objet.</span><span class="sxs-lookup"><span data-stu-id="f2066-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="f2066-138">Pendant cette phase de mise en correspondance de route, les espaces réservés « {action} » spécial « {controller} » sont traitées comme les autres espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="f2066-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="f2066-139">Ils sont simplement stockées dans le dictionnaire avec les autres valeurs.</span><span class="sxs-lookup"><span data-stu-id="f2066-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="f2066-140">Une valeur par défaut peut avoir la valeur spéciale **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="f2066-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="f2066-141">Si un espace réservé est affecté à cette valeur, la valeur n’est pas ajoutée au dictionnaire d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f2066-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="f2066-142">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f2066-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="f2066-143">Pour le chemin d’accès URI « api/produits », le dictionnaire d’itinéraires contiendra :</span><span class="sxs-lookup"><span data-stu-id="f2066-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="f2066-144">contrôleur : « produits »</span><span class="sxs-lookup"><span data-stu-id="f2066-144">controller: "products"</span></span>
- <span data-ttu-id="f2066-145">catégorie : « all »</span><span class="sxs-lookup"><span data-stu-id="f2066-145">category: "all"</span></span>

<span data-ttu-id="f2066-146">Pour « api/produits/toys/123 », toutefois, le dictionnaire d’itinéraires contiendra :</span><span class="sxs-lookup"><span data-stu-id="f2066-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="f2066-147">contrôleur : « produits »</span><span class="sxs-lookup"><span data-stu-id="f2066-147">controller: "products"</span></span>
- <span data-ttu-id="f2066-148">catégorie : « toys »</span><span class="sxs-lookup"><span data-stu-id="f2066-148">category: "toys"</span></span>
- <span data-ttu-id="f2066-149">ID : « 123 »</span><span class="sxs-lookup"><span data-stu-id="f2066-149">id: "123"</span></span>

<span data-ttu-id="f2066-150">Les valeurs par défaut peuvent également inclure une valeur qui n’apparaît pas n’importe où dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="f2066-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="f2066-151">Si l’itinéraire correspond à, cette valeur est stockée dans le dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="f2066-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="f2066-152">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f2066-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="f2066-153">Si le chemin d’accès de l’URI est « racine/api/8 », le dictionnaire contient deux valeurs :</span><span class="sxs-lookup"><span data-stu-id="f2066-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="f2066-154">contrôleur : « customers »</span><span class="sxs-lookup"><span data-stu-id="f2066-154">controller: "customers"</span></span>
- <span data-ttu-id="f2066-155">ID : « 8 »</span><span class="sxs-lookup"><span data-stu-id="f2066-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="f2066-156">Sélection d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="f2066-156">Selecting a Controller</span></span>

<span data-ttu-id="f2066-157">Sélection du contrôleur est gérée par le **IHttpControllerSelector.SelectController** (méthode).</span><span class="sxs-lookup"><span data-stu-id="f2066-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="f2066-158">Cette méthode prend un **HttpRequestMessage** instance et retourne un **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="f2066-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="f2066-159">L’implémentation par défaut est fournie par le **DefaultHttpControllerSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="f2066-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="f2066-160">Cette classe utilise un algorithme simple :</span><span class="sxs-lookup"><span data-stu-id="f2066-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="f2066-161">Rechercher dans le dictionnaire d’itinéraire pour la clé « controller ».</span><span class="sxs-lookup"><span data-stu-id="f2066-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="f2066-162">Prendre la valeur pour cette clé et ajoutez la chaîne « Controller » pour obtenir le nom de type de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="f2066-163">Recherchez un contrôleur d’API Web portant ce nom de type.</span><span class="sxs-lookup"><span data-stu-id="f2066-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="f2066-164">Par exemple, si le dictionnaire de l’itinéraire contient la paire clé-valeur « controller » = « produits », puis le type de contrôleur est « ProductsController ».</span><span class="sxs-lookup"><span data-stu-id="f2066-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="f2066-165">Si aucun type de correspondance, ou existe plusieurs correspondances, le framework retourne une erreur au client.</span><span class="sxs-lookup"><span data-stu-id="f2066-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="f2066-166">Dans l’étape 3, **DefaultHttpControllerSelector** utilise le **IHttpControllerTypeResolver** interface permettant d’obtenir la liste des types de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="f2066-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="f2066-167">L’implémentation par défaut de **IHttpControllerTypeResolver** retourne toutes les classes publiques qui implémentent (a) **IHttpController**, (b) ne sont pas abstraites et (c) ont un nom qui se termine par « Controller ».</span><span class="sxs-lookup"><span data-stu-id="f2066-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="f2066-168">Sélection d’action</span><span class="sxs-lookup"><span data-stu-id="f2066-168">Action Selection</span></span>

<span data-ttu-id="f2066-169">Après avoir sélectionné le contrôleur, le framework sélectionne l’action en appelant le **IHttpActionSelector.SelectAction** (méthode).</span><span class="sxs-lookup"><span data-stu-id="f2066-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="f2066-170">Cette méthode prend un **HttpControllerContext** et retourne un **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="f2066-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="f2066-171">L’implémentation par défaut est fournie par le **ApiControllerActionSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="f2066-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="f2066-172">Pour sélectionner une action, il examine les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f2066-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="f2066-173">La méthode HTTP de la demande.</span><span class="sxs-lookup"><span data-stu-id="f2066-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="f2066-174">L’espace réservé « {action} » dans le modèle d’itinéraire, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="f2066-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="f2066-175">Les paramètres des actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="f2066-176">Avant d’examiner l’algorithme de sélection, nous devons comprendre quelques informations sur les actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="f2066-177">**Les méthodes sur le contrôleur sont considérés comme « actions » ?**</span><span class="sxs-lookup"><span data-stu-id="f2066-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="f2066-178">Lorsque vous sélectionnez une action, le framework examine uniquement les méthodes d’instance publique sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="f2066-179">En outre, il exclut [« nom spécial »](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) méthodes (constructeurs, événements, les surcharges d’opérateur et ainsi de suite) et les méthodes héritées de la **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="f2066-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="f2066-180">**Méthodes HTTP.**</span><span class="sxs-lookup"><span data-stu-id="f2066-180">**HTTP Methods.**</span></span> <span data-ttu-id="f2066-181">Le framework choisit uniquement les actions qui correspondent à la méthode HTTP de la demande, déterminée comme suit :</span><span class="sxs-lookup"><span data-stu-id="f2066-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="f2066-182">Vous pouvez spécifier la méthode HTTP avec un attribut : **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, ou **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="f2066-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="f2066-183">Sinon, si le nom de la méthode de contrôleur commence par « Get », « Post », « Put », « Delete », « Head », « Options » ou « Patch », puis par convention l’action prend en charge cette méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2066-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="f2066-184">Si aucun des éléments ci-dessus, la méthode prend en charge POST.</span><span class="sxs-lookup"><span data-stu-id="f2066-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="f2066-185">**Liaisons de paramètres.**</span><span class="sxs-lookup"><span data-stu-id="f2066-185">**Parameter Bindings.**</span></span> <span data-ttu-id="f2066-186">Une liaison de paramètre est comment les API Web crée une valeur pour un paramètre.</span><span class="sxs-lookup"><span data-stu-id="f2066-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="f2066-187">Voici la règle par défaut pour la liaison de paramètre :</span><span class="sxs-lookup"><span data-stu-id="f2066-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="f2066-188">Types simples sont tirés de l’URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="f2066-189">Les types complexes sont tirés de corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="f2066-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="f2066-190">Tous les exemples de types simples le [types primitifs .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), ainsi que **DateTime**, **décimal**, **Guid**, **chaîne** , et **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="f2066-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="f2066-191">Pour chaque action, au moins un paramètre peut lire le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="f2066-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="f2066-192">Il est possible de remplacer les règles de liaison par défaut.</span><span class="sxs-lookup"><span data-stu-id="f2066-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="f2066-193">Consultez [liaison de paramètre de l’API Web sous le capot](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="f2066-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="f2066-194">En gardant, voici l’algorithme de sélection d’action.</span><span class="sxs-lookup"><span data-stu-id="f2066-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="f2066-195">Créer une liste de toutes les actions sur le contrôleur qui correspondent à la méthode de demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2066-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="f2066-196">Si le dictionnaire d’itinéraires a une « action », supprimez les actions dont le nom ne correspond pas à cette valeur.</span><span class="sxs-lookup"><span data-stu-id="f2066-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="f2066-197">Essayez de faire correspondre les paramètres d’action à l’URI, comme suit :</span><span class="sxs-lookup"><span data-stu-id="f2066-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="f2066-198">Pour chaque action, obtenez une liste des paramètres qui sont un type simple, où la liaison Obtient le paramètre de l’URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="f2066-199">Exclure les paramètres facultatifs.</span><span class="sxs-lookup"><span data-stu-id="f2066-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="f2066-200">Dans cette liste, essayez de trouver une correspondance pour chaque nom de paramètre, dans le dictionnaire d’itinéraires ou dans la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="f2066-201">Les correspondances respectent la casse et ne dépendent pas de l’ordre des paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2066-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="f2066-202">Sélectionnez une action dans lequel chaque paramètre dans la liste a une correspondance dans l’URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="f2066-203">Si une action plus répond à ces critères, choisir celui avec la plupart des correspondances de paramètre.</span><span class="sxs-lookup"><span data-stu-id="f2066-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="f2066-204">Ignorer les actions avec le **[NonAction]** attribut.</span><span class="sxs-lookup"><span data-stu-id="f2066-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="f2066-205">Étape #3 est probablement le plus à confusion.</span><span class="sxs-lookup"><span data-stu-id="f2066-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="f2066-206">L’idée fondamentale est qu’un paramètre peut obtenir sa valeur à partir de l’URI, à partir du corps de demande ou à partir d’une liaison personnalisée.</span><span class="sxs-lookup"><span data-stu-id="f2066-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="f2066-207">Pour les paramètres qui proviennent de l’URI, nous voulons vérifier que l’URI contienne en fait une valeur pour ce paramètre, dans le chemin d’accès (via le dictionnaire d’itinéraires) ou dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="f2066-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="f2066-208">Par exemple, considérez l’action suivante :</span><span class="sxs-lookup"><span data-stu-id="f2066-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="f2066-209">Le *id* le paramètre est lié à l’URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="f2066-210">Par conséquent, cette action peut correspondre uniquement à un URI qui contient une valeur pour « id », dans le dictionnaire d’itinéraires ou dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="f2066-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="f2066-211">Les paramètres facultatifs sont une exception, car ils sont facultatifs.</span><span class="sxs-lookup"><span data-stu-id="f2066-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="f2066-212">Pour un paramètre facultatif, il est OK si la liaison ne peut pas obtenir la valeur de l’URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="f2066-213">Les types complexes sont une exception pour une autre raison.</span><span class="sxs-lookup"><span data-stu-id="f2066-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="f2066-214">Un type complexe ne peut se lier à l’URI via une liaison personnalisée.</span><span class="sxs-lookup"><span data-stu-id="f2066-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="f2066-215">Mais dans ce cas, le framework ne peut pas savoir à l’avance le si le paramètre établirait une liaison avec un URI particulier.</span><span class="sxs-lookup"><span data-stu-id="f2066-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="f2066-216">Pour découvrir, elle aurait besoin d’appeler la liaison.</span><span class="sxs-lookup"><span data-stu-id="f2066-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="f2066-217">L’objectif de l’algorithme de sélection consiste à sélectionner une action à partir de la description statique, avant d’appeler toutes les liaisons.</span><span class="sxs-lookup"><span data-stu-id="f2066-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="f2066-218">Par conséquent, les types complexes sont exclus de l’algorithme de correspondance.</span><span class="sxs-lookup"><span data-stu-id="f2066-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="f2066-219">Une fois que l’action est sélectionnée, toutes les liaisons de paramètre sont appelés.</span><span class="sxs-lookup"><span data-stu-id="f2066-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="f2066-220">Résumé :</span><span class="sxs-lookup"><span data-stu-id="f2066-220">Summary:</span></span>

- <span data-ttu-id="f2066-221">L’action doit correspondre à la méthode HTTP de la demande.</span><span class="sxs-lookup"><span data-stu-id="f2066-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="f2066-222">Le nom d’action doit correspondre à l’entrée « action » dans le dictionnaire d’itinéraires, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="f2066-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="f2066-223">Pour chaque paramètre de l’action, si le paramètre provient de l’URI, puis le nom du paramètre doit être présent dans le dictionnaire d’itinéraires ou dans la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="f2066-224">(Les paramètres avec des types complexes et les paramètres facultatifs sont exclus.)</span><span class="sxs-lookup"><span data-stu-id="f2066-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="f2066-225">Essayez de faire correspondre le plus grand nombre de paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2066-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="f2066-226">La meilleure correspondance peut être une méthode sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2066-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="f2066-227">Exemple étendu</span><span class="sxs-lookup"><span data-stu-id="f2066-227">Extended Example</span></span>

<span data-ttu-id="f2066-228">Itinéraires :</span><span class="sxs-lookup"><span data-stu-id="f2066-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="f2066-229">Contrôleur :</span><span class="sxs-lookup"><span data-stu-id="f2066-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="f2066-230">Requête HTTP :</span><span class="sxs-lookup"><span data-stu-id="f2066-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="f2066-231">Acheminer la mise en correspondance</span><span class="sxs-lookup"><span data-stu-id="f2066-231">Route Matching</span></span>

<span data-ttu-id="f2066-232">L’URI correspond à l’itinéraire nommé « DefaultApi ».</span><span class="sxs-lookup"><span data-stu-id="f2066-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="f2066-233">Le dictionnaire de l’itinéraire contient les entrées suivantes :</span><span class="sxs-lookup"><span data-stu-id="f2066-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="f2066-234">contrôleur : « produits »</span><span class="sxs-lookup"><span data-stu-id="f2066-234">controller: "products"</span></span>
- <span data-ttu-id="f2066-235">ID : « 1 »</span><span class="sxs-lookup"><span data-stu-id="f2066-235">id: "1"</span></span>

<span data-ttu-id="f2066-236">Le dictionnaire d’itinéraire ne contient-elle pas les paramètres de chaîne de requête, la « version » et le « détails », mais ils sont toujours considérés lors de la sélection de l’action.</span><span class="sxs-lookup"><span data-stu-id="f2066-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="f2066-237">Sélection du contrôleur</span><span class="sxs-lookup"><span data-stu-id="f2066-237">Controller Selection</span></span>

<span data-ttu-id="f2066-238">À partir de l’entrée « controller » dans le dictionnaire d’itinéraires, le type de contrôleur est `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="f2066-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="f2066-239">Sélection d’action</span><span class="sxs-lookup"><span data-stu-id="f2066-239">Action Selection</span></span>

<span data-ttu-id="f2066-240">La requête HTTP est une requête GET.</span><span class="sxs-lookup"><span data-stu-id="f2066-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="f2066-241">Les actions de contrôleur qui prennent en charge de GET sont `GetAll`, `GetById`, et `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="f2066-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="f2066-242">Le dictionnaire d’itinéraire ne contient pas une entrée pour « action », nous n’avez pas besoin de correspondre au nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="f2066-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="f2066-243">Ensuite, nous essayons de correspondre aux noms de paramètre pour les actions, examinant uniquement les actions GET.</span><span class="sxs-lookup"><span data-stu-id="f2066-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="f2066-244">Action</span><span class="sxs-lookup"><span data-stu-id="f2066-244">Action</span></span> | <span data-ttu-id="f2066-245">Paramètres de correspondance</span><span class="sxs-lookup"><span data-stu-id="f2066-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="f2066-246">none</span><span class="sxs-lookup"><span data-stu-id="f2066-246">none</span></span> |
| `GetById` | <span data-ttu-id="f2066-247">« id »</span><span class="sxs-lookup"><span data-stu-id="f2066-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="f2066-248">« nom »</span><span class="sxs-lookup"><span data-stu-id="f2066-248">"name"</span></span> |

<span data-ttu-id="f2066-249">Notez que le *version* paramètre de `GetById` n’est pas considérée, s’agissant d’un paramètre facultatif.</span><span class="sxs-lookup"><span data-stu-id="f2066-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="f2066-250">Le `GetAll` méthode correspond à façon triviale.</span><span class="sxs-lookup"><span data-stu-id="f2066-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="f2066-251">Le `GetById` méthode correspond également à, car le dictionnaire de l’itinéraire contient « id ».</span><span class="sxs-lookup"><span data-stu-id="f2066-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="f2066-252">Le `FindProductsByName` méthode ne correspond pas.</span><span class="sxs-lookup"><span data-stu-id="f2066-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="f2066-253">Le `GetById` méthode wins, car il correspond à un seul paramètre, et aucun paramètre pour `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="f2066-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="f2066-254">La méthode est appelée avec les valeurs de paramètre suivantes :</span><span class="sxs-lookup"><span data-stu-id="f2066-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="f2066-255">*ID* = 1</span><span class="sxs-lookup"><span data-stu-id="f2066-255">*id* = 1</span></span>
- <span data-ttu-id="f2066-256">*version* = 1.5</span><span class="sxs-lookup"><span data-stu-id="f2066-256">*version* = 1.5</span></span>

<span data-ttu-id="f2066-257">Notez que même si *version* n’a été utilisé dans l’algorithme de sélection, la valeur du paramètre provient de la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="f2066-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="f2066-258">Points d’extension</span><span class="sxs-lookup"><span data-stu-id="f2066-258">Extension Points</span></span>

<span data-ttu-id="f2066-259">API Web fournit des points d’extension pour certaines parties du processus de routage.</span><span class="sxs-lookup"><span data-stu-id="f2066-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="f2066-260">Interface</span><span class="sxs-lookup"><span data-stu-id="f2066-260">Interface</span></span> | <span data-ttu-id="f2066-261">Description</span><span class="sxs-lookup"><span data-stu-id="f2066-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f2066-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="f2066-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="f2066-263">Sélectionne le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-263">Selects the controller.</span></span> |
| <span data-ttu-id="f2066-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="f2066-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="f2066-265">Obtient la liste des types de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-265">Gets the list of controller types.</span></span> <span data-ttu-id="f2066-266">Le **DefaultHttpControllerSelector** choisit le type de contrôleur à partir de cette liste.</span><span class="sxs-lookup"><span data-stu-id="f2066-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="f2066-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="f2066-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="f2066-268">Obtient la liste des assemblys de projet.</span><span class="sxs-lookup"><span data-stu-id="f2066-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="f2066-269">Le **IHttpControllerTypeResolver** interface utilise cette liste pour rechercher les types de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="f2066-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="f2066-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="f2066-271">Crée des instances de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="f2066-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="f2066-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="f2066-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="f2066-273">Sélectionne l’action.</span><span class="sxs-lookup"><span data-stu-id="f2066-273">Selects the action.</span></span> |
| <span data-ttu-id="f2066-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="f2066-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="f2066-275">Appelle l’action.</span><span class="sxs-lookup"><span data-stu-id="f2066-275">Invokes the action.</span></span> |

<span data-ttu-id="f2066-276">Pour fournir votre propre implémentation pour une de ces interfaces, utilisez le **Services** collection sur le **HttpConfiguration** objet :</span><span class="sxs-lookup"><span data-stu-id="f2066-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
