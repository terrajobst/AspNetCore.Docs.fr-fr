---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Afficher les données éléments et les détails | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous montrera les principes fondamentaux de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396219"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="cc785-103">Afficher les éléments de données et les détails</span><span class="sxs-lookup"><span data-stu-id="cc785-103">Display data items and details</span></span>
====================
<span data-ttu-id="cc785-104">par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="cc785-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="cc785-105">Cette série de didacticiels vous enseigne les principes fondamentaux de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cc785-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="cc785-106">Dans ce didacticiel, vous allez apprendre à afficher les éléments de données et les détails des éléments de données avec ASP.NET Web Forms et Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="cc785-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="cc785-107">Ce didacticiel s’appuie sur le didacticiel précédent de « L’interface utilisateur et Navigation » dans le cadre de la série de didacticiels Wingtip Toys Store.</span><span class="sxs-lookup"><span data-stu-id="cc785-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="cc785-108">À l’issue de ce didacticiel, vous verrez les produits sur le *ProductsList.aspx* page et les détails d’un produit sur le *ProductDetails.aspx* page.</span><span class="sxs-lookup"><span data-stu-id="cc785-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="cc785-109">Vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="cc785-109">You'll learn how to:</span></span>

- <span data-ttu-id="cc785-110">Ajouter un contrôle de données pour afficher les produits à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="cc785-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="cc785-111">Connecter un contrôle de données pour les données sélectionnées</span><span class="sxs-lookup"><span data-stu-id="cc785-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="cc785-112">Ajouter un contrôle de données pour afficher les détails du produit à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="cc785-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="cc785-113">Récupérer une valeur dans la chaîne de requête et utilisez cette valeur pour limiter les données sont récupérées à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="cc785-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="cc785-114">Fonctionnalités introduites dans ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="cc785-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="cc785-115">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="cc785-115">Model binding</span></span>
- <span data-ttu-id="cc785-116">Fournisseurs de valeurs</span><span class="sxs-lookup"><span data-stu-id="cc785-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="cc785-117">Ajouter un contrôle de données</span><span class="sxs-lookup"><span data-stu-id="cc785-117">Add a data control</span></span>

<span data-ttu-id="cc785-118">Vous pouvez utiliser plusieurs options pour lier des données à un contrôle serveur.</span><span class="sxs-lookup"><span data-stu-id="cc785-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="cc785-119">Les plus courantes sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="cc785-119">The most common include:</span></span>

 * <span data-ttu-id="cc785-120">Ajout d’un contrôle de source de données</span><span class="sxs-lookup"><span data-stu-id="cc785-120">Adding a data source control</span></span>
 * <span data-ttu-id="cc785-121">Ajout de code manuellement</span><span class="sxs-lookup"><span data-stu-id="cc785-121">Adding code by hand</span></span>
 * <span data-ttu-id="cc785-122">À l’aide de la liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="cc785-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="cc785-123">Utiliser un contrôle de source de données pour lier des données</span><span class="sxs-lookup"><span data-stu-id="cc785-123">Use a data source control to bind data</span></span>

<span data-ttu-id="cc785-124">Ajout d’un contrôle de source de données vous permet de lier le contrôle de source de données au contrôle qui affiche les données.</span><span class="sxs-lookup"><span data-stu-id="cc785-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="cc785-125">Avec cette approche, vous pouvez de façon déclarative, plutôt que par programmation, connectez les contrôles côté serveur aux sources de données.</span><span class="sxs-lookup"><span data-stu-id="cc785-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="cc785-126">Code manuellement pour lier des données</span><span class="sxs-lookup"><span data-stu-id="cc785-126">Code by hand to bind data</span></span>

<span data-ttu-id="cc785-127">En codant manuellement implique :</span><span class="sxs-lookup"><span data-stu-id="cc785-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="cc785-128">Lecture d’une valeur</span><span class="sxs-lookup"><span data-stu-id="cc785-128">Reading a value</span></span>
2. <span data-ttu-id="cc785-129">Vérifier si elle est null</span><span class="sxs-lookup"><span data-stu-id="cc785-129">Checking if it's null</span></span>
3. <span data-ttu-id="cc785-130">Convertir en un type approprié</span><span class="sxs-lookup"><span data-stu-id="cc785-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="cc785-131">Vérification de la réussite de la conversion</span><span class="sxs-lookup"><span data-stu-id="cc785-131">Checking conversion success</span></span>
5. <span data-ttu-id="cc785-132">À l’aide de la valeur dans la requête</span><span class="sxs-lookup"><span data-stu-id="cc785-132">Using the value in the query</span></span> 

