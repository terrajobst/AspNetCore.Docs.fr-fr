---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: "Appel à un Service OData à partir d’un Client .NET (c#) | Documents Microsoft"
author: MikeWasson
description: "Ce didacticiel montre comment appeler un service OData à partir d’une application cliente c#. Versions des logiciels utilisées dans le didacticiel Visual Studio 2013 (fonctionne avec Visual S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: f6266045ebf55fb7ae691bfb55e9c90cd4edcc96
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="f9581-104">Appel à un Service OData à partir d’un Client .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="f9581-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="f9581-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f9581-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f9581-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="f9581-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="f9581-107">Ce didacticiel montre comment appeler un service OData à partir d’une application cliente c#.</span><span class="sxs-lookup"><span data-stu-id="f9581-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f9581-108">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="f9581-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f9581-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (fonctionne avec Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="f9581-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="f9581-110">Bibliothèque cliente WCF Data Services</span><span class="sxs-lookup"><span data-stu-id="f9581-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/en-us/library/cc668772.aspx)
> - <span data-ttu-id="f9581-111">API Web 2.</span><span class="sxs-lookup"><span data-stu-id="f9581-111">Web API 2.</span></span> <span data-ttu-id="f9581-112">(L’exemple de service OData est créé à l’aide des API Web 2, mais l’application cliente ne dépend pas des API Web).</span><span class="sxs-lookup"><span data-stu-id="f9581-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="f9581-113">Dans ce didacticiel, je vais guider la création d’une application cliente qui appelle un service OData.</span><span class="sxs-lookup"><span data-stu-id="f9581-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="f9581-114">Le service OData expose les entités suivantes :</span><span class="sxs-lookup"><span data-stu-id="f9581-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="f9581-115">Les articles suivants décrivent comment implémenter le service OData dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="f9581-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="f9581-116">(Vous n’avez pas besoin de les lire pour comprendre ce didacticiel, toutefois.)</span><span class="sxs-lookup"><span data-stu-id="f9581-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="f9581-117">Création d’un point de terminaison OData dans l’API Web 2</span><span class="sxs-lookup"><span data-stu-id="f9581-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="f9581-118">Relations d’entité OData dans l’API Web 2</span><span class="sxs-lookup"><span data-stu-id="f9581-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="f9581-119">Actions OData dans l’API Web 2</span><span class="sxs-lookup"><span data-stu-id="f9581-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="f9581-120">Générer le Proxy de Service</span><span class="sxs-lookup"><span data-stu-id="f9581-120">Generate the Service Proxy</span></span>

<span data-ttu-id="f9581-121">La première étape consiste à générer un proxy de service.</span><span class="sxs-lookup"><span data-stu-id="f9581-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="f9581-122">Le proxy de service est une classe .NET qui définit des méthodes pour accéder au service OData.</span><span class="sxs-lookup"><span data-stu-id="f9581-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="f9581-123">Le proxy traduit les appels de méthode dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f9581-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="f9581-124">Commencez par ouvrir le projet de service OData dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9581-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="f9581-125">Appuyez sur CTRL + F5 pour exécuter le service localement dans IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f9581-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="f9581-126">Notez l’adresse locale, y compris le numéro de port qui affecte de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9581-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="f9581-127">Vous aurez besoin de cette adresse lorsque vous créez le proxy.</span><span class="sxs-lookup"><span data-stu-id="f9581-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="f9581-128">Ensuite, ouvrez une autre instance de Visual Studio et créez un projet d’application console.</span><span class="sxs-lookup"><span data-stu-id="f9581-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="f9581-129">L’application console sera notre application de client OData.</span><span class="sxs-lookup"><span data-stu-id="f9581-129">The console application will be our OData client application.</span></span> <span data-ttu-id="f9581-130">(Vous pouvez également ajouter le projet à la même solution que le service).</span><span class="sxs-lookup"><span data-stu-id="f9581-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="f9581-131">Les étapes restantes font référence le projet de console.</span><span class="sxs-lookup"><span data-stu-id="f9581-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="f9581-132">Dans l’Explorateur de solutions, cliquez sur **références** et sélectionnez **ajouter une référence de Service**.</span><span class="sxs-lookup"><span data-stu-id="f9581-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="f9581-133">Dans le **ajouter une référence de Service** boîte de dialogue, tapez l’adresse du service OData :</span><span class="sxs-lookup"><span data-stu-id="f9581-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="f9581-134">où *port* est le numéro de port.</span><span class="sxs-lookup"><span data-stu-id="f9581-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="f9581-135">Pour **Namespace**, tapez « ProductService ».</span><span class="sxs-lookup"><span data-stu-id="f9581-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="f9581-136">Cette option définit l’espace de noms de la classe proxy.</span><span class="sxs-lookup"><span data-stu-id="f9581-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="f9581-137">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9581-137">Click **Go**.</span></span> <span data-ttu-id="f9581-138">Visual Studio lit le document de métadonnées OData pour découvrir les entités dans le service.</span><span class="sxs-lookup"><span data-stu-id="f9581-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="f9581-139">Cliquez sur **OK** pour ajouter la classe proxy à votre projet.</span><span class="sxs-lookup"><span data-stu-id="f9581-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="f9581-140">Créez une Instance de la classe de Proxy de Service</span><span class="sxs-lookup"><span data-stu-id="f9581-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="f9581-141">À l’intérieur de votre `Main` (méthode), créez une nouvelle instance de la classe proxy, comme suit :</span><span class="sxs-lookup"><span data-stu-id="f9581-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="f9581-142">Là encore, utilisez le numéro de port réel où votre service est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="f9581-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="f9581-143">Lorsque vous déployez votre service, vous allez utiliser l’URI du service live.</span><span class="sxs-lookup"><span data-stu-id="f9581-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="f9581-144">Vous n’avez pas besoin de mettre à jour le serveur proxy.</span><span class="sxs-lookup"><span data-stu-id="f9581-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="f9581-145">Le code suivant ajoute un gestionnaire d’événements qui imprime les URI de demande dans la fenêtre de console.</span><span class="sxs-lookup"><span data-stu-id="f9581-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="f9581-146">Cette étape n’est pas obligatoire, mais il est intéressant de voir les URI pour chaque requête.</span><span class="sxs-lookup"><span data-stu-id="f9581-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="f9581-147">Le Service de requête</span><span class="sxs-lookup"><span data-stu-id="f9581-147">Query the Service</span></span>

