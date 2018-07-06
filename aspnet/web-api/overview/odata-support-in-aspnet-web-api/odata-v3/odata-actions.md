---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Prise en charge des Actions OData dans ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'Dans OData, les actions sont un moyen pour ajouter des comportements côté serveur qui ne sont pas facilement définies en tant qu’opérations CRUD sur les entités. Certaines utilisations pour les actions incluent : implémentez...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837770"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="8c64e-104">Prise en charge des Actions OData dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8c64e-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8c64e-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8c64e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8c64e-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="8c64e-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="8c64e-107">Dans OData, *actions* sont un moyen pour ajouter des comportements côté serveur qui ne sont pas facilement définies en tant qu’opérations CRUD sur les entités.</span><span class="sxs-lookup"><span data-stu-id="8c64e-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="8c64e-108">Certaines utilisations pour les actions sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="8c64e-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="8c64e-109">Implémentation des transactions complexes.</span><span class="sxs-lookup"><span data-stu-id="8c64e-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="8c64e-110">Manipulation de plusieurs entités à la fois.</span><span class="sxs-lookup"><span data-stu-id="8c64e-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="8c64e-111">Ce qui permet des mises à jour uniquement certaines propriétés d’une entité.</span><span class="sxs-lookup"><span data-stu-id="8c64e-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="8c64e-112">Envoi d’informations sur le serveur qui n’est pas défini dans une entité.</span><span class="sxs-lookup"><span data-stu-id="8c64e-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8c64e-113">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="8c64e-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8c64e-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="8c64e-114">Web API 2</span></span>
> - <span data-ttu-id="8c64e-115">OData Version 3</span><span class="sxs-lookup"><span data-stu-id="8c64e-115">OData Version 3</span></span>
> - <span data-ttu-id="8c64e-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8c64e-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="8c64e-117">Exemple : Évaluation d’un produit</span><span class="sxs-lookup"><span data-stu-id="8c64e-117">Example: Rating a Product</span></span>

<span data-ttu-id="8c64e-118">Dans cet exemple, nous voulons permettre aux utilisateurs de classer les produits, puis l’exposer l’évaluation moyenne pour chaque produit.</span><span class="sxs-lookup"><span data-stu-id="8c64e-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="8c64e-119">Sur la base de données, nous allons stocker une liste de contrôle d’accès, la clé de produits.</span><span class="sxs-lookup"><span data-stu-id="8c64e-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="8c64e-120">Voici le modèle que nous pouvons utiliser pour représenter les évaluations dans Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="8c64e-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="8c64e-121">Mais nous ne voulons pas les clients pour valider un `ProductRating` objet à une collection de « Notes ».</span><span class="sxs-lookup"><span data-stu-id="8c64e-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="8c64e-122">Intuitivement, l’évaluation est associée à la collection de produits, et le client ne doit valider la valeur d’évaluation.</span><span class="sxs-lookup"><span data-stu-id="8c64e-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="8c64e-123">Par conséquent, au lieu d’utiliser les opérations CRUD normales, nous définissons une action qu’un client peut appeler sur un produit.</span><span class="sxs-lookup"><span data-stu-id="8c64e-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="8c64e-124">Dans la terminologie d’OData, l’action est *lié* aux entités du produit.</span><span class="sxs-lookup"><span data-stu-id="8c64e-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="8c64e-125">Actions ont des effets sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="8c64e-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="8c64e-126">Pour cette raison, ils sont appelés à l’aide de requêtes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="8c64e-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="8c64e-127">Actions peuvent avoir des paramètres et types de retour, qui sont décrites dans les métadonnées du service.</span><span class="sxs-lookup"><span data-stu-id="8c64e-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="8c64e-128">Le client envoie les paramètres dans le corps de la demande, et le serveur envoie la valeur de retour dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="8c64e-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="8c64e-129">Pour appeler l’action « Taux produit », le client envoie un POST à un URI comme suit :</span><span class="sxs-lookup"><span data-stu-id="8c64e-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="8c64e-130">Les données dans la requête POST sont simplement l’évaluation de produit :</span><span class="sxs-lookup"><span data-stu-id="8c64e-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="8c64e-131">Déclarez l’Action dans l’Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="8c64e-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="8c64e-132">Dans la configuration de votre API Web, ajoutez l’action à l’entity data model (EDM) :</span><span class="sxs-lookup"><span data-stu-id="8c64e-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="8c64e-133">Ce code définit « RateProduct » en tant qu’action qui peut être effectuée sur les entités Product.</span><span class="sxs-lookup"><span data-stu-id="8c64e-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="8c64e-134">Elle déclare également que l’action prend un **int** paramètre nommé « Rating » et retourne un **int** valeur.</span><span class="sxs-lookup"><span data-stu-id="8c64e-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="8c64e-135">Ajoutez l’Action au contrôleur</span><span class="sxs-lookup"><span data-stu-id="8c64e-135">Add the Action to the Controller</span></span>

