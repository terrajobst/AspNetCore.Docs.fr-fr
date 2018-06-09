---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Créer un point de terminaison OData v4 à l’aide d’ASP.NET Web API 2.2 | Documents Microsoft
author: MikeWasson
description: Le protocole Open Data Protocol (OData) est un protocole d’accès aux données pour le web. OData fournit une méthode uniforme pour interroger et manipuler des jeux de données via des opérations CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508048"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="71e3a-104">Créer un point de terminaison OData v4 à l’aide d’ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="71e3a-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="71e3a-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="71e3a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="71e3a-106">Le protocole Open Data Protocol (OData) est un protocole d’accès aux données pour le web.</span><span class="sxs-lookup"><span data-stu-id="71e3a-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="71e3a-107">OData fournit une méthode uniforme pour interroger et manipuler des jeux de données via des opérations CRUD (créer, lire, mettre à jour et supprimer).</span><span class="sxs-lookup"><span data-stu-id="71e3a-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="71e3a-108">ASP.NET Web API prend en charge v3 et v4 du protocole.</span><span class="sxs-lookup"><span data-stu-id="71e3a-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="71e3a-109">Vous pouvez même demander un point de terminaison v4 qui s’exécute côte à côte avec un point de terminaison v3.</span><span class="sxs-lookup"><span data-stu-id="71e3a-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="71e3a-110">Ce didacticiel montre comment créer un point de terminaison OData v4 qui prend en charge les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="71e3a-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="71e3a-111">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="71e3a-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="71e3a-112">2.2 d’API Web</span><span class="sxs-lookup"><span data-stu-id="71e3a-112">Web API 2.2</span></span>
> - <span data-ttu-id="71e3a-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="71e3a-113">OData v4</span></span>
> - [<span data-ttu-id="71e3a-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="71e3a-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="71e3a-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="71e3a-115">Entity Framework 6</span></span>
> - <span data-ttu-id="71e3a-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="71e3a-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="71e3a-117">Versions du didacticiels</span><span class="sxs-lookup"><span data-stu-id="71e3a-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="71e3a-118">Pour la Version 3 d’OData, consultez [création d’un point de terminaison OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="71e3a-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="71e3a-119">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71e3a-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="71e3a-120">Dans Visual Studio, à partir de la **fichier** menu, sélectionnez **nouveau** &gt; **projet**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="71e3a-121">Développez **installé** &gt; **modèles** &gt; **Visual C#** &gt; **Web**, puis sélectionnez le  **Application Web ASP.NET** modèle.</span><span class="sxs-lookup"><span data-stu-id="71e3a-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="71e3a-122">Nommez le projet &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="71e3a-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="71e3a-123">Dans le **nouveau projet** boîte de dialogue, sélectionnez le **vide** modèle.</span><span class="sxs-lookup"><span data-stu-id="71e3a-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="71e3a-124">Sous &quot;ajouter des dossiers et les références de base... &quot;, cliquez sur **API Web**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="71e3a-125">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="71e3a-126">Installer les Packages OData</span><span class="sxs-lookup"><span data-stu-id="71e3a-126">Install the OData Packages</span></span>

<span data-ttu-id="71e3a-127">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet** &gt; **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="71e3a-128">Dans la fenêtre de Console du Gestionnaire de Package, tapez :</span><span class="sxs-lookup"><span data-stu-id="71e3a-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="71e3a-129">Cette commande installe les packages OData NuGet plus récente.</span><span class="sxs-lookup"><span data-stu-id="71e3a-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="71e3a-130">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="71e3a-130">Add a Model Class</span></span>

<span data-ttu-id="71e3a-131">A *modèle* est un objet qui représente une entité de données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="71e3a-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="71e3a-132">Dans l’Explorateur de solutions, cliquez sur le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="71e3a-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="71e3a-133">Dans le menu contextuel, sélectionnez **ajouter** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="71e3a-134">Par convention, les classes de modèle sont placées dans le dossier de modèles, mais vous n’êtes pas obligé de suivent cette convention dans vos propres projets.</span><span class="sxs-lookup"><span data-stu-id="71e3a-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="71e3a-135">Nommez la classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="71e3a-135">Name the class `Product`.</span></span> <span data-ttu-id="71e3a-136">Dans le fichier Product.cs, remplacez le code réutilisable avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="71e3a-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="71e3a-137">Le `Id` propriété est la clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="71e3a-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="71e3a-138">Les clients peuvent interroger des entités par clé.</span><span class="sxs-lookup"><span data-stu-id="71e3a-138">Clients can query entities by key.</span></span> <span data-ttu-id="71e3a-139">Par exemple, pour obtenir le produit avec l’ID de 5, l’URI est `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="71e3a-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="71e3a-140">Le `Id` propriété sera également être la clé primaire dans la base de données principale.</span><span class="sxs-lookup"><span data-stu-id="71e3a-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="71e3a-141">Activer l’Entity Framework</span><span class="sxs-lookup"><span data-stu-id="71e3a-141">Enable Entity Framework</span></span>

<span data-ttu-id="71e3a-142">Pour ce didacticiel, nous allons utiliser Entity Framework (EF) Code First pour créer la base de données principale.</span><span class="sxs-lookup"><span data-stu-id="71e3a-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="71e3a-143">OData d’API Web ne nécessite pas de EF.</span><span class="sxs-lookup"><span data-stu-id="71e3a-143">Web API OData does not require EF.</span></span> <span data-ttu-id="71e3a-144">Utilisez n’importe quelle couche d’accès aux données qui peut se traduire par des entités de base de données des modèles.</span><span class="sxs-lookup"><span data-stu-id="71e3a-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="71e3a-145">Tout d’abord, installez le package NuGet pour EF.</span><span class="sxs-lookup"><span data-stu-id="71e3a-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="71e3a-146">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet** &gt; **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="71e3a-147">Dans la fenêtre de Console du Gestionnaire de Package, tapez :</span><span class="sxs-lookup"><span data-stu-id="71e3a-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="71e3a-148">Ouvrez le fichier Web.config et ajoutez la section suivante à l’intérieur de la **configuration** élément, après le **configSections** élément.</span><span class="sxs-lookup"><span data-stu-id="71e3a-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="71e3a-149">Ce paramètre ajoute une chaîne de connexion pour une base de données de la base de données locale.</span><span class="sxs-lookup"><span data-stu-id="71e3a-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="71e3a-150">Cette base de données sera utilisé lorsque vous exécutez l’application localement.</span><span class="sxs-lookup"><span data-stu-id="71e3a-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="71e3a-151">Ensuite, ajoutez une classe nommée `ProductsContext` dans le dossier Modèles :</span><span class="sxs-lookup"><span data-stu-id="71e3a-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="71e3a-152">Dans le constructeur, `"name=ProductsContext"` donne le nom de la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="71e3a-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="71e3a-153">Configurer le point de terminaison OData</span><span class="sxs-lookup"><span data-stu-id="71e3a-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="71e3a-154">Ouvrez le fichier App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="71e3a-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="71e3a-155">Ajoutez le code suivant **à l’aide de** instructions :</span><span class="sxs-lookup"><span data-stu-id="71e3a-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="71e3a-156">Puis ajoutez le code suivant à la **inscrire** méthode :</span><span class="sxs-lookup"><span data-stu-id="71e3a-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="71e3a-157">Ce code effectue deux opérations :</span><span class="sxs-lookup"><span data-stu-id="71e3a-157">This code does two things:</span></span>

- <span data-ttu-id="71e3a-158">Crée un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="71e3a-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="71e3a-159">Ajoute un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="71e3a-159">Adds a route.</span></span>

<span data-ttu-id="71e3a-160">Un modèle EDM est un modèle abstrait des données.</span><span class="sxs-lookup"><span data-stu-id="71e3a-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="71e3a-161">Le modèle EDM est utilisé pour créer le document de métadonnées de service.</span><span class="sxs-lookup"><span data-stu-id="71e3a-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="71e3a-162">Le **ODataConventionModelBuilder** classe crée un modèle EDM à l’aide des conventions d’affectation de noms par défaut.</span><span class="sxs-lookup"><span data-stu-id="71e3a-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="71e3a-163">Cette approche nécessite le moins de code.</span><span class="sxs-lookup"><span data-stu-id="71e3a-163">This approach requires the least code.</span></span> <span data-ttu-id="71e3a-164">Si vous souhaitez davantage de contrôle sur le modèle EDM, vous pouvez utiliser la **odatamodelbuilder ayant** classe pour créer le modèle EDM en ajoutant explicitement les propriétés, les clés et les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="71e3a-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="71e3a-165">A *itinéraire* indique la façon de router les requêtes HTTP vers le point de terminaison API Web.</span><span class="sxs-lookup"><span data-stu-id="71e3a-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="71e3a-166">Pour créer un itinéraire d’OData v4, appelez le **MapODataServiceRoute** méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="71e3a-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="71e3a-167">Si votre application comporte plusieurs points de terminaison OData, créez un itinéraire distinct pour chacun.</span><span class="sxs-lookup"><span data-stu-id="71e3a-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="71e3a-168">Un nom d’itinéraire unique et un préfixe, donnez à chaque itinéraire.</span><span class="sxs-lookup"><span data-stu-id="71e3a-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="71e3a-169">Ajouter le contrôleur OData</span><span class="sxs-lookup"><span data-stu-id="71e3a-169">Add the OData Controller</span></span>

<span data-ttu-id="71e3a-170">A *contrôleur* est une classe qui gère les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="71e3a-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="71e3a-171">Vous créez un contrôleur distinct pour chaque jeu d’entités dans votre service OData.</span><span class="sxs-lookup"><span data-stu-id="71e3a-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="71e3a-172">Dans ce didacticiel, vous allez créer un seul contrôleur, pour le `Product` entité.</span><span class="sxs-lookup"><span data-stu-id="71e3a-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="71e3a-173">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs, puis sélectionnez **ajouter** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="71e3a-174">Nommez la classe `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="71e3a-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="71e3a-175">La version de ce didacticiel pour OData v3 utilise le **ajouter un contrôleur** génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="71e3a-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="71e3a-176">Actuellement, il n’existe aucune génération de modèles automatique pour OData v4.</span><span class="sxs-lookup"><span data-stu-id="71e3a-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="71e3a-177">Remplacez le code réutilisable dans ProductsController.cs avec les éléments suivants.</span><span class="sxs-lookup"><span data-stu-id="71e3a-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="71e3a-178">Le contrôleur emploie la `ProductsContext` classe pour accéder à la base de données à l’aide de EF.</span><span class="sxs-lookup"><span data-stu-id="71e3a-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="71e3a-179">Notez que le contrôleur substitue le **Dispose** méthode pour les supprimer de la **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="71e3a-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="71e3a-180">Il s’agit du point de départ pour le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="71e3a-180">This is the starting point for the controller.</span></span> <span data-ttu-id="71e3a-181">Ensuite, nous allons ajouter des méthodes pour toutes les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="71e3a-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="71e3a-182">Interrogation de l’ensemble d’entités</span><span class="sxs-lookup"><span data-stu-id="71e3a-182">Querying the Entity Set</span></span>

<span data-ttu-id="71e3a-183">Ajoutez les méthodes suivantes pour `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="71e3a-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="71e3a-184">La version sans paramètre de la `Get` méthode retourne l’ensemble de produits.</span><span class="sxs-lookup"><span data-stu-id="71e3a-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="71e3a-185">Le `Get` méthode avec un *clé* paramètre recherche un produit par sa clé (dans ce cas, le `Id` propriété).</span><span class="sxs-lookup"><span data-stu-id="71e3a-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="71e3a-186">Le **[EnableQuery]** attribut permet aux clients de modifier la requête, à l’aide des options de requête comme $filter, $sort et $page.</span><span class="sxs-lookup"><span data-stu-id="71e3a-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="71e3a-187">Pour plus d’informations, consultez [prise en charge des Options de requête OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="71e3a-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="71e3a-188">Ajout d’une entité au jeu d’entités</span><span class="sxs-lookup"><span data-stu-id="71e3a-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="71e3a-189">Pour permettre aux clients d’ajouter un nouveau produit à la base de données, ajoutez la méthode suivante à `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="71e3a-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="71e3a-190">Mise à jour d'une entité</span><span class="sxs-lookup"><span data-stu-id="71e3a-190">Updating an Entity</span></span>

<span data-ttu-id="71e3a-191">OData prend en charge les deux une sémantique différente pour mettre à jour une entité, PATCH et PUT.</span><span class="sxs-lookup"><span data-stu-id="71e3a-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="71e3a-192">CORRECTIF effectue une mise à jour partielle.</span><span class="sxs-lookup"><span data-stu-id="71e3a-192">PATCH performs a partial update.</span></span> <span data-ttu-id="71e3a-193">Le client spécifie uniquement les propriétés à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="71e3a-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="71e3a-194">PUT remplace l’entité toute entière.</span><span class="sxs-lookup"><span data-stu-id="71e3a-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="71e3a-195">L’inconvénient de PUT est que le client doit envoyer des valeurs pour toutes les propriétés de l’entité, y compris les valeurs qui ne changent pas.</span><span class="sxs-lookup"><span data-stu-id="71e3a-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="71e3a-196">Le [spécification OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indique que le correctif est préféré.</span><span class="sxs-lookup"><span data-stu-id="71e3a-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="71e3a-197">Dans tous les cas, voici le code pour les méthodes PATCH et PUT :</span><span class="sxs-lookup"><span data-stu-id="71e3a-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="71e3a-198">Dans le cas de correction, le contrôleur utilise le **Delta&lt;T&gt;**  type pour le suivi des modifications.</span><span class="sxs-lookup"><span data-stu-id="71e3a-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="71e3a-199">Suppression d'une entité</span><span class="sxs-lookup"><span data-stu-id="71e3a-199">Deleting an Entity</span></span>

<span data-ttu-id="71e3a-200">Pour activer les clients supprimer un produit à partir de la base de données, ajoutez la méthode suivante à `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="71e3a-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