<span data-ttu-id="f9581-148">Le code suivant obtient la liste des produits à partir du service OData.</span><span class="sxs-lookup"><span data-stu-id="f9581-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="f9581-149">Notez que vous n’avez pas besoin d’écrire du code pour envoyer la demande HTTP ou d’analyser la réponse.</span><span class="sxs-lookup"><span data-stu-id="f9581-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="f9581-150">La classe proxy effectue cela automatiquement lorsque vous énumérez les `Container.Products` collection dans le **foreach** boucle.</span><span class="sxs-lookup"><span data-stu-id="f9581-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="f9581-151">Lorsque vous exécutez l’application, la sortie doit ressembler à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f9581-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="f9581-152">Pour obtenir une entité par ID, utilisez un `where` clause.</span><span class="sxs-lookup"><span data-stu-id="f9581-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="f9581-153">Pour le reste de cette rubrique, je n’affiche pas l’ensemble du `Main` fonctionne, que le code nécessaire pour appeler le service.</span><span class="sxs-lookup"><span data-stu-id="f9581-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="f9581-154">Appliquer les Options de requête</span><span class="sxs-lookup"><span data-stu-id="f9581-154">Apply Query Options</span></span>

<span data-ttu-id="f9581-155">OData définit [options de requête](../supporting-odata-query-options.md) qui peut être utilisé pour filtrer, trier, données de la page et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="f9581-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="f9581-156">Dans le proxy de service, vous pouvez appliquer ces options à l’aide de diverses expressions LINQ.</span><span class="sxs-lookup"><span data-stu-id="f9581-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="f9581-157">Dans cette section, je vous montrerai courts exemples.</span><span class="sxs-lookup"><span data-stu-id="f9581-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="f9581-158">Pour plus d’informations, consultez la rubrique [considérations de LINQ (WCF Data Services)](https://msdn.microsoft.com/en-us/library/ee622463.aspx) sur MSDN.</span><span class="sxs-lookup"><span data-stu-id="f9581-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/en-us/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="f9581-159">Filtrage ($filter)</span><span class="sxs-lookup"><span data-stu-id="f9581-159">Filtering ($filter)</span></span>

<span data-ttu-id="f9581-160">Pour filtrer, utilisez un `where` clause.</span><span class="sxs-lookup"><span data-stu-id="f9581-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="f9581-161">L’exemple suivant filtre par catégorie de produit.</span><span class="sxs-lookup"><span data-stu-id="f9581-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="f9581-162">Ce code correspond à la requête OData.</span><span class="sxs-lookup"><span data-stu-id="f9581-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="f9581-163">Notez que le proxy convertit le `where` clause dans un OData `$filter` expression.</span><span class="sxs-lookup"><span data-stu-id="f9581-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="f9581-164">Tri ($orderby)</span><span class="sxs-lookup"><span data-stu-id="f9581-164">Sorting ($orderby)</span></span>

