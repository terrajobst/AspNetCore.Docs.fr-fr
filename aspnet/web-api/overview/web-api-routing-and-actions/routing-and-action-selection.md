---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routage et sélection d’Action dans ASP.NET Web API | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043416"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="6e405-102">Routage et sélection d’Action dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6e405-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="6e405-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6e405-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6e405-104">Cet article décrit comment les API Web ASP.NET route une requête HTTP à une action particulière sur un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="6e405-105">Pour obtenir une vue d’ensemble du routage, consultez [le routage ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6e405-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="6e405-106">Cet article examine les détails du processus de routage.</span><span class="sxs-lookup"><span data-stu-id="6e405-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="6e405-107">Si vous créez un projet d’API Web et de rechercher certaines demandes n’obtenez pas routé comme vous le souhaitez, nous espérons que cet article vous aidera.</span><span class="sxs-lookup"><span data-stu-id="6e405-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="6e405-108">Routage comprend trois phases principales :</span><span class="sxs-lookup"><span data-stu-id="6e405-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="6e405-109">Mise en correspondance l’URI vers un modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="6e405-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="6e405-110">Sélection d’un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-110">Selecting a controller.</span></span>
3. <span data-ttu-id="6e405-111">En sélectionnant une action.</span><span class="sxs-lookup"><span data-stu-id="6e405-111">Selecting an action.</span></span>

<span data-ttu-id="6e405-112">Vous pouvez remplacer certaines parties du processus par vos propres comportements personnalisés.</span><span class="sxs-lookup"><span data-stu-id="6e405-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="6e405-113">Dans cet article, je décris le comportement par défaut.</span><span class="sxs-lookup"><span data-stu-id="6e405-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="6e405-114">À la fin, j’ai Notez les emplacements où vous pouvez personnaliser le comportement.</span><span class="sxs-lookup"><span data-stu-id="6e405-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="6e405-115">Modèles d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="6e405-115">Route Templates</span></span>

<span data-ttu-id="6e405-116">Un modèle d’itinéraire est semblable à un chemin d’accès de l’URI, mais il peut avoir des valeurs d’espace réservé, indiquées par des accolades :</span><span class="sxs-lookup"><span data-stu-id="6e405-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="6e405-117">Lorsque vous créez un itinéraire, vous pouvez fournir des valeurs par défaut pour certains ou tous les espaces réservés :</span><span class="sxs-lookup"><span data-stu-id="6e405-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="6e405-118">Vous pouvez également fournir des contraintes qui limitent la façon dont un segment d’URI peut correspondre à un espace réservé :</span><span class="sxs-lookup"><span data-stu-id="6e405-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="6e405-119">Le framework tente de correspondre les segments dans le chemin d’accès de l’URI pour le modèle.</span><span class="sxs-lookup"><span data-stu-id="6e405-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="6e405-120">Littéraux dans le modèle doivent correspondre exactement.</span><span class="sxs-lookup"><span data-stu-id="6e405-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="6e405-121">Un espace réservé correspond à n’importe quelle valeur, sauf si vous spécifiez des contraintes.</span><span class="sxs-lookup"><span data-stu-id="6e405-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="6e405-122">Le framework ne correspond pas à d’autres parties de l’URI, telles que le nom d’hôte ou les paramètres de requête.</span><span class="sxs-lookup"><span data-stu-id="6e405-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="6e405-123">Le framework sélectionne le premier itinéraire dans la table d’itinéraires qui correspond à l’URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="6e405-124">Il existe deux espaces réservés spéciaux : « {controller} » et « {action} ».</span><span class="sxs-lookup"><span data-stu-id="6e405-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="6e405-125">« {controller} » fournit le nom du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="6e405-126">« {action} » fournit le nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="6e405-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="6e405-127">Dans l’API Web, la convention habituelle est d’omettre « {action} ».</span><span class="sxs-lookup"><span data-stu-id="6e405-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="6e405-128">feuille de temps</span><span class="sxs-lookup"><span data-stu-id="6e405-128">Defaults</span></span>

