---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "Partie 6 : Création de produit et les contrôleurs de commande | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 3b33543f02479b97112a63eb3879967ae31ccfb3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="93a40-102">Partie 6 : Création d’un produit et contrôleurs de commande</span><span class="sxs-lookup"><span data-stu-id="93a40-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="93a40-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="93a40-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="93a40-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="93a40-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="93a40-105">Ajouter un contrôleur de produits</span><span class="sxs-lookup"><span data-stu-id="93a40-105">Add a Products Controller</span></span>

<span data-ttu-id="93a40-106">Le contrôleur de l’administrateur est pour les utilisateurs disposant des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="93a40-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="93a40-107">Clients, quant à eux, peuvent afficher les produits, mais ne peut pas créer, mettre à jour ou supprimez-les.</span><span class="sxs-lookup"><span data-stu-id="93a40-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="93a40-108">Nous pouvons facilement restreindre l’accès aux méthodes Post, Put et Delete, en laissant les méthodes Get ouvert.</span><span class="sxs-lookup"><span data-stu-id="93a40-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="93a40-109">Mais les données retournées pour un produit :</span><span class="sxs-lookup"><span data-stu-id="93a40-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="93a40-110">Le `ActualCost` propriété ne doit pas être visible pour les clients !</span><span class="sxs-lookup"><span data-stu-id="93a40-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="93a40-111">La solution consiste à définir un *objet de transfert de données* (DTO) qui inclut un sous-ensemble de propriétés qui doivent être visibles aux clients.</span><span class="sxs-lookup"><span data-stu-id="93a40-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="93a40-112">Nous allons utiliser LINQ au projet `Product` instances à `ProductDTO` instances.</span><span class="sxs-lookup"><span data-stu-id="93a40-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="93a40-113">Ajoutez une classe nommée `ProductDTO` dans le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="93a40-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="93a40-114">Maintenant, ajoutez le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="93a40-114">Now add the controller.</span></span> <span data-ttu-id="93a40-115">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="93a40-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="93a40-116">Sélectionnez **ajouter**, puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="93a40-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="93a40-117">Dans le **ajouter un contrôleur** boîte de dialogue, le nom du contrôleur &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="93a40-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="93a40-118">Sous **modèle**, sélectionnez **contrôleur d’API vide**.</span><span class="sxs-lookup"><span data-stu-id="93a40-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="93a40-119">Remplacer tous les éléments dans le fichier source avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="93a40-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="93a40-120">Le contrôleur utilise toujours le `OrdersContext` pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="93a40-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="93a40-121">Mais au lieu de retourner `Product` instances directement, nous appelons `MapProducts` pour projeter les sur `ProductDTO` instances :</span><span class="sxs-lookup"><span data-stu-id="93a40-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="93a40-122">Le `MapProducts` méthode retourne un **IQueryable**, de sorte que nous pouvons composer le résultat avec les autres paramètres de requête.</span><span class="sxs-lookup"><span data-stu-id="93a40-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="93a40-123">Vous pouvez le constater dans le `GetProduct` méthode, qui ajoute un **où** clause à la requête :</span><span class="sxs-lookup"><span data-stu-id="93a40-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="93a40-124">Ajouter un contrôleur de commandes</span><span class="sxs-lookup"><span data-stu-id="93a40-124">Add an Orders Controller</span></span>

