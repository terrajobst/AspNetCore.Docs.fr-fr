---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Activation des opérations CRUD dans ASP.NET Web API 1 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel montre comment prendre en charge des opérations CRUD dans un service HTTP à l’aide d’API Web ASP.NET. Versions des logiciels utilisées dans le didacticiel Visual Studio 2012 Web point d’accès...
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: ba061b26b8527e447f25f6046057542a54f989a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823635"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="22716-104">Activation des opérations CRUD dans ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="22716-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="22716-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="22716-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="22716-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="22716-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="22716-107">Ce didacticiel montre comment prendre en charge des opérations CRUD dans un service HTTP à l’aide d’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="22716-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="22716-108">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="22716-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="22716-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="22716-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="22716-110">Web API 1 (fonctionne également avec l’API Web 2)</span><span class="sxs-lookup"><span data-stu-id="22716-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="22716-111">CRUD est l’acronyme &quot;Create, Read, Update et Delete,&quot; qui sont les quatre opérations de base de données.</span><span class="sxs-lookup"><span data-stu-id="22716-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="22716-112">De nombreux services HTTP également les opérations CRUD via REST ou API REST de type de modèle.</span><span class="sxs-lookup"><span data-stu-id="22716-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="22716-113">Dans ce didacticiel, vous allez générer une API pour gérer une liste de produits de web très simple.</span><span class="sxs-lookup"><span data-stu-id="22716-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="22716-114">Chaque produit contient un nom, le prix et la catégorie (tel que &quot;toys&quot; ou &quot;matériel&quot;), ainsi que d’un ID de produit.</span><span class="sxs-lookup"><span data-stu-id="22716-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="22716-115">Les produits API exposera suivant des méthodes.</span><span class="sxs-lookup"><span data-stu-id="22716-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="22716-116">Action</span><span class="sxs-lookup"><span data-stu-id="22716-116">Action</span></span> | <span data-ttu-id="22716-117">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="22716-117">HTTP method</span></span> | <span data-ttu-id="22716-118">URI relatif</span><span class="sxs-lookup"><span data-stu-id="22716-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="22716-119">Obtenir une liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="22716-119">Get a list of all products</span></span> | <span data-ttu-id="22716-120">GET</span><span class="sxs-lookup"><span data-stu-id="22716-120">GET</span></span> | <span data-ttu-id="22716-121">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="22716-121">/api/products</span></span> |
| <span data-ttu-id="22716-122">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="22716-122">Get a product by ID</span></span> | <span data-ttu-id="22716-123">GET</span><span class="sxs-lookup"><span data-stu-id="22716-123">GET</span></span> | <span data-ttu-id="22716-124">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="22716-124">/api/products/*id*</span></span> |
| <span data-ttu-id="22716-125">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="22716-125">Get a product by category</span></span> | <span data-ttu-id="22716-126">GET</span><span class="sxs-lookup"><span data-stu-id="22716-126">GET</span></span> | <span data-ttu-id="22716-127">produits/api / ? catégorie =*catégorie*</span><span class="sxs-lookup"><span data-stu-id="22716-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="22716-128">Création d’un produit</span><span class="sxs-lookup"><span data-stu-id="22716-128">Create a new product</span></span> | <span data-ttu-id="22716-129">PUBLIER</span><span class="sxs-lookup"><span data-stu-id="22716-129">POST</span></span> | <span data-ttu-id="22716-130">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="22716-130">/api/products</span></span> |
| <span data-ttu-id="22716-131">Mettre à jour un produit</span><span class="sxs-lookup"><span data-stu-id="22716-131">Update a product</span></span> | <span data-ttu-id="22716-132">PUT</span><span class="sxs-lookup"><span data-stu-id="22716-132">PUT</span></span> | <span data-ttu-id="22716-133">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="22716-133">/api/products/*id*</span></span> |
| <span data-ttu-id="22716-134">Supprimer un produit</span><span class="sxs-lookup"><span data-stu-id="22716-134">Delete a product</span></span> | <span data-ttu-id="22716-135">SUPPR</span><span class="sxs-lookup"><span data-stu-id="22716-135">DELETE</span></span> | <span data-ttu-id="22716-136">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="22716-136">/api/products/*id*</span></span> |

<span data-ttu-id="22716-137">Notez que certains des URI comprennent l’ID de produit dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="22716-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="22716-138">Par exemple, pour obtenir le produit dont l’ID est 28, le client envoie une demande GET pour `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="22716-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="22716-139">Ressources</span><span class="sxs-lookup"><span data-stu-id="22716-139">Resources</span></span>

<span data-ttu-id="22716-140">Les produits API définit l’URI pour deux types de ressources :</span><span class="sxs-lookup"><span data-stu-id="22716-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="22716-141">Ressource</span><span class="sxs-lookup"><span data-stu-id="22716-141">Resource</span></span> | <span data-ttu-id="22716-142">URI</span><span class="sxs-lookup"><span data-stu-id="22716-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="22716-143">La liste de tous les produits.</span><span class="sxs-lookup"><span data-stu-id="22716-143">The list of all the products.</span></span> | <span data-ttu-id="22716-144">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="22716-144">/api/products</span></span> |
| <span data-ttu-id="22716-145">Un produit individuel.</span><span class="sxs-lookup"><span data-stu-id="22716-145">An individual product.</span></span> | <span data-ttu-id="22716-146">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="22716-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="22716-147">Méthodes</span><span class="sxs-lookup"><span data-stu-id="22716-147">Methods</span></span>

<span data-ttu-id="22716-148">Les quatre principales méthodes HTTP (GET, PUT, POST et DELETE) peuvent être mappés à des opérations CRUD comme suit :</span><span class="sxs-lookup"><span data-stu-id="22716-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="22716-149">GET récupère la représentation sous forme de la ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="22716-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="22716-150">GET ne doit avoir aucun effet secondaire sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="22716-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="22716-151">PUT met à jour une ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="22716-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="22716-152">PUT peut également servir à créer une nouvelle ressource à un URI spécifié, si le serveur autorise les clients à spécifier de nouveaux URI.</span><span class="sxs-lookup"><span data-stu-id="22716-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="22716-153">Pour ce didacticiel, l’API ne prendra pas en charge la création via PUT.</span><span class="sxs-lookup"><span data-stu-id="22716-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="22716-154">POST crée une nouvelle ressource.</span><span class="sxs-lookup"><span data-stu-id="22716-154">POST creates a new resource.</span></span> <span data-ttu-id="22716-155">Le serveur assigne l’URI pour le nouvel objet et retourne cet URI en tant que partie du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="22716-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="22716-156">DELETE supprime une ressource à un URI spécifié.</span><span class="sxs-lookup"><span data-stu-id="22716-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="22716-157">Remarque : La méthode PUT remplace l’entité de l’ensemble du produit.</span><span class="sxs-lookup"><span data-stu-id="22716-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="22716-158">Autrement dit, le client est censé envoyer une représentation complète du produit mis à jour.</span><span class="sxs-lookup"><span data-stu-id="22716-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="22716-159">Si vous souhaitez prendre en charge les mises à jour partielles, la méthode PATCH est préférée.</span><span class="sxs-lookup"><span data-stu-id="22716-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="22716-160">Ce didacticiel n’implémente pas de correctif.</span><span class="sxs-lookup"><span data-stu-id="22716-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="22716-161">Créer un nouveau projet d’API Web</span><span class="sxs-lookup"><span data-stu-id="22716-161">Create a New Web API Project</span></span>

<span data-ttu-id="22716-162">Commencez par exécuter Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page.</span><span class="sxs-lookup"><span data-stu-id="22716-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="22716-163">Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="22716-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="22716-164">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud.</span><span class="sxs-lookup"><span data-stu-id="22716-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="22716-165">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="22716-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="22716-166">Dans la liste des modèles de projet, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="22716-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="22716-167">Nommez le projet &quot;ProductStore&quot; et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="22716-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="22716-168">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **API Web** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="22716-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="22716-169">Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="22716-169">Adding a Model</span></span>

<span data-ttu-id="22716-170">Un *modèle* est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="22716-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="22716-171">Dans l’API Web ASP.NET, vous pouvez utiliser des objets CLR fortement typés en tant que modèles, et ils sont automatiquement sérialisés au format XML ou JSON pour le client.</span><span class="sxs-lookup"><span data-stu-id="22716-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="22716-172">L’API ProductStore, nos données se composent de produits, nous allons créer une nouvelle classe nommée `Product`.</span><span class="sxs-lookup"><span data-stu-id="22716-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="22716-173">Si l’Explorateur de solutions n’est pas déjà visible, cliquez sur le **vue** menu et sélectionnez **l’Explorateur de solutions**.</span><span class="sxs-lookup"><span data-stu-id="22716-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="22716-174">Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="22716-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="22716-175">Dans le menu contextuel, sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="22716-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="22716-176">Nommez la classe &quot;produit&quot;.</span><span class="sxs-lookup"><span data-stu-id="22716-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="22716-177">Ajoutez les propriétés suivantes à la `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="22716-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="22716-178">Ajout d’un référentiel</span><span class="sxs-lookup"><span data-stu-id="22716-178">Adding a Repository</span></span>

<span data-ttu-id="22716-179">Nous avons besoin de stocker une collection de produits.</span><span class="sxs-lookup"><span data-stu-id="22716-179">We need to store a collection of products.</span></span> <span data-ttu-id="22716-180">Il est judicieux de séparer la collecte à partir de notre implémentation de service.</span><span class="sxs-lookup"><span data-stu-id="22716-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="22716-181">De cette façon, nous pouvons modifier le magasin de stockage sans réécrire la classe de service.</span><span class="sxs-lookup"><span data-stu-id="22716-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="22716-182">Ce type de conception est appelé le *référentiel* modèle.</span><span class="sxs-lookup"><span data-stu-id="22716-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="22716-183">Commencez par définir une interface générique pour le référentiel.</span><span class="sxs-lookup"><span data-stu-id="22716-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="22716-184">Dans l’Explorateur de solutions, cliquez sur le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="22716-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="22716-185">Sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="22716-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="22716-186">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le nœud c#.</span><span class="sxs-lookup"><span data-stu-id="22716-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="22716-187">Dans c#, sélectionnez **Code**.</span><span class="sxs-lookup"><span data-stu-id="22716-187">Under C#, select **Code**.</span></span> <span data-ttu-id="22716-188">Dans la liste des modèles de code, sélectionnez **Interface**.</span><span class="sxs-lookup"><span data-stu-id="22716-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="22716-189">Nom de l’interface &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="22716-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="22716-190">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="22716-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="22716-191">Ajoutez maintenant une autre classe dans le dossier Modèles, nommé &quot;ProductRepository.&quot; Cette classe va implémenter l’interface `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="22716-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="22716-192">Ajoutez l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="22716-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="22716-193">Le référentiel conserve la liste dans la mémoire locale.</span><span class="sxs-lookup"><span data-stu-id="22716-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="22716-194">Il s’agit OK pour obtenir un didacticiel, mais dans une application réelle, vous stockez les données de façon externe, une base de données ou dans le stockage cloud.</span><span class="sxs-lookup"><span data-stu-id="22716-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="22716-195">Le modèle de référentiel facilitera modifier l’implémentation par la suite.</span><span class="sxs-lookup"><span data-stu-id="22716-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="22716-196">Ajout d’un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="22716-196">Adding a Web API Controller</span></span>

<span data-ttu-id="22716-197">Si vous avez travaillé avec ASP.NET MVC, puis vous êtes déjà familiarisé avec les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="22716-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="22716-198">Dans l’API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes HTTP à partir du client.</span><span class="sxs-lookup"><span data-stu-id="22716-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="22716-199">L’Assistant Nouveau projet créé deux contrôleurs pour vous lors de la création du projet.</span><span class="sxs-lookup"><span data-stu-id="22716-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="22716-200">Pour les consulter, développez le dossier contrôleurs dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="22716-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="22716-201">HomeController est un contrôleur ASP.NET MVC traditionnel.</span><span class="sxs-lookup"><span data-stu-id="22716-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="22716-202">Il est responsable du traitement des pages HTML pour le site et n’est pas directement liée à notre API web.</span><span class="sxs-lookup"><span data-stu-id="22716-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="22716-203">ValuesController est un exemple de contrôleur WebAPI.</span><span class="sxs-lookup"><span data-stu-id="22716-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="22716-204">Continuez et supprimer ValuesController, en double-cliquant sur le fichier dans l’Explorateur de solutions et en sélectionnant **supprimer.**</span><span class="sxs-lookup"><span data-stu-id="22716-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="22716-205">Maintenant, ajoutez un nouveau contrôleur, comme suit :</span><span class="sxs-lookup"><span data-stu-id="22716-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="22716-206">Dans **l’Explorateur de solutions**, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="22716-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="22716-207">Sélectionnez **ajouter** , puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="22716-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="22716-208">Dans le **ajouter un contrôleur** Assistant, nommez le contrôleur &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="22716-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="22716-209">Dans le **modèle** liste déroulante, sélectionnez **contrôleur d’API vide**.</span><span class="sxs-lookup"><span data-stu-id="22716-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="22716-210">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="22716-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="22716-211">Il n’est pas nécessaire de le placer vos contrôleurs dans un dossier nommé contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="22716-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="22716-212">Le nom du dossier n’est pas important ; Il est simplement un moyen pratique d’organiser vos fichiers sources.</span><span class="sxs-lookup"><span data-stu-id="22716-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="22716-213">Le **ajouter un contrôleur** Assistant va créer un fichier nommé ProductsController.cs dans le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="22716-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="22716-214">Si ce fichier n’est pas déjà ouvert, double-cliquez sur le fichier pour l’ouvrir.</span><span class="sxs-lookup"><span data-stu-id="22716-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="22716-215">Ajoutez le code suivant **à l’aide de** instruction :</span><span class="sxs-lookup"><span data-stu-id="22716-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="22716-216">Ajouter un champ qui contient un **IProductRepository** instance.</span><span class="sxs-lookup"><span data-stu-id="22716-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="22716-217">Appel `new ProductRepository()` dans le contrôleur n’est pas la meilleure conception, parce qu’elle lie le contrôleur à une implémentation particulière de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="22716-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="22716-218">Pour une meilleure approche, consultez [à l’aide du résolveur de dépendance d’API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="22716-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="22716-219">Obtention d’une ressource</span><span class="sxs-lookup"><span data-stu-id="22716-219">Getting a Resource</span></span>

<span data-ttu-id="22716-220">L’API ProductStore expose plusieurs &quot;lire&quot; actions en tant que méthodes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="22716-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="22716-221">Chaque action correspond à une méthode dans la `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="22716-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="22716-222">Action</span><span class="sxs-lookup"><span data-stu-id="22716-222">Action</span></span> | <span data-ttu-id="22716-223">Méthode HTTP</span><span class="sxs-lookup"><span data-stu-id="22716-223">HTTP method</span></span> | <span data-ttu-id="22716-224">URI relatif</span><span class="sxs-lookup"><span data-stu-id="22716-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="22716-225">Obtenir une liste de tous les produits</span><span class="sxs-lookup"><span data-stu-id="22716-225">Get a list of all products</span></span> | <span data-ttu-id="22716-226">GET</span><span class="sxs-lookup"><span data-stu-id="22716-226">GET</span></span> | <span data-ttu-id="22716-227">/ api/produits</span><span class="sxs-lookup"><span data-stu-id="22716-227">/api/products</span></span> |
| <span data-ttu-id="22716-228">Obtenir un produit par ID</span><span class="sxs-lookup"><span data-stu-id="22716-228">Get a product by ID</span></span> | <span data-ttu-id="22716-229">GET</span><span class="sxs-lookup"><span data-stu-id="22716-229">GET</span></span> | <span data-ttu-id="22716-230">/API/produits/*id*</span><span class="sxs-lookup"><span data-stu-id="22716-230">/api/products/*id*</span></span> |
| <span data-ttu-id="22716-231">Obtenir un produit par catégorie</span><span class="sxs-lookup"><span data-stu-id="22716-231">Get a product by category</span></span> | <span data-ttu-id="22716-232">GET</span><span class="sxs-lookup"><span data-stu-id="22716-232">GET</span></span> | <span data-ttu-id="22716-233">produits/api / ? catégorie =*catégorie*</span><span class="sxs-lookup"><span data-stu-id="22716-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="22716-234">Pour obtenir la liste de tous les produits, ajoutez cette méthode à la `ProductsController` classe :</span><span class="sxs-lookup"><span data-stu-id="22716-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="22716-235">Le nom de la méthode commence par &quot;obtenir&quot;, par convention, il mappe aux demandes GET.</span><span class="sxs-lookup"><span data-stu-id="22716-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="22716-236">En outre, étant donné que la méthode n’a aucun paramètre, il est mappé à un URI qui ne contient-elle pas une *&quot;id&quot;* segment dans le chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="22716-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="22716-237">Pour obtenir un produit par ID, ajoutez cette méthode à la `ProductsController` classe :</span><span class="sxs-lookup"><span data-stu-id="22716-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="22716-238">Nom de la méthode commence également par &quot;obtenir&quot;, mais la méthode a un paramètre nommé *id*. Ce paramètre est mappé à la &quot;id&quot; segment du chemin d’accès de l’URI.</span><span class="sxs-lookup"><span data-stu-id="22716-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="22716-239">L’infrastructure de l’API Web ASP.NET convertit automatiquement l’ID pour le type de données correct (**int**) pour le paramètre.</span><span class="sxs-lookup"><span data-stu-id="22716-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="22716-240">La méthode GetProduct lève une exception de type **HttpResponseException** si *id* n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="22716-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="22716-241">Cette exception sera traduite par l’infrastructure en une erreur 404 (introuvable).</span><span class="sxs-lookup"><span data-stu-id="22716-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="22716-242">Enfin, ajoutez une méthode pour rechercher des produits par catégorie :</span><span class="sxs-lookup"><span data-stu-id="22716-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="22716-243">Si l’URI de requête a une chaîne de requête, API Web tente de faire correspondre les paramètres de requête aux paramètres de la méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="22716-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="22716-244">Par conséquent, un URI au format « api/products ? catégorie =*catégorie*» est mappé à cette méthode.</span><span class="sxs-lookup"><span data-stu-id="22716-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="22716-245">Création d’une ressource</span><span class="sxs-lookup"><span data-stu-id="22716-245">Creating a Resource</span></span>

<span data-ttu-id="22716-246">Ensuite, nous allons ajouter une méthode à la `ProductsController` classe pour créer un nouveau produit.</span><span class="sxs-lookup"><span data-stu-id="22716-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="22716-247">Voici une implémentation simple de la méthode :</span><span class="sxs-lookup"><span data-stu-id="22716-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="22716-248">Noter deux choses concernant cette méthode :</span><span class="sxs-lookup"><span data-stu-id="22716-248">Note two things about this method:</span></span>

- <span data-ttu-id="22716-249">Le nom de la méthode commence par &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="22716-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="22716-250">Pour créer un nouveau produit, le client envoie une requête HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="22716-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="22716-251">La méthode prend un paramètre de type de produit.</span><span class="sxs-lookup"><span data-stu-id="22716-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="22716-252">Dans l’API Web, les paramètres avec des types complexes sont désérialisés à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="22716-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="22716-253">Par conséquent, nous pensons que le client envoie une représentation sérialisée d’un objet de produit, au format XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="22716-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="22716-254">Cette implémentation fonctionnera, mais il n’est pas tout à fait complet.</span><span class="sxs-lookup"><span data-stu-id="22716-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="22716-255">Dans l’idéal, nous aimerions la réponse HTTP à inclure les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="22716-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="22716-256">**Code de réponse :** par défaut, l’infrastructure API Web définit le code d’état de réponse 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="22716-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="22716-257">Mais, conformément au protocole HTTP/1.1, quand une demande POST entraîne la création d’une ressource, le serveur doit répondre avec l’état 201 (créé).</span><span class="sxs-lookup"><span data-stu-id="22716-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="22716-258">**Emplacement :** lorsque le serveur crée une ressource, il doit inclure l’URI de la nouvelle ressource dans l’en-tête Location de la réponse.</span><span class="sxs-lookup"><span data-stu-id="22716-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="22716-259">API Web ASP.NET rend plus facile manipuler le message de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="22716-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="22716-260">Voici l’implémentation améliorée :</span><span class="sxs-lookup"><span data-stu-id="22716-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="22716-261">Notez que le type de retour de méthode est désormais **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="22716-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="22716-262">En retournant un **HttpResponseMessage** au lieu d’un produit, nous pouvons contrôler les détails du message de réponse HTTP, y compris le code d’état et l’en-tête Location.</span><span class="sxs-lookup"><span data-stu-id="22716-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="22716-263">Le **CreateResponse** méthode crée un **HttpResponseMessage** et automatiquement écrit une représentation sérialisée de l’objet Product dans le corps de fo le message de réponse.</span><span class="sxs-lookup"><span data-stu-id="22716-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="22716-264">Cet exemple ne valide pas le `Product`.</span><span class="sxs-lookup"><span data-stu-id="22716-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="22716-265">Pour plus d’informations sur la validation de modèle, consultez [Validation de modèle dans ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="22716-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="22716-266">La mise à jour une ressource</span><span class="sxs-lookup"><span data-stu-id="22716-266">Updating a Resource</span></span>

<span data-ttu-id="22716-267">La mise à jour d’un produit avec la méthode PUT est simple :</span><span class="sxs-lookup"><span data-stu-id="22716-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="22716-268">Le nom de la méthode commence par &quot;placer... &quot;, de sorte que l’API Web correspondant à des demandes PUT.</span><span class="sxs-lookup"><span data-stu-id="22716-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="22716-269">La méthode accepte deux paramètres, l’ID de produit et les produits mis à jour.</span><span class="sxs-lookup"><span data-stu-id="22716-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="22716-270">Le *id* paramètre est issu le chemin d’accès de l’URI et la *produit* paramètre est désérialisé à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="22716-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="22716-271">Par défaut, l’infrastructure de l’API Web ASP.NET effectue des types de paramètres simples à partir de l’itinéraire et les types complexes à partir du corps de demande.</span><span class="sxs-lookup"><span data-stu-id="22716-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="22716-272">Suppression d’une ressource</span><span class="sxs-lookup"><span data-stu-id="22716-272">Deleting a Resource</span></span>

<span data-ttu-id="22716-273">Pour supprimer une ressource, définissez une méthode de « Supprimer... ».</span><span class="sxs-lookup"><span data-stu-id="22716-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="22716-274">Si une demande de suppression réussit, elle peut retourner l’état 200 (OK) avec un corps d’entité qui décrit l’état ; état 202 (accepté) si la suppression est toujours en attente ; état ou 204 (aucun contenu) sans corps d’entité.</span><span class="sxs-lookup"><span data-stu-id="22716-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="22716-275">Dans ce cas, le `DeleteProduct` méthode a un `void` type de retour, afin de l’API Web ASP.NET traduit automatiquement cette situation en état code 204 (aucun contenu).</span><span class="sxs-lookup"><span data-stu-id="22716-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