<span data-ttu-id="6e405-129">Si vous fournissez des valeurs par défaut, l’itinéraire correspond un URI qui est manquant pour les segments.</span><span class="sxs-lookup"><span data-stu-id="6e405-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="6e405-130">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6e405-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="6e405-131">L’URI «`http://localhost/api/products`» correspond à cet itinéraire.</span><span class="sxs-lookup"><span data-stu-id="6e405-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="6e405-132">Le segment « {catégorie} » est affecté à la valeur par défaut « tous ».</span><span class="sxs-lookup"><span data-stu-id="6e405-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="6e405-133">Dictionnaire d’itinéraires</span><span class="sxs-lookup"><span data-stu-id="6e405-133">Route Dictionary</span></span>

<span data-ttu-id="6e405-134">Si l’infrastructure trouve une correspondance pour un URI, il crée un dictionnaire qui contient la valeur pour chaque espace réservé.</span><span class="sxs-lookup"><span data-stu-id="6e405-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="6e405-135">Les clés sont les noms d’espace réservé, non compris les accolades.</span><span class="sxs-lookup"><span data-stu-id="6e405-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="6e405-136">Les valeurs sont extraites dans le chemin d’accès URI ou dans les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="6e405-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="6e405-137">Le dictionnaire est stocké dans le **IHttpRouteData** objet.</span><span class="sxs-lookup"><span data-stu-id="6e405-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="6e405-138">Pendant cette phase de correspondance d’itinéraire, le spécial « {controller} » et les espaces réservés de « {action} » sont traités comme les autres espaces réservés.</span><span class="sxs-lookup"><span data-stu-id="6e405-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="6e405-139">Ils sont simplement stockées dans le dictionnaire avec les autres valeurs.</span><span class="sxs-lookup"><span data-stu-id="6e405-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="6e405-140">Une valeur par défaut peut avoir la valeur spéciale **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="6e405-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="6e405-141">Si un espace réservé est affecté à cette valeur, la valeur n’est pas ajoutée au dictionnaire d’itinéraires.</span><span class="sxs-lookup"><span data-stu-id="6e405-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="6e405-142">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6e405-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="6e405-143">Contient le chemin d’accès URI « api/products », le dictionnaire d’itinéraires :</span><span class="sxs-lookup"><span data-stu-id="6e405-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="6e405-144">contrôleur : « products »</span><span class="sxs-lookup"><span data-stu-id="6e405-144">controller: "products"</span></span>
- <span data-ttu-id="6e405-145">catégorie : « tous »</span><span class="sxs-lookup"><span data-stu-id="6e405-145">category: "all"</span></span>

<span data-ttu-id="6e405-146">Pour les « api/produits/jouets/123 », toutefois, le dictionnaire d’itinéraires contient :</span><span class="sxs-lookup"><span data-stu-id="6e405-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="6e405-147">contrôleur : « products »</span><span class="sxs-lookup"><span data-stu-id="6e405-147">controller: "products"</span></span>
- <span data-ttu-id="6e405-148">catégorie : « toys »</span><span class="sxs-lookup"><span data-stu-id="6e405-148">category: "toys"</span></span>
- <span data-ttu-id="6e405-149">ID : « 123 »</span><span class="sxs-lookup"><span data-stu-id="6e405-149">id: "123"</span></span>

<span data-ttu-id="6e405-150">Les valeurs par défaut peuvent également inclure une valeur qui n’apparaît nulle part dans le modèle d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="6e405-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="6e405-151">Si l’itinéraire correspond à, cette valeur est stockée dans le dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="6e405-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="6e405-152">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6e405-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="6e405-153">Si le chemin d’accès de l’URI est « racine/api/8 », le dictionnaire contient deux valeurs :</span><span class="sxs-lookup"><span data-stu-id="6e405-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="6e405-154">contrôleur : « customers »</span><span class="sxs-lookup"><span data-stu-id="6e405-154">controller: "customers"</span></span>
- <span data-ttu-id="6e405-155">ID : « 8 »</span><span class="sxs-lookup"><span data-stu-id="6e405-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="6e405-156">Sélection d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="6e405-156">Selecting a Controller</span></span>