<span data-ttu-id="93a40-125">Ensuite, ajoutez un contrôleur qui permet aux utilisateurs de créer et afficher les commandes.</span><span class="sxs-lookup"><span data-stu-id="93a40-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="93a40-126">Nous allons commencer par une autre DTO.</span><span class="sxs-lookup"><span data-stu-id="93a40-126">We'll start with another DTO.</span></span> <span data-ttu-id="93a40-127">Dans l’Explorateur de solutions, cliquez sur le dossier Modèles et ajoutez une classe nommée `OrderDTO` utiliser l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="93a40-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="93a40-128">Maintenant, ajoutez le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="93a40-128">Now add the controller.</span></span> <span data-ttu-id="93a40-129">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="93a40-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="93a40-130">Sélectionnez **ajouter**, puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="93a40-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="93a40-131">Dans le **ajouter un contrôleur** boîte de dialogue, définissez les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="93a40-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="93a40-132">Sous **nom de contrôleur**, entrez « OrdersController ».</span><span class="sxs-lookup"><span data-stu-id="93a40-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="93a40-133">Sous **modèle**, sélectionnez « Contrôleur d’API avec actions en lecture/écriture, à l’aide d’Entity Framework ».</span><span class="sxs-lookup"><span data-stu-id="93a40-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="93a40-134">Sous **classe de modèle**, sélectionnez &quot;ordre (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="93a40-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="93a40-135">Sous **classe de contexte de données**, sélectionnez &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="93a40-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="93a40-136">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="93a40-136">Click **Add**.</span></span> <span data-ttu-id="93a40-137">Cela ajoute un fichier nommé OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="93a40-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="93a40-138">Ensuite, nous avons besoin de modifier l’implémentation par défaut du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="93a40-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="93a40-139">Supprimez d’abord le `PutOrder` et `DeleteOrder` méthodes.</span><span class="sxs-lookup"><span data-stu-id="93a40-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="93a40-140">Pour cet exemple, les clients ne peuvent pas modifier ou supprimer des commandes existantes.</span><span class="sxs-lookup"><span data-stu-id="93a40-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="93a40-141">Dans une application réelle, vous devez beaucoup de logique principal pour gérer ces cas.</span><span class="sxs-lookup"><span data-stu-id="93a40-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="93a40-142">(Par exemple, la commande déjà expédiée ?)</span><span class="sxs-lookup"><span data-stu-id="93a40-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="93a40-143">Modifier la `GetOrders` méthode pour retourner uniquement les commandes qui appartiennent à l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="93a40-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="93a40-144">Modifier la `GetOrder` méthode comme suit :</span><span class="sxs-lookup"><span data-stu-id="93a40-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="93a40-145">Voici les modifications que nous avons apporté à la méthode :</span><span class="sxs-lookup"><span data-stu-id="93a40-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="93a40-146">La valeur de retour est un `OrderDTO` instance, au lieu d’un `Order`.</span><span class="sxs-lookup"><span data-stu-id="93a40-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="93a40-147">Lorsque nous interroger la base de données pour la commande, nous utilisons le [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) méthode pour extraire le `OrderDetail` et `Product` entités.</span><span class="sxs-lookup"><span data-stu-id="93a40-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="93a40-148">Nous aplatir le résultats à l’aide d’une projection.</span><span class="sxs-lookup"><span data-stu-id="93a40-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="93a40-149">La réponse HTTP contient un tableau de produits et quantités :</span><span class="sxs-lookup"><span data-stu-id="93a40-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="93a40-150">Ce format est plus facile pour les clients à utiliser que le graphique d’objet d’origine, qui contient les entités imbriquées (ordre, les détails et produits).</span><span class="sxs-lookup"><span data-stu-id="93a40-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="93a40-151">La dernière méthode de prendre en compte `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="93a40-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="93a40-152">Actuellement, cette méthode prend un `Order` instance.</span><span class="sxs-lookup"><span data-stu-id="93a40-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="93a40-153">Voyons ce qui se passe si un client envoie un corps de demande comme suit :</span><span class="sxs-lookup"><span data-stu-id="93a40-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="93a40-154">Il s’agit d’un ordre bien structuré et Entity Framework sera heureusement insérer dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="93a40-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="93a40-155">Mais il contient une entité Product qui n’existe pas encore.</span><span class="sxs-lookup"><span data-stu-id="93a40-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="93a40-156">Le client venez de créer un nouveau produit dans notre base de données !</span><span class="sxs-lookup"><span data-stu-id="93a40-156">The client just created a new product in our database!</span></span> <span data-ttu-id="93a40-157">Il s’agit une surprise au service Fulfillment ordre, lorsqu’ils voient un ordre pour koala porte.</span><span class="sxs-lookup"><span data-stu-id="93a40-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="93a40-158">La morale est, vous méfier réellement les données que vous acceptez une demande POST ou PUT.</span><span class="sxs-lookup"><span data-stu-id="93a40-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="93a40-159">Pour éviter ce problème, modifiez le `PostOrder` méthode prendre une `OrderDTO` instance.</span><span class="sxs-lookup"><span data-stu-id="93a40-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="93a40-160">Utilisez le `OrderDTO` pour créer le `Order`.</span><span class="sxs-lookup"><span data-stu-id="93a40-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="93a40-161">Notez que nous utilisons le `ProductID` et `Quantity` propriétés et nous ignorer toutes les valeurs que le client a envoyé pour nom de produit ou de prix.</span><span class="sxs-lookup"><span data-stu-id="93a40-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="93a40-162">Si l’ID de produit n’est pas valide, il enfreint la contrainte de clé étrangère dans la base de données, et l’instruction insert échouera, comme il le devrait.</span><span class="sxs-lookup"><span data-stu-id="93a40-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="93a40-163">Voici l’ensemble `PostOrder` méthode :</span><span class="sxs-lookup"><span data-stu-id="93a40-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="93a40-164">Enfin, ajoutez le **Authorize** d’attribut pour le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="93a40-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="93a40-165">Maintenant, seuls les utilisateurs inscrits peuvent créer ou afficher les commandes.</span><span class="sxs-lookup"><span data-stu-id="93a40-165">Now only registered users can create or view orders.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="93a40-166">[Précédent](using-web-api-with-entity-framework-part-5.md)
[Suivant](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="93a40-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
