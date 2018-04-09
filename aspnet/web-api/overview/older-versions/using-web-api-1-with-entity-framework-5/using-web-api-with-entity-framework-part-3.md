---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Partie 3 : Création d’un contrôleur de Admin | Documents Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="40399-102">Partie 3 : Création d’un contrôleur de l’administrateur</span><span class="sxs-lookup"><span data-stu-id="40399-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="40399-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="40399-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="40399-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="40399-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="40399-105">Ajouter un administrateur de contrôleur</span><span class="sxs-lookup"><span data-stu-id="40399-105">Add an Admin Controller</span></span>

<span data-ttu-id="40399-106">Dans cette section, nous allons ajouter un contrôleur d’API Web qui prend en charge CRUD (créer, lire, mettre à jour et supprimer) des opérations sur les produits.</span><span class="sxs-lookup"><span data-stu-id="40399-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="40399-107">Le contrôleur doit utiliser Entity Framework pour communiquer avec la couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="40399-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="40399-108">Seuls les administrateurs seront en mesure d’utiliser ce contrôleur.</span><span class="sxs-lookup"><span data-stu-id="40399-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="40399-109">Les clients auront accès les produits par un autre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="40399-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="40399-110">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="40399-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="40399-111">Sélectionnez **ajouter** , puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="40399-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="40399-112">Dans le **ajouter un contrôleur** boîte de dialogue, le nom du contrôleur `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="40399-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="40399-113">Sous **modèle**, sélectionnez &quot;contrôleur d’API avec actions en lecture/écriture, à l’aide d’Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="40399-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="40399-114">Sous **classe de modèle**, sélectionnez « Product (ProductStore.Models) ».</span><span class="sxs-lookup"><span data-stu-id="40399-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="40399-115">Sous **contexte de données**, sélectionnez «&lt;nouveau contexte de données&gt;».</span><span class="sxs-lookup"><span data-stu-id="40399-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="40399-116">Si le **classe de modèle** liste déroulante n’affiche pas toutes les classes de modèle, assurez-vous que vous avez compilé le projet.</span><span class="sxs-lookup"><span data-stu-id="40399-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="40399-117">Entity Framework utilise la réflexion, par conséquent, il doit l’assembly compilé.</span><span class="sxs-lookup"><span data-stu-id="40399-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="40399-118">En sélectionnant «&lt;nouveau contexte de données&gt;» ouvre le **nouveau contexte de données** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="40399-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="40399-119">Nommez le contexte de données `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="40399-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="40399-120">Cliquez sur **OK** pour faire disparaître la **nouveau contexte de données** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="40399-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="40399-121">Dans le **ajouter un contrôleur** boîte de dialogue, cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="40399-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="40399-122">Voici ce qui a été ajouté au projet :</span><span class="sxs-lookup"><span data-stu-id="40399-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="40399-123">Une classe nommée `OrdersContext` qui dérive de **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="40399-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="40399-124">Cette classe fournit le collage entre les modèles POCO et la base de données.</span><span class="sxs-lookup"><span data-stu-id="40399-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="40399-125">Contrôleur d’API Web nommé `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="40399-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="40399-126">Ce contrôleur prend en charge les opérations CRUD sur `Product` instances.</span><span class="sxs-lookup"><span data-stu-id="40399-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="40399-127">Elle utilise le `OrdersContext` classe pour communiquer avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="40399-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="40399-128">Une nouvelle chaîne de connexion de base de données dans le fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="40399-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="40399-129">Ouvrez le fichier OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="40399-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="40399-130">Notez que le constructeur spécifie le nom de la chaîne de connexion de base de données.</span><span class="sxs-lookup"><span data-stu-id="40399-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="40399-131">Ce nom fait référence à la chaîne de connexion qui a été ajoutée au fichier Web.config.</span><span class="sxs-lookup"><span data-stu-id="40399-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="40399-132">Ajoutez les propriétés suivantes à la classe `OrdersContext` :</span><span class="sxs-lookup"><span data-stu-id="40399-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="40399-133">A **DbSet** représente un jeu d’entités qui peuvent être interrogées.</span><span class="sxs-lookup"><span data-stu-id="40399-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="40399-134">Voici l’intégralité de la `OrdersContext` classe :</span><span class="sxs-lookup"><span data-stu-id="40399-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="40399-135">La `AdminController` classe définit cinq méthodes qui implémentent les fonctionnalités de CRUD de base.</span><span class="sxs-lookup"><span data-stu-id="40399-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="40399-136">Chaque méthode correspond à un URI que le client peut appeler :</span><span class="sxs-lookup"><span data-stu-id="40399-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="40399-137">Méthode de contrôleur</span><span class="sxs-lookup"><span data-stu-id="40399-137">Controller Method</span></span> | <span data-ttu-id="40399-138">Description</span><span class="sxs-lookup"><span data-stu-id="40399-138">Description</span></span> | <span data-ttu-id="40399-139">URI</span><span class="sxs-lookup"><span data-stu-id="40399-139">URI</span></span> | <span data-ttu-id="40399-140">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="40399-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="40399-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="40399-141">GetProducts</span></span> | <span data-ttu-id="40399-142">Obtient tous les produits.</span><span class="sxs-lookup"><span data-stu-id="40399-142">Gets all products.</span></span> | <span data-ttu-id="40399-143">API/produits</span><span class="sxs-lookup"><span data-stu-id="40399-143">api/products</span></span> | <span data-ttu-id="40399-144">GET</span><span class="sxs-lookup"><span data-stu-id="40399-144">GET</span></span> |
| <span data-ttu-id="40399-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="40399-145">GetProduct</span></span> | <span data-ttu-id="40399-146">Recherche d’un produit par ID.</span><span class="sxs-lookup"><span data-stu-id="40399-146">Finds a product by ID.</span></span> | <span data-ttu-id="40399-147">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="40399-147">api/products/*id*</span></span> | <span data-ttu-id="40399-148">GET</span><span class="sxs-lookup"><span data-stu-id="40399-148">GET</span></span> |
| <span data-ttu-id="40399-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="40399-149">PutProduct</span></span> | <span data-ttu-id="40399-150">Met à jour un produit.</span><span class="sxs-lookup"><span data-stu-id="40399-150">Updates a product.</span></span> | <span data-ttu-id="40399-151">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="40399-151">api/products/*id*</span></span> | <span data-ttu-id="40399-152">PUT</span><span class="sxs-lookup"><span data-stu-id="40399-152">PUT</span></span> |
| <span data-ttu-id="40399-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="40399-153">PostProduct</span></span> | <span data-ttu-id="40399-154">Crée un nouveau produit.</span><span class="sxs-lookup"><span data-stu-id="40399-154">Creates a new product.</span></span> | <span data-ttu-id="40399-155">API/produits</span><span class="sxs-lookup"><span data-stu-id="40399-155">api/products</span></span> | <span data-ttu-id="40399-156">POST</span><span class="sxs-lookup"><span data-stu-id="40399-156">POST</span></span> |
| <span data-ttu-id="40399-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="40399-157">DeleteProduct</span></span> | <span data-ttu-id="40399-158">Supprime un produit.</span><span class="sxs-lookup"><span data-stu-id="40399-158">Deletes a product.</span></span> | <span data-ttu-id="40399-159">api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="40399-159">api/products/*id*</span></span> | <span data-ttu-id="40399-160">SUPPR</span><span class="sxs-lookup"><span data-stu-id="40399-160">DELETE</span></span> |

<span data-ttu-id="40399-161">Chaque méthode appelle `OrdersContext` pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="40399-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="40399-162">Appellent des méthodes qui modifient la collection (PUT, POST et DELETE) `db.SaveChanges` pour rendre persistantes les modifications apportées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="40399-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="40399-163">Contrôleurs sont créés par la requête HTTP et ensuite supprimés, il est donc nécessaire de conserver les modifications avant le retour de méthode.</span><span class="sxs-lookup"><span data-stu-id="40399-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="40399-164">Ajouter un initialiseur de base de données</span><span class="sxs-lookup"><span data-stu-id="40399-164">Add a Database Initializer</span></span>

<span data-ttu-id="40399-165">Entity Framework offre une fonctionnalité intéressante qui vous permet de remplir la base de données au démarrage et recréer automatiquement la base de données chaque fois que les modèles modifiez.</span><span class="sxs-lookup"><span data-stu-id="40399-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="40399-166">Cette fonctionnalité est utile lors du développement, car vous avez toujours des données de test, même si vous modifiez les modèles.</span><span class="sxs-lookup"><span data-stu-id="40399-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="40399-167">Dans l’Explorateur de solutions, cliquez sur le dossier Modèles et créer une nouvelle classe nommée `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="40399-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="40399-168">Collez dans l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="40399-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="40399-169">En héritant de la **DropCreateDatabaseIfModelChanges** (classe), nous indiquons Entity Framework pour supprimer la base de données chaque fois que nous modifions les classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="40399-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="40399-170">Lorsque Entity Framework crée ou recrée la base de données, il appelle la **Seed** méthode pour remplir les tables.</span><span class="sxs-lookup"><span data-stu-id="40399-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="40399-171">Nous utilisons le **Seed** méthode pour ajouter une commande exemple ainsi que certains produits de l’exemple.</span><span class="sxs-lookup"><span data-stu-id="40399-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="40399-172">Cette fonctionnalité est très utile pour les tests, mais n’utilisez pas le **DropCreateDatabaseIfModelChanges** classe en production, car vous risquez de perdre vos données si une personne modifie une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="40399-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="40399-173">Ensuite, ouvrez Global.asax et ajoutez le code suivant à la **Application\_Démarrer** méthode :</span><span class="sxs-lookup"><span data-stu-id="40399-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="40399-174">Envoyer une demande au contrôleur</span><span class="sxs-lookup"><span data-stu-id="40399-174">Send a Request to the Controller</span></span>

<span data-ttu-id="40399-175">À ce stade, nous n’avons pas écrit tout le code client, mais que vous pouvez appeler le web API à l’aide d’un navigateur web ou un débogage HTTP outil tel que [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="40399-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="40399-176">Dans Visual Studio, appuyez sur F5 pour démarrer le débogage.</span><span class="sxs-lookup"><span data-stu-id="40399-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="40399-177">Votre navigateur web s’ouvre sur `http://localhost:*portnum*/`, où *portnum* correspond à un numéro de port.</span><span class="sxs-lookup"><span data-stu-id="40399-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="40399-178">Envoyer une demande HTTP à «`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="40399-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="40399-179">La première requête peut être lente, car Entify Framework a besoin de créer et de la base de données de valeur initiale.</span><span class="sxs-lookup"><span data-stu-id="40399-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="40399-180">La réponse doit code similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="40399-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="40399-181">[Précédent](using-web-api-with-entity-framework-part-2.md)
> [Suivant](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="40399-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