<span data-ttu-id="6e405-157">Sélection du contrôleur est gérée par le **IHttpControllerSelector.SelectController** (méthode).</span><span class="sxs-lookup"><span data-stu-id="6e405-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="6e405-158">Cette méthode prend un **HttpRequestMessage** instance et retourne un **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="6e405-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="6e405-159">L’implémentation par défaut est fournie par le **DefaultHttpControllerSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="6e405-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="6e405-160">Cette classe utilise un algorithme simple :</span><span class="sxs-lookup"><span data-stu-id="6e405-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="6e405-161">Recherchez dans le dictionnaire d’itinéraire pour la clé « controller ».</span><span class="sxs-lookup"><span data-stu-id="6e405-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="6e405-162">Prendre la valeur de cette clé et ajouter la chaîne « Contrôleur » pour obtenir le nom de type de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="6e405-163">Recherchez le contrôleur d’API Web avec ce nom de type.</span><span class="sxs-lookup"><span data-stu-id="6e405-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="6e405-164">Par exemple, si le dictionnaire d’itinéraires contient la paire clé-valeur « contrôleur » = « products », puis le type de contrôleur est « ProductsController ».</span><span class="sxs-lookup"><span data-stu-id="6e405-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="6e405-165">Si aucun type de correspondance, ou existe plusieurs correspondances, le framework retourne une erreur au client.</span><span class="sxs-lookup"><span data-stu-id="6e405-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="6e405-166">Dans l’étape 3, **DefaultHttpControllerSelector** utilise le **IHttpControllerTypeResolver** interface à obtenir la liste des types de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="6e405-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="6e405-167">L’implémentation par défaut de **IHttpControllerTypeResolver** retourne toutes les classes publiques qui implémentent (a) **IHttpController**, (b) ne sont pas abstraites et (c) ont un nom qui se termine par « Controller ».</span><span class="sxs-lookup"><span data-stu-id="6e405-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="6e405-168">Sélection d’action</span><span class="sxs-lookup"><span data-stu-id="6e405-168">Action Selection</span></span>