<span data-ttu-id="cc785-133">Cette approche vous permet d’avoir un contrôle total sur votre logique d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="cc785-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="cc785-134">Utilisez la liaison de modèle pour lier des données</span><span class="sxs-lookup"><span data-stu-id="cc785-134">Use model binding to bind data</span></span>

<span data-ttu-id="cc785-135">Liaison de modèle vous permet de lier les résultats avec beaucoup moins de code et vous donne la possibilité de réutiliser les fonctionnalités dans votre application.</span><span class="sxs-lookup"><span data-stu-id="cc785-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="cc785-136">Il simplifie l’utilisation avec la logique d’accès aux données orientés code tout en fournissant une infrastructure riche, la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="cc785-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="cc785-137">Afficher les produits</span><span class="sxs-lookup"><span data-stu-id="cc785-137">Display products</span></span>

<span data-ttu-id="cc785-138">Dans ce didacticiel, vous allez utiliser la liaison de modèle pour lier des données.</span><span class="sxs-lookup"><span data-stu-id="cc785-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="cc785-139">Pour configurer un contrôle de données pour utiliser la liaison de modèle pour sélectionner les données, vous définissez le contrôle `SelectMethod` propriété à un nom de méthode dans le code de la page.</span><span class="sxs-lookup"><span data-stu-id="cc785-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="cc785-140">Le contrôle de données appelle la méthode au moment opportun dans le cycle de vie de page et lie automatiquement les données retournées.</span><span class="sxs-lookup"><span data-stu-id="cc785-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="cc785-141">Il est inutile d’appeler explicitement la `DataBind` (méthode).</span><span class="sxs-lookup"><span data-stu-id="cc785-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="cc785-142">Dans **l’Explorateur de solutions**, ouvrez *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="cc785-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="cc785-143">Remplacez le balisage existant par ce balisage :</span><span class="sxs-lookup"><span data-stu-id="cc785-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="cc785-144">Ce code utilise un **ListView** contrôle nommé `productList` pour afficher les produits.</span><span class="sxs-lookup"><span data-stu-id="cc785-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="cc785-145">Avec les styles et modèles, vous définissez la **ListView** contrôle affiche les données.</span><span class="sxs-lookup"><span data-stu-id="cc785-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="cc785-146">Il est utile pour les données dans une structure à répétition.</span><span class="sxs-lookup"><span data-stu-id="cc785-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="cc785-147">Bien que cela **ListView** exemple affiche simplement la base de données, vous pouvez également, sans code, permettre aux utilisateurs à modifier, insérer et supprimer des données et de trier et paginer des données.</span><span class="sxs-lookup"><span data-stu-id="cc785-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="cc785-148">En définissant le `ItemType` propriété dans le **ListView** contrôler, l’expression de liaison de données `Item` est disponible et le contrôle devienne fortement typé.</span><span class="sxs-lookup"><span data-stu-id="cc785-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="cc785-149">Comme mentionné dans le didacticiel précédent, vous pouvez sélectionner les détails de l’objet élément avec IntelliSense, telles que la spécification du `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="cc785-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Afficher les données des éléments et des détails - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="cc785-151">Vous utilisez également une liaison de modèle pour spécifier un `SelectMethod` valeur.</span><span class="sxs-lookup"><span data-stu-id="cc785-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="cc785-152">Cette valeur (`GetProducts`) correspond à la méthode que vous allez ajouter du code derrière pour afficher les produits à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="cc785-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="cc785-153">Ajoutez du code pour afficher les produits</span><span class="sxs-lookup"><span data-stu-id="cc785-153">Add code to display products</span></span>