<span data-ttu-id="8c64e-136">L’action « RateProduct » est liée aux entités de produit.</span><span class="sxs-lookup"><span data-stu-id="8c64e-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="8c64e-137">Pour implémenter l’action, ajoutez une méthode nommée `RateProduct` au contrôleur de produits :</span><span class="sxs-lookup"><span data-stu-id="8c64e-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="8c64e-138">Notez que le nom de méthode correspond au nom de l’action dans le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="8c64e-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="8c64e-139">La méthode a deux paramètres :</span><span class="sxs-lookup"><span data-stu-id="8c64e-139">The method has two parameters:</span></span>

- <span data-ttu-id="8c64e-140">*clé*: la clé du produit d’un taux.</span><span class="sxs-lookup"><span data-stu-id="8c64e-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="8c64e-141">*paramètres*: un dictionnaire de valeurs de paramètre d’action.</span><span class="sxs-lookup"><span data-stu-id="8c64e-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="8c64e-142">Si vous utilisez les conventions d’itinéraire par défaut, le paramètre de clé doit être nommé « clé ».</span><span class="sxs-lookup"><span data-stu-id="8c64e-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="8c64e-143">Il est également important d’inclure le **[FromOdataUri]** d’attribut, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="8c64e-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="8c64e-144">Cet attribut indique à l’API Web à utiliser les règles de syntaxe OData lorsqu’il analyse la clé à partir de l’URI de demande.</span><span class="sxs-lookup"><span data-stu-id="8c64e-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="8c64e-145">Utilisez le *paramètres* dictionnaire pour obtenir les paramètres d’action :</span><span class="sxs-lookup"><span data-stu-id="8c64e-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="8c64e-146">Si le client envoie les paramètres d’action dans le bon format, la valeur de **ModelState.IsValid** a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="8c64e-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="8c64e-147">Dans ce cas, vous pouvez utiliser la **ODataActionParameters** dictionnaire pour obtenir les valeurs de paramètre.</span><span class="sxs-lookup"><span data-stu-id="8c64e-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="8c64e-148">Dans cet exemple, le `RateProduct` action prend un paramètre unique nommé « Rating ».</span><span class="sxs-lookup"><span data-stu-id="8c64e-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="8c64e-149">Métadonnées d’action</span><span class="sxs-lookup"><span data-stu-id="8c64e-149">Action Metadata</span></span>