<span data-ttu-id="6e405-169">Après avoir sélectionné le contrôleur, le framework sélectionne l’action en appelant le **IHttpActionSelector.SelectAction** (méthode).</span><span class="sxs-lookup"><span data-stu-id="6e405-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="6e405-170">Cette méthode prend un **HttpControllerContext** et retourne un **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="6e405-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="6e405-171">L’implémentation par défaut est fournie par le **ApiControllerActionSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="6e405-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="6e405-172">Pour sélectionner une action, il examine les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="6e405-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="6e405-173">La méthode HTTP de la demande.</span><span class="sxs-lookup"><span data-stu-id="6e405-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="6e405-174">L’espace réservé « {action} » dans le modèle d’itinéraire, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="6e405-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="6e405-175">Les paramètres des actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="6e405-176">Avant d’examiner l’algorithme de sélection, nous devons comprendre certains éléments concernant les actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="6e405-177">**Les méthodes sur le contrôleur sont considérées comme « actions » ?**</span><span class="sxs-lookup"><span data-stu-id="6e405-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="6e405-178">Lorsque vous sélectionnez une action, le framework examine uniquement les méthodes d’instance publique sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="6e405-179">En outre, il exclut [« nom spécial »](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) méthodes (constructeurs, événements, les surcharges d’opérateur et ainsi de suite) et les méthodes héritées de la **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="6e405-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="6e405-180">**Méthodes HTTP.**</span><span class="sxs-lookup"><span data-stu-id="6e405-180">**HTTP Methods.**</span></span> <span data-ttu-id="6e405-181">Le framework choisit uniquement les actions qui correspondent à la méthode HTTP de la demande, déterminée comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e405-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="6e405-182">Vous pouvez spécifier la méthode HTTP avec un attribut : **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, ou **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="6e405-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="6e405-183">Sinon, si le nom de la méthode de contrôleur commence par « Get », « Post », « Put », « Delete », « Head », « Options » ou « Patch », puis par convention l’action prend en charge cette méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e405-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="6e405-184">Si aucun des éléments ci-dessus, la méthode prend en charge POST.</span><span class="sxs-lookup"><span data-stu-id="6e405-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="6e405-185">**Liaisons de paramètres.**</span><span class="sxs-lookup"><span data-stu-id="6e405-185">**Parameter Bindings.**</span></span> <span data-ttu-id="6e405-186">Un paramètre de liaison est comment API Web crée une valeur pour un paramètre.</span><span class="sxs-lookup"><span data-stu-id="6e405-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="6e405-187">Voici la règle par défaut pour la liaison de paramètre :</span><span class="sxs-lookup"><span data-stu-id="6e405-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="6e405-188">Types simples sont tirées de l’URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="6e405-189">Les types complexes sont effectuées à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="6e405-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="6e405-190">Tous les exemples de types simples le [types primitifs .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), ainsi que **DateTime**, **décimal**, **Guid**, **chaîne** , et **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="6e405-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="6e405-191">Pour chaque action, au moins un paramètre peut lire le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="6e405-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="6e405-192">Il est possible de remplacer les règles de liaison par défaut.</span><span class="sxs-lookup"><span data-stu-id="6e405-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="6e405-193">Consultez [liaison WebAPI paramètre sous le capot](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e405-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="6e405-194">Fond, voici l’algorithme de sélection d’action.</span><span class="sxs-lookup"><span data-stu-id="6e405-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="6e405-195">Créer une liste de toutes les actions sur le contrôleur qui correspondent à la méthode de demande HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e405-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="6e405-196">Si le dictionnaire d’itinéraires a une « action », supprimez les actions dont le nom ne correspond pas à cette valeur.</span><span class="sxs-lookup"><span data-stu-id="6e405-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="6e405-197">Essayez de faire correspondre les paramètres d’action à l’URI, comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e405-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="6e405-198">Pour chaque action, obtenir la liste des paramètres qui sont un type simple, où la liaison est le paramètre de l’URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="6e405-199">Exclure les paramètres facultatifs.</span><span class="sxs-lookup"><span data-stu-id="6e405-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="6e405-200">Dans cette liste, essayez de rechercher une correspondance pour chaque nom de paramètre, dans le dictionnaire d’itinéraires ou dans la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="6e405-201">Les correspondances respectent la casse et ne dépendent pas de l’ordre des paramètres.</span><span class="sxs-lookup"><span data-stu-id="6e405-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="6e405-202">Sélectionnez une action dans lequel chaque paramètre dans la liste a une correspondance dans l’URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="6e405-203">Si plus d’une action répond à ces critères, choisissez l’une avec la plupart des correspondances de paramètre.</span><span class="sxs-lookup"><span data-stu-id="6e405-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="6e405-204">Ignorer les actions avec le **[NonAction]** attribut.</span><span class="sxs-lookup"><span data-stu-id="6e405-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="6e405-205">Étape #3 est probablement le plus simple.</span><span class="sxs-lookup"><span data-stu-id="6e405-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="6e405-206">L’idée de base est qu’un paramètre peut obtenir sa valeur de l’URI, à partir du corps de la demande ou à partir d’une liaison personnalisée.</span><span class="sxs-lookup"><span data-stu-id="6e405-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="6e405-207">Pour les paramètres qui proviennent de l’URI, que vous souhaitez vous assurer que l’URI contienne réellement une valeur pour ce paramètre, dans le chemin d’accès (via le dictionnaire d’itinéraires) ou dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="6e405-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="6e405-208">Par exemple, considérez l’action suivante :</span><span class="sxs-lookup"><span data-stu-id="6e405-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="6e405-209">Le *id* le paramètre est lié à l’URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="6e405-210">Par conséquent, cette action peut correspondre uniquement à un URI qui contient une valeur pour « id », dans le dictionnaire d’itinéraires ou dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="6e405-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="6e405-211">Paramètres facultatifs sont une exception, car ils sont facultatifs.</span><span class="sxs-lookup"><span data-stu-id="6e405-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="6e405-212">Pour un paramètre facultatif, il est OK si la liaison ne peut pas obtenir la valeur de l’URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="6e405-213">Les types complexes sont une exception pour une autre raison.</span><span class="sxs-lookup"><span data-stu-id="6e405-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="6e405-214">Un type complexe ne peut se lier à l’URI via une liaison personnalisée.</span><span class="sxs-lookup"><span data-stu-id="6e405-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="6e405-215">Mais dans ce cas, le framework ne peut pas savoir à l’avance le si le paramètre crée une liaison avec un URI particulier.</span><span class="sxs-lookup"><span data-stu-id="6e405-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="6e405-216">Pour découvrir, il faudrait appeler la liaison.</span><span class="sxs-lookup"><span data-stu-id="6e405-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="6e405-217">Est de l’objectif de l’algorithme de sélection pour sélectionner une action à partir de la description statique, avant d’appeler toutes les liaisons.</span><span class="sxs-lookup"><span data-stu-id="6e405-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="6e405-218">Par conséquent, les types complexes sont exclus de l’algorithme de correspondance.</span><span class="sxs-lookup"><span data-stu-id="6e405-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="6e405-219">Une fois que l’action est sélectionnée, toutes les liaisons de paramètre sont appelés.</span><span class="sxs-lookup"><span data-stu-id="6e405-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="6e405-220">Résumé :</span><span class="sxs-lookup"><span data-stu-id="6e405-220">Summary:</span></span>

- <span data-ttu-id="6e405-221">L’action doit correspondre à la méthode HTTP de la demande.</span><span class="sxs-lookup"><span data-stu-id="6e405-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="6e405-222">Le nom de l’action doit correspondre à l’entrée « action » dans le dictionnaire d’itinéraire, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="6e405-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="6e405-223">Pour chaque paramètre de l’action, si le paramètre provient de l’URI, puis le nom du paramètre doit être présent dans le dictionnaire d’itinéraires ou dans la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="6e405-224">(Les paramètres avec des types complexes et les paramètres facultatifs sont exclus.)</span><span class="sxs-lookup"><span data-stu-id="6e405-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="6e405-225">Essayez de faire correspondre le plus grand nombre de paramètres.</span><span class="sxs-lookup"><span data-stu-id="6e405-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="6e405-226">La meilleure correspondance peut être une méthode sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="6e405-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="6e405-227">Exemple étendu</span><span class="sxs-lookup"><span data-stu-id="6e405-227">Extended Example</span></span>

<span data-ttu-id="6e405-228">Itinéraires :</span><span class="sxs-lookup"><span data-stu-id="6e405-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="6e405-229">Contrôleur :</span><span class="sxs-lookup"><span data-stu-id="6e405-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="6e405-230">Requête HTTP :</span><span class="sxs-lookup"><span data-stu-id="6e405-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="6e405-231">Itinéraire correspondant</span><span class="sxs-lookup"><span data-stu-id="6e405-231">Route Matching</span></span>

<span data-ttu-id="6e405-232">L’URI correspond à l’itinéraire nommé « DefaultApi ».</span><span class="sxs-lookup"><span data-stu-id="6e405-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="6e405-233">Le dictionnaire d’itinéraires contient les entrées suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e405-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="6e405-234">contrôleur : « products »</span><span class="sxs-lookup"><span data-stu-id="6e405-234">controller: "products"</span></span>
- <span data-ttu-id="6e405-235">ID : « 1 »</span><span class="sxs-lookup"><span data-stu-id="6e405-235">id: "1"</span></span>

<span data-ttu-id="6e405-236">Le dictionnaire d’itinéraires ne contient pas les paramètres de chaîne de requête, « version » et « détails », mais ils sont toujours considérés lors de la sélection de l’action.</span><span class="sxs-lookup"><span data-stu-id="6e405-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="6e405-237">Sélection du contrôleur</span><span class="sxs-lookup"><span data-stu-id="6e405-237">Controller Selection</span></span>

<span data-ttu-id="6e405-238">À partir de l’entrée « contrôleur » dans le dictionnaire d’itinéraire, le type de contrôleur est `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6e405-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="6e405-239">Sélection d’action</span><span class="sxs-lookup"><span data-stu-id="6e405-239">Action Selection</span></span>

<span data-ttu-id="6e405-240">La requête HTTP est une demande GET.</span><span class="sxs-lookup"><span data-stu-id="6e405-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="6e405-241">Les actions de contrôleur qui prend en charge GET sont `GetAll`, `GetById`, et `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="6e405-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="6e405-242">Le dictionnaire d’itinéraires ne contient pas d’entrée pour « action », nous n’avez pas besoin de correspondre au nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="6e405-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="6e405-243">Ensuite, nous essayons de faire correspondre les noms de paramètre pour les actions, recherche uniquement sur les actions de GET.</span><span class="sxs-lookup"><span data-stu-id="6e405-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="6e405-244">Action</span><span class="sxs-lookup"><span data-stu-id="6e405-244">Action</span></span> | <span data-ttu-id="6e405-245">Paramètres de correspondance</span><span class="sxs-lookup"><span data-stu-id="6e405-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="6e405-246">aucun</span><span class="sxs-lookup"><span data-stu-id="6e405-246">none</span></span> |
| `GetById` | <span data-ttu-id="6e405-247">« id »</span><span class="sxs-lookup"><span data-stu-id="6e405-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="6e405-248">« nom »</span><span class="sxs-lookup"><span data-stu-id="6e405-248">"name"</span></span> |

<span data-ttu-id="6e405-249">Notez que la *version* paramètre de `GetById` n’est pas considérée, car il s’agit d’un paramètre facultatif.</span><span class="sxs-lookup"><span data-stu-id="6e405-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="6e405-250">Le `GetAll` méthodes correspondent aux plus simplement.</span><span class="sxs-lookup"><span data-stu-id="6e405-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="6e405-251">Le `GetById` méthode correspond également à, car le dictionnaire d’itinéraires contient « id ».</span><span class="sxs-lookup"><span data-stu-id="6e405-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="6e405-252">Le `FindProductsByName` méthode ne correspond pas.</span><span class="sxs-lookup"><span data-stu-id="6e405-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="6e405-253">Le `GetById` méthode wins, car elle correspond à un paramètre, et aucun paramètre pour `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="6e405-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="6e405-254">La méthode est appelée avec les valeurs de paramètre suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e405-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="6e405-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="6e405-255">*id* = 1</span></span>
- <span data-ttu-id="6e405-256">*version* = 1.5</span><span class="sxs-lookup"><span data-stu-id="6e405-256">*version* = 1.5</span></span>

<span data-ttu-id="6e405-257">Notez que même si *version* n’a été utilisé dans l’algorithme de sélection, la valeur du paramètre provient de la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="6e405-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="6e405-258">Points d’extension</span><span class="sxs-lookup"><span data-stu-id="6e405-258">Extension Points</span></span>

<span data-ttu-id="6e405-259">API Web fournit des points d’extension pour certaines parties du processus de routage.</span><span class="sxs-lookup"><span data-stu-id="6e405-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="6e405-260">Interface</span><span class="sxs-lookup"><span data-stu-id="6e405-260">Interface</span></span> | <span data-ttu-id="6e405-261">Description</span><span class="sxs-lookup"><span data-stu-id="6e405-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6e405-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="6e405-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="6e405-263">Sélectionne le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-263">Selects the controller.</span></span> |
| <span data-ttu-id="6e405-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="6e405-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="6e405-265">Obtient la liste des types de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-265">Gets the list of controller types.</span></span> <span data-ttu-id="6e405-266">Le **DefaultHttpControllerSelector** choisit le type de contrôleur à partir de cette liste.</span><span class="sxs-lookup"><span data-stu-id="6e405-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="6e405-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="6e405-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="6e405-268">Obtient la liste des assemblys de projet.</span><span class="sxs-lookup"><span data-stu-id="6e405-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="6e405-269">Le **IHttpControllerTypeResolver** interface utilise cette liste pour rechercher les types de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="6e405-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="6e405-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="6e405-271">Crée des instances de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e405-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="6e405-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="6e405-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="6e405-273">Sélectionne l’action.</span><span class="sxs-lookup"><span data-stu-id="6e405-273">Selects the action.</span></span> |
| <span data-ttu-id="6e405-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="6e405-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="6e405-275">Appelle l’action.</span><span class="sxs-lookup"><span data-stu-id="6e405-275">Invokes the action.</span></span> |

<span data-ttu-id="6e405-276">Pour fournir votre propre implémentation d’une de ces interfaces, utilisez le **Services** collection sur le **HttpConfiguration** objet :</span><span class="sxs-lookup"><span data-stu-id="6e405-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
