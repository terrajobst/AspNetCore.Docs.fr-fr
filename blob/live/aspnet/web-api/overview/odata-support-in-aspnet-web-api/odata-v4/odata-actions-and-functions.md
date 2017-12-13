---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: "Actions et fonctions dans OData v4, à l’aide d’ASP.NET Web API 2.2 | Documents Microsoft"
author: MikeWasson
description: "Dans OData, les fonctions et les actions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas définies facilement comme des opérations CRUD sur les entités. Ce didacticiel montre comment..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="6a1f7-104">Actions et fonctions dans OData v4, à l’aide d’ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="6a1f7-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="6a1f7-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6a1f7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6a1f7-106">Dans OData, les fonctions et les actions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas définies facilement comme des opérations CRUD sur les entités.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="6a1f7-107">Ce didacticiel montre comment ajouter des fonctions et les actions à un point de terminaison de v4 OData, à l’aide de Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="6a1f7-108">Le didacticiel s’appuie sur le didacticiel [créer un Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="6a1f7-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6a1f7-109">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="6a1f7-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6a1f7-110">2.2 d’API Web</span><span class="sxs-lookup"><span data-stu-id="6a1f7-110">Web API 2.2</span></span>
> - <span data-ttu-id="6a1f7-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="6a1f7-111">OData v4</span></span>
> - [<span data-ttu-id="6a1f7-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="6a1f7-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="6a1f7-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6a1f7-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="6a1f7-114">Versions du didacticiels</span><span class="sxs-lookup"><span data-stu-id="6a1f7-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="6a1f7-115">Pour OData Version 3, consultez [Actions OData dans ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="6a1f7-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="6a1f7-116">La différence entre *actions* et *fonctions* que les actions peuvent avoir des effets secondaires, et les fonctions ne sont pas.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="6a1f7-117">Les actions et les fonctions peuvent retourner des données.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-117">Both actions and functions can return data.</span></span> <span data-ttu-id="6a1f7-118">Certaines utilisations pour les actions sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-118">Some uses for actions include:</span></span>

- <span data-ttu-id="6a1f7-119">Transactions complexes.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-119">Complex transactions.</span></span>
- <span data-ttu-id="6a1f7-120">La manipulation de plusieurs entités à la fois.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="6a1f7-121">Autoriser les mises à jour uniquement certaines propriétés d’une entité.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="6a1f7-122">Envoi de données qui n’est pas une entité.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="6a1f7-123">Les fonctions sont utiles pour renvoyer des informations qui ne correspondant pas directement à une entité ou une collection.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="6a1f7-124">Une action (ou une fonction) peut cibler une seule entité ou une collection.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="6a1f7-125">Dans la terminologie d’OData, il s’agit du *liaison*.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="6a1f7-126">Vous pouvez également avoir &quot;indépendant&quot; actions/fonctions sont appelées en tant qu’opérations statiques sur le service.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="6a1f7-127">Exemple : Ajout d’une Action</span><span class="sxs-lookup"><span data-stu-id="6a1f7-127">Example: Adding an Action</span></span>

<span data-ttu-id="6a1f7-128">Nous allons définir une action pour évaluer un produit.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="6a1f7-129">Ce didacticiel s’appuie sur le didacticiel [créer un Using ASP.NET Web API 2 OData v4 point de terminaison](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="6a1f7-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="6a1f7-130">Tout d’abord, ajoutez un `ProductRating` modèle pour représenter les évaluations.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="6a1f7-131">Ajoutez également une **DbSet** à la `ProductsContext` classe, afin que EF crée une table de contrôle d’accès dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="6a1f7-132">Ajoutez l’Action à l’EDM</span><span class="sxs-lookup"><span data-stu-id="6a1f7-132">Add the Action to the EDM</span></span>

<span data-ttu-id="6a1f7-133">Dans WebApiConfig.cs, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="6a1f7-134">Le **EntityTypeConfiguration.Action** méthode ajoute une action à l’entity data model (EDM).</span><span class="sxs-lookup"><span data-stu-id="6a1f7-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="6a1f7-135">Le **paramètre** méthode spécifie un paramètre typé pour l’action.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="6a1f7-136">Ce code définit également l’espace de noms pour le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="6a1f7-137">L’espace de noms est importante, car l’URI pour l’action inclut le nom qualifié complet :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="6a1f7-138">Dans une configuration IIS par défaut, le point de cette URL entraîne IIS retourner l’erreur 404.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="6a1f7-139">Vous pouvez résoudre ce problème en ajoutant la section suivante à votre fichier Web.Config :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="6a1f7-140">Ajouter une méthode de contrôleur pour l’Action</span><span class="sxs-lookup"><span data-stu-id="6a1f7-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="6a1f7-141">Pour activer la &quot;taux&quot; action, ajoutez la méthode suivante à `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="6a1f7-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="6a1f7-142">Notez que le nom de méthode correspond au nom de l’action.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="6a1f7-143">Le **[HttpPost]** attribut spécifie la méthode est une méthode HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="6a1f7-144">Pour appeler l’action, le client envoie une requête HTTP POST comme suit :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="6a1f7-145">Le &quot;taux&quot; action est liée à des instances de produit, par conséquent, l’URI de l’action est le nom qualifié complet des actions ajouté à l’URI de l’entité.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="6a1f7-146">(Rappelez-vous que nous avons défini l’espace de noms EDM &quot;ProductService&quot;, de sorte que le nom qualifié complet est &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="6a1f7-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="6a1f7-147">Le corps de la demande contient les paramètres d’action comme une charge utile JSON.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="6a1f7-148">API Web convertit automatiquement la charge JSON pour un **ODataActionParameters** objet, qui est simplement un dictionnaire de valeurs de paramètre.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="6a1f7-149">Ce dictionnaire permet d’accéder aux paramètres dans votre méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="6a1f7-150">Si le client envoie les paramètres d’action au bon format, la valeur de **ModelState.IsValid** a la valeur false.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="6a1f7-151">Contrôler cet indicateur dans votre méthode de contrôleur et de retourner une erreur si **IsValid** a la valeur false.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="6a1f7-152">Exemple : Ajout d’une fonction</span><span class="sxs-lookup"><span data-stu-id="6a1f7-152">Example: Adding a Function</span></span>

<span data-ttu-id="6a1f7-153">Maintenant vous allez ajouter une fonction OData qui retourne le produit plus coûteux.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="6a1f7-154">Comme avant, la première étape ajoute la fonction dans le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="6a1f7-155">Dans WebApiConfig.cs, ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="6a1f7-156">Dans ce cas, la fonction est liée à la collection de produits, plutôt qu’à des instances individuelles de produit.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="6a1f7-157">Les clients appeler la fonction en envoyant une demande GET :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="6a1f7-158">Voici la méthode de contrôleur pour cette fonction :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="6a1f7-159">Notez que le nom de méthode correspond au nom de fonction.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="6a1f7-160">Le **[HttpGet]** attribut spécifie la méthode est une méthode HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="6a1f7-161">Voici la réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="6a1f7-162">Exemple : Ajout d’une fonction non liée</span><span class="sxs-lookup"><span data-stu-id="6a1f7-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="6a1f7-163">L’exemple précédent a une fonction liée à une collection.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="6a1f7-164">Dans l’exemple suivant, nous allons créer une *indépendant* (fonction).</span><span class="sxs-lookup"><span data-stu-id="6a1f7-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="6a1f7-165">Indépendant de fonctions est appelé en tant qu’opérations statiques sur le service.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="6a1f7-166">La fonction dans cet exemple renvoie la taxe pour un code postal donné.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="6a1f7-167">Dans le fichier WebApiConfig, ajoutez la fonction à l’EDM :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="6a1f7-168">Notez que nous appelons **fonction** directement sur le **odatamodelbuilder ayant**, plutôt qu’au type d’entité ou de la collection.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="6a1f7-169">Le Générateur de modèles vous indique que la fonction est indépendante.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="6a1f7-170">Voici la méthode du contrôleur qui implémente la fonction :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="6a1f7-171">Il n’a pas d’importance quel contrôleur Web API vous placez cette méthode dans.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="6a1f7-172">Vous pouvez le placer `ProductsController`, ou définir un contrôleur distinct.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="6a1f7-173">Le **[ODataRoute]** attribut définit le modèle d’URI pour la fonction.</span><span class="sxs-lookup"><span data-stu-id="6a1f7-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="6a1f7-174">Voici un exemple de demande client :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="6a1f7-175">La réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="6a1f7-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