<span data-ttu-id="8c64e-150">Pour afficher les métadonnées du service, envoyez une requête GET à /odata/$ metadata.</span><span class="sxs-lookup"><span data-stu-id="8c64e-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="8c64e-151">Voici la partie des métadonnées qui déclare le `RateProduct` action :</span><span class="sxs-lookup"><span data-stu-id="8c64e-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="8c64e-152">Le **FunctionImport** élément déclare l’action.</span><span class="sxs-lookup"><span data-stu-id="8c64e-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="8c64e-153">La plupart des champs est explicite, mais les deux sont à noter :</span><span class="sxs-lookup"><span data-stu-id="8c64e-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="8c64e-154">**IsBindable** signifie que l’action peut être appelée sur l’entité cible, au moins du temps.</span><span class="sxs-lookup"><span data-stu-id="8c64e-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="8c64e-155">**IsAlwaysBindable** signifie que l’action peut toujours être appelée sur l’entité cible.</span><span class="sxs-lookup"><span data-stu-id="8c64e-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="8c64e-156">La différence est que certaines actions sont toujours disponibles pour les clients, mais les autres actions peuvent dépendre de l’état de l’entité.</span><span class="sxs-lookup"><span data-stu-id="8c64e-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="8c64e-157">Par exemple, supposons que vous définissez une action « Acheter ».</span><span class="sxs-lookup"><span data-stu-id="8c64e-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="8c64e-158">Vous pouvez acheter uniquement un élément qui est en stock.</span><span class="sxs-lookup"><span data-stu-id="8c64e-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="8c64e-159">Si l’élément est en rupture de stock, un client ne peut pas appeler cette action.</span><span class="sxs-lookup"><span data-stu-id="8c64e-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="8c64e-160">Lorsque vous définissez le modèle EDM, le **Action** méthode crée une action toujours pouvant être lié à :</span><span class="sxs-lookup"><span data-stu-id="8c64e-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="8c64e-161">Je vous parlerai pas toujours liable actions (également appelé *temporaires* actions) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="8c64e-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="8c64e-162">Appel d’Action</span><span class="sxs-lookup"><span data-stu-id="8c64e-162">Invoking the Action</span></span>

<span data-ttu-id="8c64e-163">Maintenant nous allons voir comment un client appelle cette action.</span><span class="sxs-lookup"><span data-stu-id="8c64e-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="8c64e-164">Supposons que le client veut donner une évaluation égale à 2 pour le produit avec l’ID = 4.</span><span class="sxs-lookup"><span data-stu-id="8c64e-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="8c64e-165">Voici un exemple de message de demande, à l’aide du format JSON pour le corps de la demande :</span><span class="sxs-lookup"><span data-stu-id="8c64e-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="8c64e-166">Voici le message de réponse :</span><span class="sxs-lookup"><span data-stu-id="8c64e-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="8c64e-167">Liaison d’une Action à un jeu d’entités</span><span class="sxs-lookup"><span data-stu-id="8c64e-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="8c64e-168">Dans l’exemple précédent, l’action est liée à une seule entité : le client évalue un produit unique.</span><span class="sxs-lookup"><span data-stu-id="8c64e-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="8c64e-169">Vous pouvez également lier une action à une collection d’entités.</span><span class="sxs-lookup"><span data-stu-id="8c64e-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="8c64e-170">Apportez simplement les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="8c64e-170">Just make the following changes:</span></span>

<span data-ttu-id="8c64e-171">Dans le modèle EDM, ajoutez l’action à l’entité **Collection** propriété.</span><span class="sxs-lookup"><span data-stu-id="8c64e-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="8c64e-172">Dans la méthode de contrôleur, omettez la *clé* paramètre.</span><span class="sxs-lookup"><span data-stu-id="8c64e-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="8c64e-173">Désormais, le client appelle l’action sur le jeu d’entités de produits :</span><span class="sxs-lookup"><span data-stu-id="8c64e-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="8c64e-174">Actions avec des paramètres de collecte</span><span class="sxs-lookup"><span data-stu-id="8c64e-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="8c64e-175">Actions peuvent avoir des paramètres qui acceptent une collection de valeurs.</span><span class="sxs-lookup"><span data-stu-id="8c64e-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="8c64e-176">Dans le modèle EDM, utilisez **CollectionParameter&lt;T&gt;**  pour déclarer le paramètre.</span><span class="sxs-lookup"><span data-stu-id="8c64e-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="8c64e-177">Cela déclare un paramètre nommé « Évaluation » qui accepte une collection de **int** valeurs.</span><span class="sxs-lookup"><span data-stu-id="8c64e-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="8c64e-178">Dans la méthode de contrôleur, vous obtenez toujours la valeur du paramètre à partir de la **ODataActionParameters** objet, mais désormais la valeur est un **ICollection&lt;int&gt;**  valeur :</span><span class="sxs-lookup"><span data-stu-id="8c64e-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="8c64e-179">Actions temporaires</span><span class="sxs-lookup"><span data-stu-id="8c64e-179">Transient Actions</span></span>

