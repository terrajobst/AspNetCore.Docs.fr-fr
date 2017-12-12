---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "Activation des opérations CRUD dans ASP.NET Web API 1 | Documents Microsoft"
author: MikeWasson
description: "Ce didacticiel montre comment prendre en charge des opérations dans un service HTTP à l’aide des API Web ASP.NET. Versions des logiciels utilisées dans le didacticiel Visual Studio 2012 Web AP..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="4cf71-104">Activation des opérations CRUD dans ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="4cf71-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="4cf71-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4cf71-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4cf71-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="4cf71-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="4cf71-107">Ce didacticiel montre comment prendre en charge des opérations dans un service HTTP à l’aide des API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4cf71-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4cf71-108">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="4cf71-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4cf71-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4cf71-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="4cf71-110">API Web 1 (fonctionne également avec l’API Web 2)</span><span class="sxs-lookup"><span data-stu-id="4cf71-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="4cf71-111">CRUD est l’acronyme &quot;création, lecture, mise à jour et suppression,&quot; qui sont les quatre opérations de base de données.</span><span class="sxs-lookup"><span data-stu-id="4cf71-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="4cf71-112">De nombreux services HTTP également les opérations CRUD via REST ou des API REST de type de modèle.</span><span class="sxs-lookup"><span data-stu-id="4cf71-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="4cf71-113">Dans ce didacticiel, vous allez générer une API pour gérer une liste de produits de web très simple.</span><span class="sxs-lookup"><span data-stu-id="4cf71-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="4cf71-114">Chaque produit contient un nom, le prix et la catégorie (tel que &quot;toys&quot; ou &quot;matériel&quot;), ainsi que d’un ID de produit.</span><span class="sxs-lookup"><span data-stu-id="4cf71-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="4cf71-115">Exposent les API de produits suivants méthodes.</span><span class="sxs-lookup"><span data-stu-id="4cf71-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="4cf71-116">Action</span><span class="sxs-lookup"><span data-stu-id="4cf71-116">Action</span></span> | <span data-ttu-id="4cf71-117">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="4cf71-117">HTTP method</span></span> | <span data-ttu-id="4cf71-118">URI relatif</span><span class="sxs-lookup"><span data-stu-id="4cf71-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4cf71-119">Obtenir une liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="4cf71-119">Get a list of all products</span></span> | <span data-ttu-id="4cf71-120">GET</span><span class="sxs-lookup"><span data-stu-id="4cf71-120">GET</span></span> | <span data-ttu-id="4cf71-121">produits/api /</span><span class="sxs-lookup"><span data-stu-id="4cf71-121">/api/products</span></span> |
| <span data-ttu-id="4cf71-122">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="4cf71-122">Get a product by ID</span></span> | <span data-ttu-id="4cf71-123">GET</span><span class="sxs-lookup"><span data-stu-id="4cf71-123">GET</span></span> | <span data-ttu-id="4cf71-124">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="4cf71-124">/api/products/*id*</span></span> |
| <span data-ttu-id="4cf71-125">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="4cf71-125">Get a product by category</span></span> | <span data-ttu-id="4cf71-126">GET</span><span class="sxs-lookup"><span data-stu-id="4cf71-126">GET</span></span> | <span data-ttu-id="4cf71-127">produits/api / ? catégorie =*catégorie*</span><span class="sxs-lookup"><span data-stu-id="4cf71-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="4cf71-128">Créer un nouveau produit</span><span class="sxs-lookup"><span data-stu-id="4cf71-128">Create a new product</span></span> | <span data-ttu-id="4cf71-129">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="4cf71-129">POST</span></span> | <span data-ttu-id="4cf71-130">produits/api /</span><span class="sxs-lookup"><span data-stu-id="4cf71-130">/api/products</span></span> |
| <span data-ttu-id="4cf71-131">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="4cf71-131">Update a product</span></span> | <span data-ttu-id="4cf71-132">PUT</span><span class="sxs-lookup"><span data-stu-id="4cf71-132">PUT</span></span> | <span data-ttu-id="4cf71-133">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="4cf71-133">/api/products/*id*</span></span> |
| <span data-ttu-id="4cf71-134">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="4cf71-134">Delete a product</span></span> | <span data-ttu-id="4cf71-135">SUPPR</span><span class="sxs-lookup"><span data-stu-id="4cf71-135">DELETE</span></span> | <span data-ttu-id="4cf71-136">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="4cf71-136">/api/products/*id*</span></span> |

<span data-ttu-id="4cf71-137">Notez que certaines des URI incluent l’ID de produit dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="4cf71-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="4cf71-138">Par exemple, pour obtenir le produit dont l’ID est égal à 28, le client envoie une demande GET pour `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="4cf71-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="4cf71-139">Ressources</span><span class="sxs-lookup"><span data-stu-id="4cf71-139">Resources</span></span>

<span data-ttu-id="4cf71-140">Les API de produits définit l’URI pour les deux types de ressources :</span><span class="sxs-lookup"><span data-stu-id="4cf71-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="4cf71-141">Ressource</span><span class="sxs-lookup"><span data-stu-id="4cf71-141">Resource</span></span> | <span data-ttu-id="4cf71-142">URI</span><span class="sxs-lookup"><span data-stu-id="4cf71-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="4cf71-143">La liste de tous les produits.</span><span class="sxs-lookup"><span data-stu-id="4cf71-143">The list of all the products.</span></span> | <span data-ttu-id="4cf71-144">produits/api /</span><span class="sxs-lookup"><span data-stu-id="4cf71-144">/api/products</span></span> |
| <span data-ttu-id="4cf71-145">Un produit individuel.</span><span class="sxs-lookup"><span data-stu-id="4cf71-145">An individual product.</span></span> | <span data-ttu-id="4cf71-146">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="4cf71-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="4cf71-147">Méthodes</span><span class="sxs-lookup"><span data-stu-id="4cf71-147">Methods</span></span>

<span data-ttu-id="4cf71-148">Les quatre principales méthodes HTTP (GET, PUT, POST et DELETE) peuvent être mappés aux opérations, comme suit :</span><span class="sxs-lookup"><span data-stu-id="4cf71-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="4cf71-149">GET récupère la représentation sous forme de la ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="4cf71-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="4cf71-150">GET ne doit avoir aucun effet sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="4cf71-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="4cf71-151">PUT met à jour une ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="4cf71-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="4cf71-152">PUT peut également servir à créer une ressource à un URI spécifié, si le serveur autorise les clients à spécifier l’URI de nouveau.</span><span class="sxs-lookup"><span data-stu-id="4cf71-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="4cf71-153">Pour ce didacticiel, l’API ne prendra pas en charge la création par PUT.</span><span class="sxs-lookup"><span data-stu-id="4cf71-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="4cf71-154">POST crée une nouvelle ressource.</span><span class="sxs-lookup"><span data-stu-id="4cf71-154">POST creates a new resource.</span></span> <span data-ttu-id="4cf71-155">Le serveur assigne l’URI pour le nouvel objet et retourne cet URI en tant que partie du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="4cf71-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="4cf71-156">DELETE supprime une ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="4cf71-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="4cf71-157">Remarque : La méthode PUT remplace l’entité complète de produits.</span><span class="sxs-lookup"><span data-stu-id="4cf71-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="4cf71-158">Autrement dit, le client est censé envoyer une représentation complète du produit mis à jour.</span><span class="sxs-lookup"><span data-stu-id="4cf71-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="4cf71-159">Si vous souhaitez prendre en charge les mises à jour partielles, la méthode PATCH est préférée.</span><span class="sxs-lookup"><span data-stu-id="4cf71-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="4cf71-160">Ce didacticiel n’implémente pas de correctif.</span><span class="sxs-lookup"><span data-stu-id="4cf71-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="4cf71-161">Créer un nouveau projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="4cf71-161">Create a New Web API Project</span></span>

<span data-ttu-id="4cf71-162">Commencez par exécuter Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="4cf71-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="4cf71-163">Ou, à partir de la **fichier** menu, sélectionnez **nouveau** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="4cf71-164">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="4cf71-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="4cf71-165">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="4cf71-166">Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="4cf71-167">Nommez le projet &quot;ProductStore&quot; et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="4cf71-168">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **API Web** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="4cf71-169">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="4cf71-169">Adding a Model</span></span>

<span data-ttu-id="4cf71-170">Un *modèle* est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="4cf71-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="4cf71-171">Dans ASP.NET Web API, vous pouvez utiliser des objets CLR fortement typés en tant que modèles, et ils sont automatiquement sérialisés au format XML ou JSON pour le client.</span><span class="sxs-lookup"><span data-stu-id="4cf71-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="4cf71-172">L’API ProductStore, nos données se composent de produits, donc nous allons créer une nouvelle classe nommée `Product`.</span><span class="sxs-lookup"><span data-stu-id="4cf71-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="4cf71-173">Si l’Explorateur de solutions n’est pas déjà visible, cliquez sur le **vue** menu et sélectionnez **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="4cf71-174">Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="4cf71-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="4cf71-175">Dans le contexte meny, sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="4cf71-176">Nom de la classe &quot;produit&quot;.</span><span class="sxs-lookup"><span data-stu-id="4cf71-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="4cf71-177">Ajoutez les propriétés suivantes à la `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="4cf71-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="4cf71-178">Ajout d’un référentiel</span><span class="sxs-lookup"><span data-stu-id="4cf71-178">Adding a Repository</span></span>

<span data-ttu-id="4cf71-179">Nous devons stocker une collection de produits.</span><span class="sxs-lookup"><span data-stu-id="4cf71-179">We need to store a collection of products.</span></span> <span data-ttu-id="4cf71-180">Il est judicieux de séparer la collection à partir de notre implémentation de service.</span><span class="sxs-lookup"><span data-stu-id="4cf71-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="4cf71-181">De cette façon, nous pouvons modifier le magasin de stockage sans réécrire la classe de service.</span><span class="sxs-lookup"><span data-stu-id="4cf71-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="4cf71-182">Ce type de modèle est appelé le *référentiel* modèle.</span><span class="sxs-lookup"><span data-stu-id="4cf71-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="4cf71-183">Commencez par définir une interface générique pour le référentiel.</span><span class="sxs-lookup"><span data-stu-id="4cf71-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="4cf71-184">Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="4cf71-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="4cf71-185">Sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="4cf71-186">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le nœud C#.</span><span class="sxs-lookup"><span data-stu-id="4cf71-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="4cf71-187">Sous c#, sélectionnez **Code**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-187">Under C#, select **Code**.</span></span> <span data-ttu-id="4cf71-188">Dans la liste des modèles de code, sélectionnez **Interface**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="4cf71-189">Nom de l’interface &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="4cf71-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="4cf71-190">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="4cf71-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="4cf71-191">Maintenant ajouter une autre classe dans le dossier de modèles, nommé &quot;ProductRepository.&quot; Cette classe va implémenter l’interface `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="4cf71-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="4cf71-192">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="4cf71-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="4cf71-193">Le référentiel conserve la liste dans la mémoire locale.</span><span class="sxs-lookup"><span data-stu-id="4cf71-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="4cf71-194">Il s’agit d’un didacticiel OK, mais dans une application réelle, vous devez stocker les données en externe, une base de données ou dans le stockage cloud.</span><span class="sxs-lookup"><span data-stu-id="4cf71-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="4cf71-195">Le modèle de référentiel facilitera également le modifier ultérieurement l’implémentation.</span><span class="sxs-lookup"><span data-stu-id="4cf71-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="4cf71-196">Ajout d’un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="4cf71-196">Adding a Web API Controller</span></span>

<span data-ttu-id="4cf71-197">Si vous avez travaillé avec ASP.NET MVC, puis vous êtes déjà familiarisé avec les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="4cf71-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="4cf71-198">Dans ASP.NET Web API, un *contrôleur* est une classe qui gère les demandes HTTP à partir du client.</span><span class="sxs-lookup"><span data-stu-id="4cf71-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="4cf71-199">L’Assistant Nouveau projet créé deux contrôleurs pour vous lors de sa création du projet.</span><span class="sxs-lookup"><span data-stu-id="4cf71-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="4cf71-200">Pour les visualiser, développez le dossier contrôleurs dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="4cf71-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="4cf71-201">HomeController est un contrôleur ASP.NET MVC traditionnel.</span><span class="sxs-lookup"><span data-stu-id="4cf71-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="4cf71-202">Il est responsable du traitement des pages HTML pour le site et n’est pas directement liée à notre API web.</span><span class="sxs-lookup"><span data-stu-id="4cf71-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="4cf71-203">ValuesController est un exemple WebAPI controller.</span><span class="sxs-lookup"><span data-stu-id="4cf71-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="4cf71-204">Pour commencer, supprimez ValuesController, en double-cliquant sur le fichier dans l’Explorateur de solutions et en sélectionnant **supprimer.**</span><span class="sxs-lookup"><span data-stu-id="4cf71-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="4cf71-205">Maintenant, ajoutez un nouveau contrôleur, comme suit :</span><span class="sxs-lookup"><span data-stu-id="4cf71-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="4cf71-206">Dans **l’Explorateur de solutions**, avec le bouton droit le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="4cf71-206">In **Solution Explorer**, right-click the the Controllers folder.</span></span> <span data-ttu-id="4cf71-207">Sélectionnez **ajouter** , puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="4cf71-208">Dans le **ajouter un contrôleur** Assistant, le nom du contrôleur &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="4cf71-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="4cf71-209">Dans le **modèle** la liste déroulante, sélectionnez **contrôleur d’API vide**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="4cf71-210">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="4cf71-211">Il n’est pas nécessaire de placer vos contrôleurs dans un dossier nommé contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="4cf71-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="4cf71-212">Le nom du dossier n’est pas important ; Il est simplement un moyen pratique d’organiser vos fichiers source.</span><span class="sxs-lookup"><span data-stu-id="4cf71-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="4cf71-213">Le **ajouter un contrôleur** Assistant va créer un fichier nommé ProductsController.cs dans le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="4cf71-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="4cf71-214">Si ce fichier n’est pas déjà ouvert, double-cliquez sur le fichier pour l’ouvrir.</span><span class="sxs-lookup"><span data-stu-id="4cf71-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="4cf71-215">Ajoutez le code suivant **à l’aide de** instruction :</span><span class="sxs-lookup"><span data-stu-id="4cf71-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="4cf71-216">Ajouter un champ qui contient un **IProductRepository** instance.</span><span class="sxs-lookup"><span data-stu-id="4cf71-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="4cf71-217">Appel de `new ProductRepository()` dans le contrôleur n’est pas la meilleure conception, parce qu’elle lie le contrôleur à une implémentation particulière de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="4cf71-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="4cf71-218">Pour une meilleure approche, consultez [à l’aide du résolveur de dépendance d’API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="4cf71-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="4cf71-219">Obtention d’une ressource</span><span class="sxs-lookup"><span data-stu-id="4cf71-219">Getting a Resource</span></span>

<span data-ttu-id="4cf71-220">L’API ProductStore expose plusieurs &quot;lire&quot; actions en tant que méthodes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="4cf71-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="4cf71-221">Chaque action correspond à une méthode dans la `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="4cf71-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="4cf71-222">Action</span><span class="sxs-lookup"><span data-stu-id="4cf71-222">Action</span></span> | <span data-ttu-id="4cf71-223">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="4cf71-223">HTTP method</span></span> | <span data-ttu-id="4cf71-224">URI relatif</span><span class="sxs-lookup"><span data-stu-id="4cf71-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4cf71-225">Obtenir une liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="4cf71-225">Get a list of all products</span></span> | <span data-ttu-id="4cf71-226">GET</span><span class="sxs-lookup"><span data-stu-id="4cf71-226">GET</span></span> | <span data-ttu-id="4cf71-227">produits/api /</span><span class="sxs-lookup"><span data-stu-id="4cf71-227">/api/products</span></span> |
| <span data-ttu-id="4cf71-228">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="4cf71-228">Get a product by ID</span></span> | <span data-ttu-id="4cf71-229">GET</span><span class="sxs-lookup"><span data-stu-id="4cf71-229">GET</span></span> | <span data-ttu-id="4cf71-230">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="4cf71-230">/api/products/*id*</span></span> |
| <span data-ttu-id="4cf71-231">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="4cf71-231">Get a product by category</span></span> | <span data-ttu-id="4cf71-232">GET</span><span class="sxs-lookup"><span data-stu-id="4cf71-232">GET</span></span> | <span data-ttu-id="4cf71-233">produits/api / ? catégorie =*catégorie*</span><span class="sxs-lookup"><span data-stu-id="4cf71-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="4cf71-234">Pour obtenir la liste de tous les produits, ajoutez cette méthode à la `ProductsController` classe :</span><span class="sxs-lookup"><span data-stu-id="4cf71-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="4cf71-235">Le nom de la méthode commence par &quot;obtenir&quot;, par convention, il mappe aux demandes GET.</span><span class="sxs-lookup"><span data-stu-id="4cf71-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="4cf71-236">En outre, étant donné que la méthode n’a aucun paramètre, il est mappé à un URI qui ne contient-elle pas un  *&quot;id&quot;*  segment dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="4cf71-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="4cf71-237">Pour obtenir un produit par ID, ajoutez cette méthode à la `ProductsController` classe :</span><span class="sxs-lookup"><span data-stu-id="4cf71-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="4cf71-238">Nom de la méthode également commence par &quot;obtenir&quot;, mais la méthode possède un paramètre nommé *id*. Ce paramètre est associé à la &quot;id&quot; segment du chemin d’accès de l’URI.</span><span class="sxs-lookup"><span data-stu-id="4cf71-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="4cf71-239">L’infrastructure ASP.NET Web API convertit automatiquement l’ID pour le type de données correct (**int**) pour le paramètre.</span><span class="sxs-lookup"><span data-stu-id="4cf71-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="4cf71-240">La méthode GetProduct lève une exception de type **HttpResponseException** si *id* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="4cf71-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="4cf71-241">Cette exception est convertie par l’infrastructure en une erreur 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="4cf71-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="4cf71-242">Enfin, ajoutez une méthode pour rechercher des produits par catégorie :</span><span class="sxs-lookup"><span data-stu-id="4cf71-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="4cf71-243">Si l’URI de requête a une chaîne de requête, API Web essaie de faire correspondre les paramètres de requête aux paramètres de la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4cf71-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="4cf71-244">Par conséquent, un URI sous la forme « api/produits ? catégorie =*catégorie*» sont mappés à cette méthode.</span><span class="sxs-lookup"><span data-stu-id="4cf71-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="4cf71-245">Création d’une ressource</span><span class="sxs-lookup"><span data-stu-id="4cf71-245">Creating a Resource</span></span>

<span data-ttu-id="4cf71-246">Ensuite, nous allons ajouter une méthode à la `ProductsController` classe pour créer un nouveau produit.</span><span class="sxs-lookup"><span data-stu-id="4cf71-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="4cf71-247">Voici une implémentation simple de la méthode :</span><span class="sxs-lookup"><span data-stu-id="4cf71-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="4cf71-248">Notez les deux choses à propos de cette méthode :</span><span class="sxs-lookup"><span data-stu-id="4cf71-248">Note two things about this method:</span></span>

- <span data-ttu-id="4cf71-249">Le nom de la méthode commence par &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="4cf71-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="4cf71-250">Pour créer un nouveau produit, le client envoie une demande HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="4cf71-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="4cf71-251">La méthode prend un paramètre de type produit.</span><span class="sxs-lookup"><span data-stu-id="4cf71-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="4cf71-252">Dans l’API Web, les paramètres avec des types complexes sont désérialisées à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="4cf71-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="4cf71-253">Par conséquent, nous pensons que le client envoie une représentation sérialisée d’un objet de produit, au format XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="4cf71-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="4cf71-254">Cette implémentation fonctionnera, mais il n’est pas tout à fait complet.</span><span class="sxs-lookup"><span data-stu-id="4cf71-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="4cf71-255">Nous aimerions dans l’idéal, la réponse HTTP pour inclure les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4cf71-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="4cf71-256">**Code de réponse :** par défaut, l’infrastructure API Web définit le code d’état de réponse 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="4cf71-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="4cf71-257">Mais, selon le protocole HTTP/1.1, lorsqu’une demande POST entraîne la création d’une ressource, le serveur doit répondre avec état 201 (créé).</span><span class="sxs-lookup"><span data-stu-id="4cf71-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="4cf71-258">**Emplacement :** lorsque le serveur crée une ressource, il doit inclure l’URI de la nouvelle ressource dans l’en-tête d’emplacement de la réponse.</span><span class="sxs-lookup"><span data-stu-id="4cf71-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="4cf71-259">API Web ASP.NET permet de manipuler le message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="4cf71-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="4cf71-260">Voici l’implémentation améliorée :</span><span class="sxs-lookup"><span data-stu-id="4cf71-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="4cf71-261">Notez que le type de retour de méthode est désormais **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="4cf71-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="4cf71-262">En retournant un **HttpResponseMessage** au lieu d’un produit, nous pouvons contrôler les détails du message de réponse HTTP, y compris le code d’état et l’en-tête d’emplacement.</span><span class="sxs-lookup"><span data-stu-id="4cf71-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="4cf71-263">Le **CreateResponse** méthode crée un **HttpResponseMessage** et automatiquement écrit une représentation sérialisée de l’objet de produit dans le corps fo le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="4cf71-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="4cf71-264">Cet exemple ne valide pas le `Product`.</span><span class="sxs-lookup"><span data-stu-id="4cf71-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="4cf71-265">Pour plus d’informations sur la validation de modèle, consultez [Validation de modèle dans ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4cf71-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="4cf71-266">Mise à jour d’une ressource</span><span class="sxs-lookup"><span data-stu-id="4cf71-266">Updating a Resource</span></span>

<span data-ttu-id="4cf71-267">Mise à jour d’un produit avec une méthode PUT est simple :</span><span class="sxs-lookup"><span data-stu-id="4cf71-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="4cf71-268">Le nom de la méthode commence par &quot;placer... &quot;, de sorte que l’API Web met en correspondance pour les demandes PUT.</span><span class="sxs-lookup"><span data-stu-id="4cf71-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="4cf71-269">La méthode prend deux paramètres, l’ID de produit et les produits mis à jour.</span><span class="sxs-lookup"><span data-stu-id="4cf71-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="4cf71-270">Le *id* paramètre est issu le chemin d’accès de l’URI et la *produit* paramètre est désérialisé à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="4cf71-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="4cf71-271">Par défaut, l’infrastructure ASP.NET Web API accepte les types de paramètres simples à partir de l’itinéraire et les types complexes à partir du corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="4cf71-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="4cf71-272">Suppression d’une ressource</span><span class="sxs-lookup"><span data-stu-id="4cf71-272">Deleting a Resource</span></span>

<span data-ttu-id="4cf71-273">Pour supprimer une ressources, définir une méthode de « Supprimer... ».</span><span class="sxs-lookup"><span data-stu-id="4cf71-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="4cf71-274">Si une demande de suppression réussit, elle peut retourner l’état 200 (OK) avec un corps d’entité qui décrit l’état ; état 202 (accepté) si la suppression est toujours en attente ; ou état 204 (aucun contenu) sans corps d’entité.</span><span class="sxs-lookup"><span data-stu-id="4cf71-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="4cf71-275">Dans ce cas, le `DeleteProduct` méthode a un `void` type de retour, le code afin de l’API Web ASP.NET cela traduit automatiquement en état 204 (aucun contenu).</span><span class="sxs-lookup"><span data-stu-id="4cf71-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