<span data-ttu-id="f9581-165">Pour trier, utilisez un `orderby` clause.</span><span class="sxs-lookup"><span data-stu-id="f9581-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="f9581-166">L’exemple suivant trie par prix, du plus élevé au plus bas.</span><span class="sxs-lookup"><span data-stu-id="f9581-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="f9581-167">Voici la requête OData correspondante.</span><span class="sxs-lookup"><span data-stu-id="f9581-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="f9581-168">La pagination côté client ($skip et $top)</span><span class="sxs-lookup"><span data-stu-id="f9581-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="f9581-169">Pour les jeux d’entités volumineux, le client souhaite peut limiter le nombre de résultats.</span><span class="sxs-lookup"><span data-stu-id="f9581-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="f9581-170">Par exemple, un client peut afficher des entrées de 10 à la fois.</span><span class="sxs-lookup"><span data-stu-id="f9581-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="f9581-171">Il s’agit *la pagination côté client*.</span><span class="sxs-lookup"><span data-stu-id="f9581-171">This is called *client-side paging*.</span></span> <span data-ttu-id="f9581-172">(Il existe également [la pagination côté serveur](../supporting-odata-query-options.md#server-paging), où le serveur limite le nombre de résultats.) Pour effectuer la pagination côté client, utilisez LINQ **ignorer** et **prendre** méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9581-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="f9581-173">L’exemple suivant ignore les 40 premiers résultats et prend les 10 suivants.</span><span class="sxs-lookup"><span data-stu-id="f9581-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="f9581-174">Voici la requête OData correspondante :</span><span class="sxs-lookup"><span data-stu-id="f9581-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="f9581-175">Select ($select) et développer ($expand)</span><span class="sxs-lookup"><span data-stu-id="f9581-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="f9581-176">Pour inclure les entités associées, utilisez le `DataServiceQuery<t>.Expand` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f9581-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="f9581-177">Par exemple, pour inclure la `Supplier` pour chaque `Product`:</span><span class="sxs-lookup"><span data-stu-id="f9581-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="f9581-178">Voici la requête OData correspondante :</span><span class="sxs-lookup"><span data-stu-id="f9581-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="f9581-179">Pour modifier la forme de la réponse, utilisez LINQ **sélectionnez** clause.</span><span class="sxs-lookup"><span data-stu-id="f9581-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="f9581-180">L’exemple suivant obtient simplement le nom de chaque produit, avec aucune autre propriété.</span><span class="sxs-lookup"><span data-stu-id="f9581-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="f9581-181">Voici la requête OData correspondante :</span><span class="sxs-lookup"><span data-stu-id="f9581-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="f9581-182">Une clause select peut inclure des entités associées.</span><span class="sxs-lookup"><span data-stu-id="f9581-182">A select clause can include related entities.</span></span> <span data-ttu-id="f9581-183">Dans ce cas, n’appelez pas **Expand**; le proxy inclut automatiquement l’expansion dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="f9581-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="f9581-184">L’exemple suivant obtient le nom et le fournisseur de chaque produit.</span><span class="sxs-lookup"><span data-stu-id="f9581-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="f9581-185">Voici la requête OData correspondante.</span><span class="sxs-lookup"><span data-stu-id="f9581-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="f9581-186">Notez qu’il contient le **$expand** option.</span><span class="sxs-lookup"><span data-stu-id="f9581-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="f9581-187">Pour plus d’informations sur $select et $développez, consultez [à l’aide de $select, $expand et $value dans l’API Web 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="f9581-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="f9581-188">Ajouter une nouvelle entité</span><span class="sxs-lookup"><span data-stu-id="f9581-188">Add a New Entity</span></span>

<span data-ttu-id="f9581-189">Pour ajouter une nouvelle entité à un jeu d’entités, appelez `AddToEntitySet`, où *EntitySet* est le nom du jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="f9581-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="f9581-190">Par exemple, `AddToProducts` ajoute un nouveau `Product` à la `Products` jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="f9581-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="f9581-191">Lorsque vous générez le proxy, WCF Data Services crée automatiquement ces fortement typée **AddTo** méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9581-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="f9581-192">Pour ajouter un lien entre deux entités, utilisez le **AddLink** et **SetLink** méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9581-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="f9581-193">Le code suivant ajoute un nouveau fournisseur et un nouveau produit et crée ensuite des liens entre eux.</span><span class="sxs-lookup"><span data-stu-id="f9581-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="f9581-194">Utilisez **AddLink** lorsque la propriété de navigation est une collection.</span><span class="sxs-lookup"><span data-stu-id="f9581-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="f9581-195">Dans cet exemple, nous ajoutons un produit à la `Products` collection sur le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="f9581-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="f9581-196">Utilisez **SetLink** lorsque la propriété de navigation est une entité unique.</span><span class="sxs-lookup"><span data-stu-id="f9581-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="f9581-197">Dans cet exemple, nous définissons le `Supplier` propriété sur le produit.</span><span class="sxs-lookup"><span data-stu-id="f9581-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="f9581-198">Mettre à jour / correctifs</span><span class="sxs-lookup"><span data-stu-id="f9581-198">Update / Patch</span></span>

<span data-ttu-id="f9581-199">Pour mettre à jour une entité, appelez le **UpdateObject** (méthode).</span><span class="sxs-lookup"><span data-stu-id="f9581-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="f9581-200">La mise à jour est effectuée lorsque vous appelez **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="f9581-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="f9581-201">Par défaut, WCF envoie une demande HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="f9581-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="f9581-202">Le **PatchOnUpdate** option indique à WCF pour envoyer un HTTP PATCH à la place.</span><span class="sxs-lookup"><span data-stu-id="f9581-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="f9581-203">Pourquoi des correctifs et la fusion ?</span><span class="sxs-lookup"><span data-stu-id="f9581-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="f9581-204">La spécification HTTP 1.1 d’origine ([RCF 2616](http://tools.ietf.org/html/rfc2616)) n’a pas défini de n’importe quelle méthode HTTP avec la sémantique de « mise à jour partielle ».</span><span class="sxs-lookup"><span data-stu-id="f9581-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="f9581-205">Pour prendre en charge les mises à jour partielles, la spécification OData définie par la méthode de fusion.</span><span class="sxs-lookup"><span data-stu-id="f9581-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="f9581-206">En 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) défini par la méthode PATCH pour les mises à jour partielles.</span><span class="sxs-lookup"><span data-stu-id="f9581-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="f9581-207">Vous pouvez lire la partie de l’historique dans ce [billet de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) sur le Blog de WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="f9581-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="f9581-208">Aujourd'hui, un correctif logiciel est préféré sur la fusion.</span><span class="sxs-lookup"><span data-stu-id="f9581-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="f9581-209">Le contrôleur OData créé par la génération de modèles automatique Web API prend en charge les deux méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9581-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="f9581-210">Si vous souhaitez remplacer l’entité toute entière (sémantique PUT), spécifiez la **ReplaceOnUpdate** option.</span><span class="sxs-lookup"><span data-stu-id="f9581-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="f9581-211">Cela provoque une WCF envoyer une demande HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f9581-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="f9581-212">Supprimer une entité</span><span class="sxs-lookup"><span data-stu-id="f9581-212">Delete an Entity</span></span>

<span data-ttu-id="f9581-213">Pour supprimer une entité, appelez **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="f9581-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="f9581-214">Appeler une Action OData</span><span class="sxs-lookup"><span data-stu-id="f9581-214">Invoke an OData Action</span></span>

<span data-ttu-id="f9581-215">Dans OData, [actions](odata-actions.md) permettent d’ajouter des comportements côté serveur qui ne sont pas définies facilement comme des opérations CRUD sur les entités.</span><span class="sxs-lookup"><span data-stu-id="f9581-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="f9581-216">Bien que le document de métadonnées OData décrit les actions, la classe proxy ne crée pas de méthodes fortement typées pour eux.</span><span class="sxs-lookup"><span data-stu-id="f9581-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="f9581-217">Vous pouvez toujours appeler une action OData à l’aide de l’objet générique **Execute** (méthode).</span><span class="sxs-lookup"><span data-stu-id="f9581-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="f9581-218">Toutefois, vous devez connaître les types de données des paramètres et la valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="f9581-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="f9581-219">Par exemple, le `RateProduct` action prend le paramètre nommé « Évaluation » de type `Int32` et retourne un `double`.</span><span class="sxs-lookup"><span data-stu-id="f9581-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="f9581-220">Le code suivant montre comment appeler cette action.</span><span class="sxs-lookup"><span data-stu-id="f9581-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="f9581-221">Pour plus d’informations, consultez[appeler les opérations de Service et les Actions](https://msdn.microsoft.com/en-us/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="f9581-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/en-us/library/hh230677.aspx).</span></span>

<span data-ttu-id="f9581-222">Une option consiste à étendre le **conteneur** classe afin de fournir une méthode fortement typée qui appelle l’action :</span><span class="sxs-lookup"><span data-stu-id="f9581-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
