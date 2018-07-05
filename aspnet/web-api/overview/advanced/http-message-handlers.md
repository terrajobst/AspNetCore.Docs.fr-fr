---
uid: web-api/overview/advanced/http-message-handlers
title: Gestionnaires de messages HTTP dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: daf5bac8203beca3be0036b798188e373b02212a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394519"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="ddf6a-102">Gestionnaires de messages HTTP dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ddf6a-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ddf6a-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ddf6a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ddf6a-104">Un *Gestionnaire de messages* est une classe qui reçoit une requête HTTP et renvoie une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="ddf6a-105">Gestionnaires de messages de dérivent de la classe abstraite **HttpMessageHandler** classe.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="ddf6a-106">En règle générale, une série de gestionnaires de messages sont chaînées ensemble.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="ddf6a-107">Le premier gestionnaire reçoit une requête HTTP effectue un traitement et donne la demande au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="ddf6a-108">À un moment donné, la réponse est créée et remonte la chaîne.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="ddf6a-109">Ce modèle est appelé un *délégation* gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="ddf6a-110">Gestionnaires de messages côté serveur</span><span class="sxs-lookup"><span data-stu-id="ddf6a-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="ddf6a-111">Sur le côté serveur, le pipeline API Web utilise des gestionnaires de messages intégrés :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="ddf6a-112">**Ftpserver** Obtient la demande à partir de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="ddf6a-113">**HttpRoutingDispatcher** distribue la demande en fonction de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="ddf6a-114">**HttpControllerDispatcher** envoie la demande à un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="ddf6a-115">Vous pouvez ajouter des gestionnaires personnalisés au pipeline.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="ddf6a-116">Gestionnaires de messages conviennent pour les problèmes transversaux qui opèrent au niveau de HTTP messages (plutôt que les actions de contrôleur).</span><span class="sxs-lookup"><span data-stu-id="ddf6a-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="ddf6a-117">Par exemple, un gestionnaire de messages peut :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-117">For example, a message handler might:</span></span>

