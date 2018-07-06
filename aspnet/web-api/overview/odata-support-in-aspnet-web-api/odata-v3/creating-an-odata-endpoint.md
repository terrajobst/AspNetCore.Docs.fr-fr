---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Création d’un point de terminaison OData v3 avec Web API 2 | Microsoft Docs
author: MikeWasson
description: L’Open Data Protocol (OData) est un protocole d’accès aux données pour le web. OData fournit une méthode uniforme pour structurer les données, interroger les données et manipuler les données...
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 67a41f37a09d089336dc96d393f929e56661c4f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813703"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="b482c-104">Création d’un point de terminaison OData v3 avec Web API 2</span><span class="sxs-lookup"><span data-stu-id="b482c-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="b482c-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b482c-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b482c-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="b482c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="b482c-107">Le [Open Data Protocol](http://www.odata.org/) (OData) est un protocole d’accès aux données pour le web.</span><span class="sxs-lookup"><span data-stu-id="b482c-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="b482c-108">OData offre un moyen uniforme pour structurer les données, interroger les données et manipuler le jeu de données par le biais des opérations CRUD (créer, lire, mettre à jour et supprimer).</span><span class="sxs-lookup"><span data-stu-id="b482c-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="b482c-109">OData prend en charge AtomPub (XML) et les formats JSON.</span><span class="sxs-lookup"><span data-stu-id="b482c-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="b482c-110">OData définit également un moyen d’exposer des métadonnées relatives aux données.</span><span class="sxs-lookup"><span data-stu-id="b482c-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="b482c-111">Les clients peuvent utiliser les métadonnées pour découvrir les informations de type et les relations pour le jeu de données.</span><span class="sxs-lookup"><span data-stu-id="b482c-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="b482c-112">API Web ASP.NET rend plus facile créer un point de terminaison OData pour un jeu de données.</span><span class="sxs-lookup"><span data-stu-id="b482c-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="b482c-113">Vous pouvez contrôler exactement les opérations OData prend en charge par le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="b482c-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="b482c-114">Vous pouvez héberger plusieurs points de terminaison OData, en même temps que les points de terminaison non OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="b482c-115">Vous avez un contrôle total sur votre modèle de données, logique métier du serveur principal et la couche de données.</span><span class="sxs-lookup"><span data-stu-id="b482c-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b482c-116">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="b482c-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b482c-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b482c-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b482c-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="b482c-118">Web API 2</span></span>
> - <span data-ttu-id="b482c-119">OData Version 3</span><span class="sxs-lookup"><span data-stu-id="b482c-119">OData Version 3</span></span>
> - <span data-ttu-id="b482c-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b482c-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="b482c-121">Web Fiddler Proxy (facultatif) de débogage</span><span class="sxs-lookup"><span data-stu-id="b482c-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="b482c-122">Prise en charge de Web API OData a été ajoutée dans [ASP.NET et Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="b482c-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="b482c-123">Toutefois, ce didacticiel utilise la génération de modèles automatique qui a été ajoutée dans Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b482c-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="b482c-124">Dans ce didacticiel, vous allez créer un point de terminaison OData simples, les clients peuvent interroger.</span><span class="sxs-lookup"><span data-stu-id="b482c-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="b482c-125">Vous allez également créer un client c# pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="b482c-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="b482c-126">Après avoir terminé ce didacticiel, l’ensemble suivant de didacticiels montrent comment ajouter des fonctionnalités, y compris les relations d’entité, actions, puis développez $/ $.</span><span class="sxs-lookup"><span data-stu-id="b482c-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="b482c-127">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b482c-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="b482c-128">Ajouter un modèle d’entité</span><span class="sxs-lookup"><span data-stu-id="b482c-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="b482c-129">Ajouter un contrôleur OData</span><span class="sxs-lookup"><span data-stu-id="b482c-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="b482c-130">Ajouter le modèle EDM et l’itinéraire</span><span class="sxs-lookup"><span data-stu-id="b482c-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="b482c-131">Valeur initiale de la base de données (facultatif)</span><span class="sxs-lookup"><span data-stu-id="b482c-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="b482c-132">Explorer le point de terminaison OData</span><span class="sxs-lookup"><span data-stu-id="b482c-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="b482c-133">Formats de sérialisation OData</span><span class="sxs-lookup"><span data-stu-id="b482c-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="b482c-134">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b482c-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="b482c-135">Dans ce didacticiel, vous allez créer un point de terminaison OData qui prend en charge les opérations CRUD de base.</span><span class="sxs-lookup"><span data-stu-id="b482c-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="b482c-136">Le point de terminaison expose une ressource unique, une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="b482c-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="b482c-137">Les didacticiels suivants ajouter d’autres fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="b482c-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="b482c-138">Démarrez Visual Studio et sélectionnez **nouveau projet** à partir de la page de démarrage.</span><span class="sxs-lookup"><span data-stu-id="b482c-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="b482c-139">Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.</span><span class="sxs-lookup"><span data-stu-id="b482c-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="b482c-140">Dans le **modèles** volet, sélectionnez **modèles installés** et développez le nœud Visual c#.</span><span class="sxs-lookup"><span data-stu-id="b482c-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="b482c-141">Sous **Visual C#**, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="b482c-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="b482c-142">Sélectionnez **l’Application Web ASP.NET** modèle.</span><span class="sxs-lookup"><span data-stu-id="b482c-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="b482c-143">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **vide** modèle.</span><span class="sxs-lookup"><span data-stu-id="b482c-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="b482c-144">Sous &quot;ajouter des dossiers et les références principales pour... &quot;, vérifiez **API Web**.</span><span class="sxs-lookup"><span data-stu-id="b482c-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="b482c-145">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b482c-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="b482c-146">Ajouter un modèle d’entité</span><span class="sxs-lookup"><span data-stu-id="b482c-146">Add an Entity Model</span></span>

<span data-ttu-id="b482c-147">Un *modèle* est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="b482c-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="b482c-148">Pour ce didacticiel, nous avons besoin d’un modèle qui représente un produit.</span><span class="sxs-lookup"><span data-stu-id="b482c-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="b482c-149">Le modèle correspond à notre type d’entité OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="b482c-150">Dans l’Explorateur de solutions, cliquez sur le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="b482c-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="b482c-151">Dans le menu contextuel, sélectionnez **ajouter** puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="b482c-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="b482c-152">Dans le **Ajouter nouveau** élément de boîte de dialogue, nommez la classe &quot;produit&quot;.</span><span class="sxs-lookup"><span data-stu-id="b482c-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="b482c-153">Par convention, les classes de modèle sont placés dans le dossier Models.</span><span class="sxs-lookup"><span data-stu-id="b482c-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="b482c-154">Vous n’êtes pas obligé de suivre cette convention dans vos propres projets, mais nous allons l’utiliser pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="b482c-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="b482c-155">Dans le fichier Product.cs, ajoutez la définition de classe suivante :</span><span class="sxs-lookup"><span data-stu-id="b482c-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="b482c-156">La propriété ID sera la clé d’entité.</span><span class="sxs-lookup"><span data-stu-id="b482c-156">The ID property will be the entity key.</span></span> <span data-ttu-id="b482c-157">Les clients peuvent interroger les produits par ID.</span><span class="sxs-lookup"><span data-stu-id="b482c-157">Clients can query products by ID.</span></span> <span data-ttu-id="b482c-158">Ce champ serait également la clé primaire dans la base de données back-end.</span><span class="sxs-lookup"><span data-stu-id="b482c-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="b482c-159">Générez le projet maintenant.</span><span class="sxs-lookup"><span data-stu-id="b482c-159">Build the project now.</span></span> <span data-ttu-id="b482c-160">Dans l’étape suivante, nous allons utiliser une structure de Visual Studio qui utilise la réflexion pour rechercher le type de produit.</span><span class="sxs-lookup"><span data-stu-id="b482c-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="b482c-161">Ajouter un contrôleur OData</span><span class="sxs-lookup"><span data-stu-id="b482c-161">Add an OData Controller</span></span>

<span data-ttu-id="b482c-162">Un *contrôleur* est une classe qui gère les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b482c-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="b482c-163">Vous définissez un contrôleur séparé pour chaque entité définie dans votre service OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="b482c-164">Dans ce didacticiel, nous allons créer un seul contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b482c-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="b482c-165">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="b482c-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="b482c-166">Sélectionnez **ajouter** , puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="b482c-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="b482c-167">Dans le **ajouter une structure** boîte de dialogue, sélectionnez &quot;Web API 2 OData contrôleur avec actions, utilisant Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="b482c-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="b482c-168">Dans le **ajouter un contrôleur** boîte de dialogue, nommez le contrôleur « ProductsController ».</span><span class="sxs-lookup"><span data-stu-id="b482c-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="b482c-169">Sélectionnez le &quot;utiliser les actions de contrôleur asynchrones&quot; case à cocher.</span><span class="sxs-lookup"><span data-stu-id="b482c-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="b482c-170">Dans le **modèle** liste déroulante, sélectionnez la classe Product.</span><span class="sxs-lookup"><span data-stu-id="b482c-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="b482c-171">Cliquez sur le **nouveau contexte de données...**  bouton.</span><span class="sxs-lookup"><span data-stu-id="b482c-171">Click the **New data context...** button.</span></span> <span data-ttu-id="b482c-172">Laissez le nom par défaut pour le type de contexte de données, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b482c-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="b482c-173">Cliquez sur Ajouter dans la boîte de dialogue Ajouter un contrôleur pour ajouter le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b482c-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="b482c-174">Remarque : Si vous obtenez un message d’erreur qui indique que &quot;comportait une erreur d’obtention du type... &quot;, assurez-vous que vous avez créé le projet Visual Studio une fois que vous avez ajouté la classe Product.</span><span class="sxs-lookup"><span data-stu-id="b482c-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="b482c-175">La génération de modèles automatique utilise la réflexion pour trouver la classe.</span><span class="sxs-lookup"><span data-stu-id="b482c-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="b482c-176">La génération de modèles automatique ajoute deux fichiers de code au projet :</span><span class="sxs-lookup"><span data-stu-id="b482c-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="b482c-177">Products.cs définit le contrôleur d’API Web qui implémente le point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="b482c-178">ProductServiceContext.cs fournit des méthodes pour interroger la base de données sous-jacente à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b482c-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="b482c-179">Ajouter le modèle EDM et l’itinéraire</span><span class="sxs-lookup"><span data-stu-id="b482c-179">Add the EDM and Route</span></span>

<span data-ttu-id="b482c-180">Dans l’Explorateur de solutions, développez l’application\_dossier Démarrer et d’ouvrir le fichier WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="b482c-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="b482c-181">Cette classe concerne le code de configuration pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="b482c-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="b482c-182">Remplacez ce code avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b482c-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="b482c-183">Ce code effectue deux opérations :</span><span class="sxs-lookup"><span data-stu-id="b482c-183">This code does two things:</span></span>

- <span data-ttu-id="b482c-184">Crée un modèle EDM (Entity Data) pour le point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="b482c-185">Ajoute un itinéraire pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="b482c-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="b482c-186">Un modèle EDM est un modèle abstrait des données.</span><span class="sxs-lookup"><span data-stu-id="b482c-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="b482c-187">Le modèle EDM est utilisé pour créer le document de métadonnées et de définir l’URI pour le service.</span><span class="sxs-lookup"><span data-stu-id="b482c-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="b482c-188">Le **ODataConventionModelBuilder** crée un modèle EDM à l’aide d’un ensemble de conventions d’affectation de noms par défaut EDM.</span><span class="sxs-lookup"><span data-stu-id="b482c-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="b482c-189">Cette approche nécessite le moins de code.</span><span class="sxs-lookup"><span data-stu-id="b482c-189">This approach requires the least code.</span></span> <span data-ttu-id="b482c-190">Si vous souhaitez davantage de contrôle sur le modèle EDM, vous pouvez utiliser la **ODataModelBuilder** classe pour créer le modèle EDM en ajoutant explicitement les propriétés, les clés et les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="b482c-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="b482c-191">Le **EntitySet** méthode ajoute une jeu d’entités à l’EDM :</span><span class="sxs-lookup"><span data-stu-id="b482c-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="b482c-192">La chaîne « Produits » définit le nom du jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="b482c-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="b482c-193">Le nom du contrôleur doit correspondre au nom du jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="b482c-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="b482c-194">Dans ce didacticiel, le jeu d’entités est nommé « Produits » et le contrôleur est nommé `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b482c-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="b482c-195">Si vous avez nommé « ProductSet » du jeu d’entités, vous devez nommer le contrôleur `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="b482c-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="b482c-196">Notez qu’un point de terminaison peut avoir plusieurs jeux d’entités.</span><span class="sxs-lookup"><span data-stu-id="b482c-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="b482c-197">Appelez **EntitySet&lt;T&gt;**  pour chaque entité définie, puis définir un contrôleur correspondant.</span><span class="sxs-lookup"><span data-stu-id="b482c-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="b482c-198">Le **MapODataRoute** méthode ajoute un itinéraire pour le point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="b482c-199">Le premier paramètre est un nom convivial pour l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="b482c-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="b482c-200">Ce nom n’apparaît pas dans les clients de votre service.</span><span class="sxs-lookup"><span data-stu-id="b482c-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="b482c-201">Le deuxième paramètre est le préfixe URI pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="b482c-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="b482c-202">Étant donné ce code, l’URI pour le jeu d’entités de produits est http://<em>nom d’hôte</em>  /odata/produits.</span><span class="sxs-lookup"><span data-stu-id="b482c-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="b482c-203">Votre application peut avoir plus d’un point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="b482c-204">Pour chaque point de terminaison, appelez <strong>MapODataRoute</strong> et fournir un nom d’itinéraire unique et un préfixe URI unique.</span><span class="sxs-lookup"><span data-stu-id="b482c-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="b482c-205">Valeur initiale de la base de données (facultatif)</span><span class="sxs-lookup"><span data-stu-id="b482c-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="b482c-206">Dans cette étape, vous allez utiliser Entity Framework pour amorcer la base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="b482c-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="b482c-207">Cette étape est facultative, mais il vous permet de tester votre point de terminaison OData tout de suite.</span><span class="sxs-lookup"><span data-stu-id="b482c-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="b482c-208">À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="b482c-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b482c-209">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b482c-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="b482c-210">Cette opération ajoute un dossier nommé Migrations et un fichier de code nommé Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="b482c-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="b482c-211">Ouvrez ce fichier et ajoutez le code suivant à la `Configuration.Seed` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b482c-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="b482c-212">Dans la fenêtre de Console du Gestionnaire de Package, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b482c-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="b482c-213">Ces commandes génèrent un code qui crée la base de données, puis l’exécute ce code.</span><span class="sxs-lookup"><span data-stu-id="b482c-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="b482c-214">Explorer le point de terminaison OData</span><span class="sxs-lookup"><span data-stu-id="b482c-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="b482c-215">Dans cette section, nous allons utiliser le [Fiddler Web Debugging Proxy](http://www.fiddler2.com) pour envoyer des demandes au point de terminaison et examinez les messages de réponse.</span><span class="sxs-lookup"><span data-stu-id="b482c-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="b482c-216">Cela vous aidera à comprendre les fonctionnalités d’un point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="b482c-217">Dans Visual Studio, appuyez sur F5 pour démarrer le débogage.</span><span class="sxs-lookup"><span data-stu-id="b482c-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="b482c-218">Par défaut, Visual Studio ouvre votre navigateur pour `http://localhost:*port*`, où *port* est le numéro de port configuré dans les paramètres du projet.</span><span class="sxs-lookup"><span data-stu-id="b482c-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="b482c-219">Vous pouvez modifier le numéro de port dans les paramètres du projet.</span><span class="sxs-lookup"><span data-stu-id="b482c-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="b482c-220">Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="b482c-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="b482c-221">Dans la fenêtre Propriétés, sélectionnez **Web**.</span><span class="sxs-lookup"><span data-stu-id="b482c-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="b482c-222">Entrez le numéro de port sous **Url du projet**.</span><span class="sxs-lookup"><span data-stu-id="b482c-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="b482c-223">Document de service</span><span class="sxs-lookup"><span data-stu-id="b482c-223">Service Document</span></span>

<span data-ttu-id="b482c-224">Le *document de service* contient une liste des jeux d’entités pour le point de terminaison OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="b482c-225">Pour obtenir le document de service, envoie une requête GET à l’URI du service racine.</span><span class="sxs-lookup"><span data-stu-id="b482c-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="b482c-226">À l’aide de Fiddler, entrez l’URI suivant dans le **Composer** onglet : `http://localhost:port/odata/`, où *port* est le numéro de port.</span><span class="sxs-lookup"><span data-stu-id="b482c-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="b482c-227">Cliquez sur le **Execute** bouton.</span><span class="sxs-lookup"><span data-stu-id="b482c-227">Click the **Execute** button.</span></span> <span data-ttu-id="b482c-228">Fiddler envoie une requête HTTP GET à votre application.</span><span class="sxs-lookup"><span data-stu-id="b482c-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="b482c-229">Vous devez voir la réponse dans la liste des Sessions Web.</span><span class="sxs-lookup"><span data-stu-id="b482c-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="b482c-230">Si tout fonctionne, le code d’état sera de 200.</span><span class="sxs-lookup"><span data-stu-id="b482c-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="b482c-231">Double-cliquez sur la réponse dans la liste des Sessions Web pour afficher les détails du message de réponse dans l’onglet inspecteurs.</span><span class="sxs-lookup"><span data-stu-id="b482c-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="b482c-232">Le message de réponse HTTP brut doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="b482c-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="b482c-233">Par défaut, les API Web retourne le document de service dans le format AtomPub.</span><span class="sxs-lookup"><span data-stu-id="b482c-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="b482c-234">Pour demander le JSON, ajoutez l’en-tête suivant à la requête HTTP :</span><span class="sxs-lookup"><span data-stu-id="b482c-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="b482c-235">La réponse HTTP contient maintenant une charge utile JSON :</span><span class="sxs-lookup"><span data-stu-id="b482c-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="b482c-236">Document de métadonnées de service</span><span class="sxs-lookup"><span data-stu-id="b482c-236">Service Metadata Document</span></span>

<span data-ttu-id="b482c-237">Le *document de métadonnées de service* décrit le modèle de données du service, à l’aide d’un langage XML appelé le langage de définition de schéma conceptuel (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b482c-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="b482c-238">Le document de métadonnées montre la structure des données dans le service et peut être utilisé pour générer le code client.</span><span class="sxs-lookup"><span data-stu-id="b482c-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="b482c-239">Pour obtenir le document de métadonnées, envoyez une demande GET à `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="b482c-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="b482c-240">Voici les métadonnées du point de terminaison indiqué dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="b482c-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="b482c-241">Jeu d'entités</span><span class="sxs-lookup"><span data-stu-id="b482c-241">Entity Set</span></span>

<span data-ttu-id="b482c-242">Pour obtenir le jeu d’entités de produits, envoyez une demande GET à `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="b482c-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="b482c-243">Entité</span><span class="sxs-lookup"><span data-stu-id="b482c-243">Entity</span></span>

<span data-ttu-id="b482c-244">Pour obtenir un produit individuel, envoyez une demande GET à `http://localhost:port/odata/Products(1)`, où « 1 » est l’ID de produit.</span><span class="sxs-lookup"><span data-stu-id="b482c-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="b482c-245">Formats de sérialisation OData</span><span class="sxs-lookup"><span data-stu-id="b482c-245">OData Serialization Formats</span></span>

<span data-ttu-id="b482c-246">OData prend en charge plusieurs formats de sérialisation :</span><span class="sxs-lookup"><span data-stu-id="b482c-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="b482c-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="b482c-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="b482c-248">JSON « light » (introduite dans OData v3)</span><span class="sxs-lookup"><span data-stu-id="b482c-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="b482c-249">JSON « commentaires » (OData v2)</span><span class="sxs-lookup"><span data-stu-id="b482c-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="b482c-250">Par défaut, API Web utilise le format de « light » AtomPubJSON.</span><span class="sxs-lookup"><span data-stu-id="b482c-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="b482c-251">Pour obtenir le format AtomPub, définissez l’en-tête Accept à « application/atom + xml ».</span><span class="sxs-lookup"><span data-stu-id="b482c-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="b482c-252">Voici un exemple de corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="b482c-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="b482c-253">Vous pouvez voir l’un des inconvénients évidents au format Atom : il est beaucoup plus longue que le JSON light.</span><span class="sxs-lookup"><span data-stu-id="b482c-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="b482c-254">Toutefois, si vous avez un client qui comprend AtomPub, le client préférerez peut-être ce format sur JSON.</span><span class="sxs-lookup"><span data-stu-id="b482c-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="b482c-255">Voici la version légère de JSON de la même entité :</span><span class="sxs-lookup"><span data-stu-id="b482c-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="b482c-256">Le format JSON light a été introduit dans la version 3 du protocole OData.</span><span class="sxs-lookup"><span data-stu-id="b482c-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="b482c-257">Pour la compatibilité descendante, un client peut demander l’ancien format JSON « commentaires ».</span><span class="sxs-lookup"><span data-stu-id="b482c-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="b482c-258">Pour demander le JSON détaillés, la valeur est l’en-tête Accept `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="b482c-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="b482c-259">Voici la version détaillée :</span><span class="sxs-lookup"><span data-stu-id="b482c-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="b482c-260">Ce format véhicule davantage de métadonnées dans le corps de réponse, ce qui peut ajouter une charge considérable sur une session entière.</span><span class="sxs-lookup"><span data-stu-id="b482c-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="b482c-261">En outre, il ajoute un niveau d’indirection en encapsulant l’objet dans une propriété nommée « d ».</span><span class="sxs-lookup"><span data-stu-id="b482c-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="b482c-262">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="b482c-262">Next Steps</span></span>

- [<span data-ttu-id="b482c-263">Ajouter des Relations d’entité</span><span class="sxs-lookup"><span data-stu-id="b482c-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="b482c-264">Ajouter des Actions OData</span><span class="sxs-lookup"><span data-stu-id="b482c-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="b482c-265">Appeler le Service OData à partir d’un Client .NET</span><span class="sxs-lookup"><span data-stu-id="b482c-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