<span data-ttu-id="cc785-154">Dans cette étape, vous ajouterez du code pour remplir le **ListView** contrôle avec les données de produit à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="cc785-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="cc785-155">Le code prend en charge montrant tous les produits et les produits de catégorie individuelle.</span><span class="sxs-lookup"><span data-stu-id="cc785-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="cc785-156">Dans **l’Explorateur de solutions**, avec le bouton droit *ProductList.aspx* , puis sélectionnez **afficher le Code**.</span><span class="sxs-lookup"><span data-stu-id="cc785-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="cc785-157">Remplacez le code existant dans le *ProductList.aspx.cs* fichier avec ce :</span><span class="sxs-lookup"><span data-stu-id="cc785-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="cc785-158">Ce code montre le `GetProducts` méthode qui le **ListView** du contrôle `ItemType` références de propriété dans le *ProductList.aspx* page.</span><span class="sxs-lookup"><span data-stu-id="cc785-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="cc785-159">Pour limiter les résultats à une catégorie spécifique de la base de données, le code définit le `categoryId` valeur à partir de la valeur de chaîne de requête passée à la *ProductList.aspx* page lorsque le *ProductList.aspx* page est cible de la navigation.</span><span class="sxs-lookup"><span data-stu-id="cc785-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="cc785-160">Le `QueryStringAttribute` classe dans le `System.Web.ModelBinding` espace de noms est utilisé pour récupérer la valeur de la variable de chaîne de requête `id`.</span><span class="sxs-lookup"><span data-stu-id="cc785-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="cc785-161">Cela indique à la liaison de modèle pour essayer de lier une valeur de la chaîne de requête à la `categoryId` paramètre en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="cc785-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="cc785-162">Lorsqu’une catégorie valide est passée comme une chaîne de requête à la page, les résultats de la requête sont limités à ces produits dans la base de données qui correspondent à la `categoryId` valeur.</span><span class="sxs-lookup"><span data-stu-id="cc785-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="cc785-163">Par exemple, si le *ProductsList.aspx* s’agit-il d’URL de la page :</span><span class="sxs-lookup"><span data-stu-id="cc785-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="cc785-164">La page affiche uniquement les produits où le `categoryId` est égal à `1`.</span><span class="sxs-lookup"><span data-stu-id="cc785-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="cc785-165">Tous les produits sont affichés si aucune chaîne de requête n’est inclus lorsque le *ProductList.aspx* page est appelée.</span><span class="sxs-lookup"><span data-stu-id="cc785-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="cc785-166">Les sources des valeurs pour ces méthodes sont appelées *valeur fournisseurs* (tel que *QueryString*), et les attributs de paramètre qui indiquent le fournisseur de valeur à utiliser sont appelés *les attributs de fournisseur de valeur* (tel que `id`).</span><span class="sxs-lookup"><span data-stu-id="cc785-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="cc785-167">ASP.NET inclut les fournisseurs de valeurs et des attributs correspondants pour toutes les sources d’entrée d’utilisateur standard dans une application Web Forms, telles que la chaîne de requête, les cookies, les valeurs de formulaire, contrôles, état d’affichage, l’état de session et les propriétés de profil.</span><span class="sxs-lookup"><span data-stu-id="cc785-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="cc785-168">Vous pouvez également écrire des fournisseurs de valeurs personnalisés.</span><span class="sxs-lookup"><span data-stu-id="cc785-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="cc785-169">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="cc785-169">Run the application</span></span>