<span data-ttu-id="8c64e-180">Dans l’exemple « RateProduct », les utilisateurs peuvent évaluer toujours un produit, par conséquent, l’action est toujours disponible.</span><span class="sxs-lookup"><span data-stu-id="8c64e-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="8c64e-181">Mais certaines actions dépendent de l’état de l’entité.</span><span class="sxs-lookup"><span data-stu-id="8c64e-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="8c64e-182">Par exemple, dans un service de location de vidéo, l’action « Récupération » n’est pas toujours disponible.</span><span class="sxs-lookup"><span data-stu-id="8c64e-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="8c64e-183">(Cela dépend si une copie de cette vidéo est disponible). Ce type d’action est appelé un *temporaires* action.</span><span class="sxs-lookup"><span data-stu-id="8c64e-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="8c64e-184">Dans les métadonnées de service, une action temporaire a **IsAlwaysBindable** égal à false.</span><span class="sxs-lookup"><span data-stu-id="8c64e-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="8c64e-185">C’est effectivement la valeur par défaut, et les métadonnées ressemblera à ceci :</span><span class="sxs-lookup"><span data-stu-id="8c64e-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="8c64e-186">Voici pourquoi c’est important : si une action est temporaire, le serveur doit indiquer au client lorsque l’action est disponible.</span><span class="sxs-lookup"><span data-stu-id="8c64e-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="8c64e-187">Pour ce faire, il comprend un lien vers l’action dans l’entité.</span><span class="sxs-lookup"><span data-stu-id="8c64e-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="8c64e-188">Voici un exemple d’une entité de film :</span><span class="sxs-lookup"><span data-stu-id="8c64e-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="8c64e-189">La propriété « #CheckOut » contient un lien vers l’action d’extraction.</span><span class="sxs-lookup"><span data-stu-id="8c64e-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="8c64e-190">Si l’action n’est pas disponible, le serveur omet le lien.</span><span class="sxs-lookup"><span data-stu-id="8c64e-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="8c64e-191">Pour déclarer une action temporaire dans le modèle EDM, appelez le **TransientAction** méthode :</span><span class="sxs-lookup"><span data-stu-id="8c64e-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="8c64e-192">En outre, vous devez fournir une fonction qui retourne un lien d’action pour une entité donnée.</span><span class="sxs-lookup"><span data-stu-id="8c64e-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="8c64e-193">Définir cette fonction en appelant **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="8c64e-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="8c64e-194">Vous pouvez écrire la fonction comme une expression lambda :</span><span class="sxs-lookup"><span data-stu-id="8c64e-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="8c64e-195">Si l’action est disponible, l’expression lambda renvoie un lien vers l’action.</span><span class="sxs-lookup"><span data-stu-id="8c64e-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="8c64e-196">Le sérialiseur OData inclut ce lien lorsqu’il sérialise l’entité.</span><span class="sxs-lookup"><span data-stu-id="8c64e-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="8c64e-197">Lorsque l’action n’est pas disponible, la fonction retourne `null`.</span><span class="sxs-lookup"><span data-stu-id="8c64e-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c64e-198">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8c64e-198">Additional Resources</span></span>

[<span data-ttu-id="8c64e-199">Exemple d’Actions OData</span><span class="sxs-lookup"><span data-stu-id="8c64e-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