- <span data-ttu-id="ddf6a-118">Lire ou modifier les en-têtes de demande.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-118">Read or modify request headers.</span></span>
- <span data-ttu-id="ddf6a-119">Ajouter un en-tête de réponse aux réponses.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-119">Add a response header to responses.</span></span>
- <span data-ttu-id="ddf6a-120">Valider les demandes avant qu’ils atteignent le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="ddf6a-121">Ce diagramme illustre deux gestionnaires personnalisés insérés dans le pipeline :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="ddf6a-122">Du côté client, HttpClient utilise également des gestionnaires de messages.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="ddf6a-123">Pour plus d’informations, consultez [gestionnaires de messages HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="ddf6a-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="ddf6a-124">Gestionnaires de messages personnalisés</span><span class="sxs-lookup"><span data-stu-id="ddf6a-124">Custom Message Handlers</span></span>

<span data-ttu-id="ddf6a-125">Pour écrire un gestionnaire de messages personnalisés, dérivez de **System.Net.Http.DelegatingHandler** et remplacer le **SendAsync** (méthode).</span><span class="sxs-lookup"><span data-stu-id="ddf6a-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="ddf6a-126">Cette méthode a la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="ddf6a-127">La méthode accepte un **HttpRequestMessage** comme entrée et retourne de façon asynchrone un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="ddf6a-128">Une implémentation classique effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="ddf6a-129">Traiter le message de demande.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-129">Process the request message.</span></span>
2. <span data-ttu-id="ddf6a-130">Appelez `base.SendAsync` pour envoyer la demande au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="ddf6a-131">Le gestionnaire interne retourne un message de réponse.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-131">The inner handler returns a response message.</span></span> <span data-ttu-id="ddf6a-132">(Cette étape est asynchrone).</span><span class="sxs-lookup"><span data-stu-id="ddf6a-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="ddf6a-133">Traiter la réponse et le renvoyer à l’appelant.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="ddf6a-134">Voici un exemple simple :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="ddf6a-135">L’appel à `base.SendAsync` est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="ddf6a-136">Si le gestionnaire effectue tout travail après cet appel, utilisez le **await** mot clé, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="ddf6a-137">Un gestionnaire de délégation peut également ignorer le gestionnaire interne et créer directement la réponse :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="ddf6a-138">Si une délégation de gestionnaire crée la réponse sans appeler `base.SendAsync`, la requête ignore le reste du pipeline.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="ddf6a-139">Cela peut être utile pour un gestionnaire qui valide la demande (création d’une réponse d’erreur).</span><span class="sxs-lookup"><span data-stu-id="ddf6a-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="ddf6a-140">Ajout d’un gestionnaire au Pipeline</span><span class="sxs-lookup"><span data-stu-id="ddf6a-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="ddf6a-141">Pour ajouter un gestionnaire de messages côté serveur, ajoutez le gestionnaire à la **HttpConfiguration.MessageHandlers** collection.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="ddf6a-142">Si vous avez utilisé le modèle « Application Web ASP.NET MVC 4 » pour créer le projet, vous pouvez effectuer ceci dans le **WebApiConfig** classe :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="ddf6a-143">Gestionnaires de messages sont appelés dans l’ordre dans lequel ils apparaissent dans **MessageHandlers** collection.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="ddf6a-144">Car ils sont imbriqués, le message de réponse sont transmises dans l’autre direction.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="ddf6a-145">Autrement dit, le dernier gestionnaire est le premier à recevoir le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="ddf6a-146">Notez que vous n’avez pas besoin de définir des gestionnaires internes ; l’infrastructure API Web connecte automatiquement les gestionnaires de messages.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="ddf6a-147">Si vous êtes [auto-hébergement](../older-versions/self-host-a-web-api.md), créez une instance de la **HttpSelfHostConfiguration** classe et ajoutez les gestionnaires pour le **MessageHandlers** collection.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="ddf6a-148">Maintenant nous allons examiner quelques exemples de gestionnaires de messages personnalisés.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="ddf6a-149">Exemple : X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="ddf6a-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="ddf6a-150">X-HTTP-Method-Override est un en-tête HTTP non standard.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="ddf6a-151">Il est conçu pour les clients qui ne peuvent pas envoyer de certains types de demande HTTP, telles que PUT ou DELETE.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="ddf6a-152">Au lieu de cela, le client envoie une demande POST et définit l’en-tête X-HTTP-Method-Override à la méthode de votre choix.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="ddf6a-153">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="ddf6a-154">Voici un gestionnaire de messages qui ajoute la prise en charge de X-HTTP-Method-Override :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="ddf6a-155">Dans le **SendAsync** (méthode), le gestionnaire vérifie si le message de demande est une demande POST, et s’il contient l’en-tête X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="ddf6a-156">Dans ce cas, il valide la valeur d’en-tête, puis modifie la méthode de demande.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="ddf6a-157">Enfin, le gestionnaire appelle `base.SendAsync` pour transférer le message au gestionnaire suivant.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="ddf6a-158">Lorsque la demande atteint le **HttpControllerDispatcher** (classe), **HttpControllerDispatcher** achemine la demande selon la méthode de demande de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="ddf6a-159">Exemple : Ajout d’un en-tête de réponse personnalisée</span><span class="sxs-lookup"><span data-stu-id="ddf6a-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="ddf6a-160">Voici un gestionnaire de messages qui ajoute un en-tête personnalisé à chaque message de réponse :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="ddf6a-161">Tout d’abord, le gestionnaire appelle `base.SendAsync` de transmettre la demande au Gestionnaire de messages interne.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="ddf6a-162">Le gestionnaire interne retourne un message de réponse, mais il le fait à l’aide de manière asynchrone un **tâche&lt;T&gt;**  objet.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="ddf6a-163">Le message de réponse n’est pas disponible tant que `base.SendAsync` se termine de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="ddf6a-164">Cet exemple utilise le **await** mot clé pour effectuer le travail asynchrone après `SendAsync` se termine.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="ddf6a-165">Si vous ciblez .NET Framework 4.0, utilisez le **tâche**&lt;T&gt;**. ContinueWith** méthode :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="ddf6a-166">Exemple : La vérification pour obtenir une clé API</span><span class="sxs-lookup"><span data-stu-id="ddf6a-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="ddf6a-167">Certains services web nécessitent des clients à inclure une clé API dans leur demande.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="ddf6a-168">L’exemple suivant montre comment un gestionnaire de messages peut vérifier les demandes pour une clé API valide :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="ddf6a-169">Ce gestionnaire recherche la clé API dans la chaîne de requête URI.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="ddf6a-170">(Pour cet exemple, nous partons du principe que la clé est une chaîne statique.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="ddf6a-171">Une implémentation réelle utiliseriez sans doute une validation plus complexe.) Si la chaîne de requête contient la clé, le gestionnaire transmet la demande au gestionnaire interne.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="ddf6a-172">Si la demande n’a pas d’une clé valide, le gestionnaire crée un message de réponse avec état 403, interdit.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="ddf6a-173">Dans ce cas, le gestionnaire n’appelle pas `base.SendAsync`, par conséquent, le gestionnaire interne ne reçoit jamais de la demande, de même le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="ddf6a-174">Par conséquent, le contrôleur peut supposer que toutes les demandes entrantes ont une clé API valide.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="ddf6a-175">Si la clé d’API s’applique uniquement à certaines actions de contrôleur, envisagez d’utiliser un filtre d’action au lieu d’un gestionnaire de messages.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="ddf6a-176">Filtres d’action exécutent après l’exécution de routage de l’URI.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="ddf6a-177">Gestionnaires de messages d’itinéraire</span><span class="sxs-lookup"><span data-stu-id="ddf6a-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="ddf6a-178">Gestionnaires dans le **HttpConfiguration.MessageHandlers** collection s’appliquent globalement.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="ddf6a-179">Ou bien, vous pouvez ajouter un gestionnaire de messages à un itinéraire spécifique lorsque vous définissez l’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="ddf6a-180">Dans cet exemple, si l’URI de demande correspond à « Route2 », la demande est distribuée à `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="ddf6a-181">Le diagramme suivant illustre le pipeline pour ces deux itinéraires :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="ddf6a-182">Notez que `MessageHandler2` remplace la valeur par défaut **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="ddf6a-183">Dans cet exemple, `MessageHandler2` crée la réponse, et les requêtes correspondant à « Route2 » accédez jamais à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="ddf6a-184">Cela vous permet de remplacer l’ensemble du mécanisme contrôleur API Web avec votre propre point de terminaison personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="ddf6a-185">Vous pouvez également un gestionnaire de messages par route peut déléguer au **HttpControllerDispatcher**, qui ensuite envoie à un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ddf6a-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="ddf6a-186">Le code suivant montre comment configurer cet itinéraire :</span><span class="sxs-lookup"><span data-stu-id="ddf6a-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