<span data-ttu-id="cc785-170">Exécutez maintenant l’application pour afficher tous les produits ou des produits d’une catégorie.</span><span class="sxs-lookup"><span data-stu-id="cc785-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="cc785-171">Appuyez sur **F5** tandis que dans Visual Studio pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="cc785-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="cc785-172">Le navigateur s’ouvre et affiche le *Default.aspx* page.</span><span class="sxs-lookup"><span data-stu-id="cc785-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="cc785-173">Sélectionnez **voitures** dans le menu de navigation de catégorie de produit.</span><span class="sxs-lookup"><span data-stu-id="cc785-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="cc785-174">Le *ProductList.aspx* page affiche uniquement **voitures** produits de la catégorie.</span><span class="sxs-lookup"><span data-stu-id="cc785-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="cc785-175">Plus loin dans ce didacticiel, vous allez afficher les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="cc785-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Afficher les données des éléments et des détails - voitures](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="cc785-177">Sélectionnez **produits** dans le menu de navigation en haut.</span><span class="sxs-lookup"><span data-stu-id="cc785-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="cc785-178">Là encore, le *ProductList.aspx* page s’affiche, mais cette fois, il indique la liste complète des produits.</span><span class="sxs-lookup"><span data-stu-id="cc785-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="cc785-180">Fermez le navigateur et revenir à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc785-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="cc785-181">Ajouter un contrôle de données pour afficher les détails du produit</span><span class="sxs-lookup"><span data-stu-id="cc785-181">Add a data control to display product details</span></span>

<span data-ttu-id="cc785-182">Ensuite, vous allez modifier le balisage dans le *ProductDetails.aspx* page que vous avez ajouté dans le didacticiel précédent pour afficher des informations de produit spécifique.</span><span class="sxs-lookup"><span data-stu-id="cc785-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="cc785-183">Dans **l’Explorateur de solutions**, ouvrez *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="cc785-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="cc785-184">Remplacez le balisage existant par ce balisage :</span><span class="sxs-lookup"><span data-stu-id="cc785-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="cc785-185">Ce code utilise un **FormView** contrôle pour afficher les détails de produit spécifique.</span><span class="sxs-lookup"><span data-stu-id="cc785-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="cc785-186">Ce balisage utilise des méthodes, telles que les méthodes utilisées pour afficher des données dans le *ProductList.aspx* page.</span><span class="sxs-lookup"><span data-stu-id="cc785-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="cc785-187">Le **FormView** contrôle est utilisé pour afficher un seul enregistrement à la fois à partir d’une source de données.</span><span class="sxs-lookup"><span data-stu-id="cc785-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="cc785-188">Lorsque vous utilisez le **FormView** contrôle, vous créez des modèles pour afficher et modifier des valeurs liées aux données.</span><span class="sxs-lookup"><span data-stu-id="cc785-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="cc785-189">Ces modèles contiennent des contrôles, les expressions de liaison, et mise en forme qui définissent apparence et du fonctionnement du formulaire.</span><span class="sxs-lookup"><span data-stu-id="cc785-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="cc785-190">Connexion le balisage précédent à la base de données nécessite du code supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="cc785-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="cc785-191">Dans **l’Explorateur de solutions**, avec le bouton droit *ProductDetails.aspx* puis cliquez sur **afficher le Code**.</span><span class="sxs-lookup"><span data-stu-id="cc785-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="cc785-192">Le *ProductDetails.aspx.cs* fichier s’affiche.</span><span class="sxs-lookup"><span data-stu-id="cc785-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="cc785-193">Remplacez le code existant par ce code :</span><span class="sxs-lookup"><span data-stu-id="cc785-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="cc785-194">Ce code vérifie pour un «`productID`« valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="cc785-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="cc785-195">Si une valeur de chaîne de requête valide est trouvée, le produit correspondant s’affiche.</span><span class="sxs-lookup"><span data-stu-id="cc785-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="cc785-196">Si la chaîne de requête n’est pas trouvée, ou sa valeur n’est pas valide, aucun produit ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="cc785-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="cc785-197">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="cc785-197">Run the application</span></span>

<span data-ttu-id="cc785-198">Vous pouvez maintenant exécuter l’application pour voir un produit individuel affiché en fonction des ID de produit.</span><span class="sxs-lookup"><span data-stu-id="cc785-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="cc785-199">Appuyez sur **F5** tandis que dans Visual Studio pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="cc785-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="cc785-200">Le navigateur s’ouvre et affiche le *Default.aspx* page.</span><span class="sxs-lookup"><span data-stu-id="cc785-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="cc785-201">Sélectionnez **bateaux** dans le menu de navigation de catégorie.</span><span class="sxs-lookup"><span data-stu-id="cc785-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="cc785-202">Le *ProductList.aspx* page s’affiche.</span><span class="sxs-lookup"><span data-stu-id="cc785-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="cc785-203">Sélectionnez **livre bateau** à partir de la liste des produits.</span><span class="sxs-lookup"><span data-stu-id="cc785-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="cc785-204">Le *ProductDetails.aspx* page s’affiche.</span><span class="sxs-lookup"><span data-stu-id="cc785-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="cc785-206">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="cc785-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cc785-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cc785-207">Additional resources</span></span>

[<span data-ttu-id="cc785-208">Récupération et affichage des données avec la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="cc785-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="cc785-209">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="cc785-209">Next steps</span></span>

<span data-ttu-id="cc785-210">Dans ce didacticiel, vous avez ajouté le balisage et le code pour afficher les produits et les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="cc785-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="cc785-211">Vous avez appris à des contrôles de données fortement typées, de liaison de modèle et de fournisseurs de valeurs.</span><span class="sxs-lookup"><span data-stu-id="cc785-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="cc785-212">Dans le didacticiel suivant, vous allez ajouter un panier d’achat pour l’exemple d’application Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="cc785-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="cc785-213">[Précédent](ui_and_navigation.md)
> [Suivant](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="cc785-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
