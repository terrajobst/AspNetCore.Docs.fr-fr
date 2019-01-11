---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Afficher les données éléments et les détails | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio Community 2017 pour le Web
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207432"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="6a078-103">Afficher les éléments de données et les détails</span><span class="sxs-lookup"><span data-stu-id="6a078-103">Display data items and details</span></span>
====================
<span data-ttu-id="6a078-104">par [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="6a078-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="6a078-105">Cette série de didacticiels vous enseigne les principes fondamentaux de la création d’une application Web Forms ASP.NET avec ASP.NET 4.7 et Microsoft Visual Studio Community 2017 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="6a078-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="6a078-106">Dans ce didacticiel, vous allez apprendre à afficher les éléments de données et les détails des éléments de données avec ASP.NET Web Forms et Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="6a078-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="6a078-107">Ce didacticiel s’appuie sur le didacticiel précédent de « L’interface utilisateur et Navigation » dans le cadre de la série de didacticiels Wingtip Toys Store.</span><span class="sxs-lookup"><span data-stu-id="6a078-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="6a078-108">Dans le didacticiel terminé, les produits sur le *ProductsList.aspx* page et les détails d’un produit sur le *ProductDetails.aspx* page sont affichés.</span><span class="sxs-lookup"><span data-stu-id="6a078-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="6a078-109">Vous découvrez</span><span class="sxs-lookup"><span data-stu-id="6a078-109">What you learn</span></span>

- <span data-ttu-id="6a078-110">Ajoutez un contrôle de données pour afficher des produits de base de données.</span><span class="sxs-lookup"><span data-stu-id="6a078-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="6a078-111">Se connecter à un contrôle de données vers les données sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="6a078-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="6a078-112">Ajoutez un contrôle de données pour afficher les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="6a078-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="6a078-113">Analyser une valeur de chaîne de requête et l’utiliser pour filtrer les données de la base de données récupérée.</span><span class="sxs-lookup"><span data-stu-id="6a078-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="6a078-114">Les fonctionnalités présentées dans ce didacticiel incluent la liaison de modèle et les fournisseurs de valeurs.</span><span class="sxs-lookup"><span data-stu-id="6a078-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="6a078-115">Ajouter un contrôle de données pour afficher des produits</span><span class="sxs-lookup"><span data-stu-id="6a078-115">Add a data control to display products</span></span>
 
<span data-ttu-id="6a078-116">Vous avez quelques options pour lier des données à un contrôle serveur.</span><span class="sxs-lookup"><span data-stu-id="6a078-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="6a078-117">Les plus courantes sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a078-117">The most common include:</span></span>

 * <span data-ttu-id="6a078-118">Ajout d’un contrôle de source de données</span><span class="sxs-lookup"><span data-stu-id="6a078-118">Adding a data source control</span></span>
 * <span data-ttu-id="6a078-119">Ajout de code manuellement</span><span class="sxs-lookup"><span data-stu-id="6a078-119">Adding code by hand</span></span>
 * <span data-ttu-id="6a078-120">Implémentation de liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="6a078-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="6a078-121">Utiliser un contrôle de source de données pour lier des données</span><span class="sxs-lookup"><span data-stu-id="6a078-121">Use a data source control to bind data</span></span>

<span data-ttu-id="6a078-122">Ajout d’un contrôle de source de données lie le contrôle de source de données au contrôle qui affiche les données.</span><span class="sxs-lookup"><span data-stu-id="6a078-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="6a078-123">Avec cette approche, vous pouvez de façon déclarative, plutôt que par programmation, connectez les contrôles côté serveur aux sources de données.</span><span class="sxs-lookup"><span data-stu-id="6a078-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="6a078-124">Code manuellement pour lier des données</span><span class="sxs-lookup"><span data-stu-id="6a078-124">Code by hand to bind data</span></span>

<span data-ttu-id="6a078-125">En codant manuellement implique :</span><span class="sxs-lookup"><span data-stu-id="6a078-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="6a078-126">Lecture d’une valeur</span><span class="sxs-lookup"><span data-stu-id="6a078-126">Reading a value</span></span>
2. <span data-ttu-id="6a078-127">Vérifier si elle est null</span><span class="sxs-lookup"><span data-stu-id="6a078-127">Checking if it's null</span></span>
3. <span data-ttu-id="6a078-128">Convertir en un type approprié</span><span class="sxs-lookup"><span data-stu-id="6a078-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="6a078-129">Vérification de la réussite de la conversion</span><span class="sxs-lookup"><span data-stu-id="6a078-129">Checking conversion success</span></span>
5. <span data-ttu-id="6a078-130">Création d’une requête avec la valeur convertie</span><span class="sxs-lookup"><span data-stu-id="6a078-130">Making a query with the converted value</span></span> 

<span data-ttu-id="6a078-131">Avec cette approche, vous avez un contrôle total sur votre logique d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="6a078-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="6a078-132">Utilisez la liaison de modèle pour lier des données</span><span class="sxs-lookup"><span data-stu-id="6a078-132">Use model binding to bind data</span></span>

<span data-ttu-id="6a078-133">Avec la liaison de modèle, vous liez des résultats avec beaucoup moins de code et il vous donne la possibilité de réutiliser les fonctionnalités dans votre application.</span><span class="sxs-lookup"><span data-stu-id="6a078-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="6a078-134">Il simplifie l’utilisation avec la logique d’accès aux données orientés code tout en fournissant une infrastructure riche, la liaison de données.</span><span class="sxs-lookup"><span data-stu-id="6a078-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="6a078-135">Afficher les produits</span><span class="sxs-lookup"><span data-stu-id="6a078-135">Display products</span></span>

<span data-ttu-id="6a078-136">Dans ce didacticiel, vous utilisez la liaison de modèle pour lier des données.</span><span class="sxs-lookup"><span data-stu-id="6a078-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="6a078-137">Pour configurer un contrôle de données pour utiliser la liaison de modèle pour sélectionner les données, vous définissez le contrôle `SelectMethod` propriété à une méthode dans le code de la page.</span><span class="sxs-lookup"><span data-stu-id="6a078-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="6a078-138">Le contrôle de données appelle la méthode au moment opportun dans le cycle de vie de page et lie automatiquement les données retournées.</span><span class="sxs-lookup"><span data-stu-id="6a078-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="6a078-139">Il est inutile d’appeler explicitement la `DataBind` (méthode).</span><span class="sxs-lookup"><span data-stu-id="6a078-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="6a078-140">Utilisez les étapes suivantes, vous modifiez *ProductList.aspx* balisage pour afficher les produits.</span><span class="sxs-lookup"><span data-stu-id="6a078-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="6a078-141">Dans **l’Explorateur de solutions**, ouvrez *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="6a078-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="6a078-142">Remplacez le balisage existant par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="6a078-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="6a078-143">Le balisage précédent utilise un **ListView** contrôle nommé `productList` pour afficher les produits.</span><span class="sxs-lookup"><span data-stu-id="6a078-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="6a078-144">Avec les styles et modèles, vous définissez la **ListView** contrôle affiche les données.</span><span class="sxs-lookup"><span data-stu-id="6a078-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="6a078-145">Il est utile pour les données dans une structure à répétition.</span><span class="sxs-lookup"><span data-stu-id="6a078-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="6a078-146">Bien que cela **ListView** exemple affiche simplement la base de données, vous pouvez également, sans code, permettre aux utilisateurs à modifier, insérer et supprimer des données et de trier et paginer des données.</span><span class="sxs-lookup"><span data-stu-id="6a078-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="6a078-147">Lorsque vous définissez la `ItemType` propriété dans le **ListView** contrôler, l’expression de liaison de données `Item` est disponible et le contrôle devienne fortement typé.</span><span class="sxs-lookup"><span data-stu-id="6a078-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="6a078-148">Comme mentionné dans le didacticiel précédent, vous pouvez sélectionner les détails de l’objet élément avec IntelliSense, telles que la spécification du `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="6a078-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Afficher les données des éléments et des détails - IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="6a078-150">Avec la liaison de modèle, vous spécifiez un `SelectMethod` valeur (`GetProducts`).</span><span class="sxs-lookup"><span data-stu-id="6a078-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="6a078-151">C’est la méthode que vous ajoutez du code derrière pour afficher les produits à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="6a078-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="6a078-152">Ajoutez du code pour afficher les produits</span><span class="sxs-lookup"><span data-stu-id="6a078-152">Add code to display products</span></span>

<span data-ttu-id="6a078-153">Dans cette étape, vous ajoutez le code pour remplir le **ListView** contrôle avec les données de produit de base de données.</span><span class="sxs-lookup"><span data-stu-id="6a078-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="6a078-154">Le code prend en charge montrant tous les produits et les produits de catégorie individuelle.</span><span class="sxs-lookup"><span data-stu-id="6a078-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="6a078-155">Dans **l’Explorateur de solutions**, avec le bouton droit *ProductList.aspx* , puis sélectionnez **afficher le Code**.</span><span class="sxs-lookup"><span data-stu-id="6a078-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="6a078-156">Remplacez le code existant dans le *ProductList.aspx.cs* fichier avec ce :</span><span class="sxs-lookup"><span data-stu-id="6a078-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="6a078-157">Ce code montre le `GetProducts` méthode qui le **ListView** du contrôle `ItemType` références de propriété dans *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="6a078-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="6a078-158">Pour limiter les résultats à une catégorie spécifique de la base de données, le code définit le `categoryId` valeur à partir de la chaîne de requête transmise à *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="6a078-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="6a078-159">Le `QueryStringAttribute` classe dans le `System.Web.ModelBinding` espace de noms est utilisée pour récupérer la variable de chaîne de requête `id`de valeur.</span><span class="sxs-lookup"><span data-stu-id="6a078-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="6a078-160">Cela indique à la liaison de modèle à, au moment de l’exécution, lier une valeur de chaîne de requête à la `categoryId` paramètre.</span><span class="sxs-lookup"><span data-stu-id="6a078-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="6a078-161">Lorsqu’une catégorie valide (`categoryId`) est passée, les résultats sont limités aux produits de base de données de cette catégorie.</span><span class="sxs-lookup"><span data-stu-id="6a078-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="6a078-162">Par exemple, si le *ProductsList.aspx* s’agit-il d’URL de la page :</span><span class="sxs-lookup"><span data-stu-id="6a078-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="6a078-163">La page affiche uniquement les produits où le `categoryId` est égal à `1`.</span><span class="sxs-lookup"><span data-stu-id="6a078-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="6a078-164">Tous les produits sont affichés si aucune chaîne de requête n’est passée.</span><span class="sxs-lookup"><span data-stu-id="6a078-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="6a078-165">Les sources de valeur pour ces méthodes sont appelées *valeur fournisseurs* (tel que `QueryString`), et les attributs de paramètre qui indiquent le fournisseur de valeur à utiliser sont appelés *les attributs de fournisseur de valeur* () comme `id`).</span><span class="sxs-lookup"><span data-stu-id="6a078-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="6a078-166">ASP.NET inclut les fournisseurs de valeurs et les attributs de tous les classique Web Forms application utilisateur sources d’entrée.</span><span class="sxs-lookup"><span data-stu-id="6a078-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="6a078-167">Citons notamment la chaîne de requête, les cookies, valeurs de formulaire, contrôles, état d’affichage, l’état de session et les propriétés de profil.</span><span class="sxs-lookup"><span data-stu-id="6a078-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="6a078-168">Vous pouvez également écrire des fournisseurs de valeurs personnalisés.</span><span class="sxs-lookup"><span data-stu-id="6a078-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="6a078-169">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="6a078-169">Run the application</span></span>

<span data-ttu-id="6a078-170">Exécutez maintenant l’application pour afficher tous les produits ou des produits d’une catégorie.</span><span class="sxs-lookup"><span data-stu-id="6a078-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="6a078-171">Dans Visual Studio, appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6a078-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="6a078-172">Le navigateur s’ouvre et affiche le *Default.aspx* page.</span><span class="sxs-lookup"><span data-stu-id="6a078-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="6a078-173">Dans le menu de catégorie de produit, sélectionnez **voitures**.</span><span class="sxs-lookup"><span data-stu-id="6a078-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="6a078-174">Le *ProductList.aspx* page apparaît, affichant uniquement les produits à partir de la **voitures** catégorie.</span><span class="sxs-lookup"><span data-stu-id="6a078-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="6a078-175">Plus loin dans ce didacticiel, vous afficher les détails du produit.</span><span class="sxs-lookup"><span data-stu-id="6a078-175">Later in this tutorial, you display product details.</span></span>

    ![Afficher les données des éléments et des détails - voitures](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="6a078-177">Sélectionnez **produits** dans le menu supérieur.</span><span class="sxs-lookup"><span data-stu-id="6a078-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="6a078-178">Le *ProductList.aspx* page affiche maintenant tous les produits.</span><span class="sxs-lookup"><span data-stu-id="6a078-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="6a078-180">Fermez le navigateur et revenir à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a078-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="6a078-181">Ajouter un contrôle de données pour afficher les détails du produit</span><span class="sxs-lookup"><span data-stu-id="6a078-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="6a078-182">Modifier le *ProductDetails.aspx* balisage que vous avez ajouté dans le didacticiel précédent pour afficher des informations de produit spécifique :</span><span class="sxs-lookup"><span data-stu-id="6a078-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="6a078-183">Dans **l’Explorateur de solutions**, ouvrez *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="6a078-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="6a078-184">Remplacez le balisage existant par ce balisage :</span><span class="sxs-lookup"><span data-stu-id="6a078-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="6a078-185">Ce balisage utilise un **FormView** contrôle pour afficher les détails de produit spécifique.</span><span class="sxs-lookup"><span data-stu-id="6a078-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="6a078-186">Il utilise des méthodes telles que celles utilisées pour afficher des données dans *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="6a078-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="6a078-187">Le **FormView** contrôle est utilisé pour afficher un seul enregistrement à la fois à partir d’une source de données.</span><span class="sxs-lookup"><span data-stu-id="6a078-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="6a078-188">Lorsque vous utilisez le **FormView** contrôle, vous créez des modèles pour afficher et modifier des valeurs liées aux données.</span><span class="sxs-lookup"><span data-stu-id="6a078-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="6a078-189">Ces modèles contiennent des contrôles, les expressions de liaison, et mise en forme qui définissent apparence et du fonctionnement du formulaire.</span><span class="sxs-lookup"><span data-stu-id="6a078-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="6a078-190">Connexion le balisage précédent à la base de données nécessite du code supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="6a078-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="6a078-191">Dans **l’Explorateur de solutions**, avec le bouton droit *ProductDetails.aspx* , puis sélectionnez **afficher le Code**.</span><span class="sxs-lookup"><span data-stu-id="6a078-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="6a078-192">Le *ProductDetails.aspx.cs* fichier s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6a078-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="6a078-193">Remplacez le code existant par ceci :</span><span class="sxs-lookup"><span data-stu-id="6a078-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="6a078-194">Ce code vérifie pour un «`productID`« valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="6a078-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="6a078-195">Si une valeur valide est trouvée, le produit correspondant s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6a078-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="6a078-196">Si la chaîne de requête n’est pas trouvée, ou sa valeur n’est pas valide, aucun produit ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6a078-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="6a078-197">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="6a078-197">Run the application</span></span>

<span data-ttu-id="6a078-198">Vous pouvez maintenant exécuter l’application pour afficher les détails de produit spécifique en fonction de l’ID de produit.</span><span class="sxs-lookup"><span data-stu-id="6a078-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="6a078-199">Dans Visual Studio, appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6a078-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="6a078-200">Le navigateur ouvre *Default.aspx*.</span><span class="sxs-lookup"><span data-stu-id="6a078-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="6a078-201">Dans le menu de la catégorie, sélectionnez **bateaux**.</span><span class="sxs-lookup"><span data-stu-id="6a078-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="6a078-202">Le *ProductList.aspx* page s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6a078-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="6a078-203">Sélectionnez **papier bateau**.</span><span class="sxs-lookup"><span data-stu-id="6a078-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="6a078-204">Le *ProductDetails.aspx* page s’affiche.</span><span class="sxs-lookup"><span data-stu-id="6a078-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Afficher les données des éléments et des détails - produits](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="6a078-206">Dans le didacticiel suivant, vous ajoutez un panier d’achat à l’application Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="6a078-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a078-207">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6a078-207">Additional resources</span></span>

[<span data-ttu-id="6a078-208">Récupération et affichage des données avec la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="6a078-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="6a078-209">[Précédent](ui_and_navigation.md)
> [Suivant](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="6a078-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
