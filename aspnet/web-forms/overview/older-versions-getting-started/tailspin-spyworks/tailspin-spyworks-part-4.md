---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Partie 4 : Liste des produits | Documents Microsoft'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks. Partie 4 couvre les produits de la liste avec le contrat de GridView...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881062"
---
<a name="part-4-listing-products"></a><span data-ttu-id="41bc8-104">Partie 4 : Liste de produits</span><span class="sxs-lookup"><span data-stu-id="41bc8-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="41bc8-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="41bc8-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="41bc8-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plateforme .NET.</span><span class="sxs-lookup"><span data-stu-id="41bc8-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="41bc8-107">Il illustre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="41bc8-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="41bc8-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="41bc8-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="41bc8-109">Partie 4 couvre la liste des produits avec le contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="41bc8-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="41bc8-110">Liste des produits avec le contrôle GridView</span><span class="sxs-lookup"><span data-stu-id="41bc8-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="41bc8-111">Nous allons commencer l’implémentation de notre page ProductsList.aspx en « Cliquant avec le bouton droit sur » sur notre solution, puis en sélectionnant « Ajouter » et « Nouvel élément ».</span><span class="sxs-lookup"><span data-stu-id="41bc8-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="41bc8-112">Choisissez « Formulaire à l’aide de Page maître Web » et entrez un nom de page de ProductsList.aspx ».</span><span class="sxs-lookup"><span data-stu-id="41bc8-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="41bc8-113">Cliquez sur « Ajouter ».</span><span class="sxs-lookup"><span data-stu-id="41bc8-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="41bc8-114">Ensuite, choisissez le dossier « Styles » où nous avons placé la page Site.Master et sélectionnez-le dans la fenêtre « Contenu du dossier ».</span><span class="sxs-lookup"><span data-stu-id="41bc8-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="41bc8-115">Cliquez sur « Ok » pour créer la page.</span><span class="sxs-lookup"><span data-stu-id="41bc8-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="41bc8-116">Notre base de données est remplie avec les données de produit, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="41bc8-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="41bc8-117">Après la création de notre page Nous utiliserons à nouveau une Source de données d’entité pour accéder aux données de ce produit, mais dans ce cas, nous devons sélectionner les entités Product et nous avons besoin limiter les éléments qui sont retournés à ceux de la catégorie sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="41bc8-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="41bc8-118">Pour cela nous indiquerons EntityDataSource pour générer automatiquement la clause WHERE et spécifions le WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="41bc8-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="41bc8-119">Rappelez-vous que lorsque nous avons créé les éléments de Menu dans notre « Menu de catégorie de produit » nous construites dynamiquement le lien en ajoutant le CatagoryID à la chaîne de requête pour chaque lien.</span><span class="sxs-lookup"><span data-stu-id="41bc8-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="41bc8-120">Nous indique à la Source de données d’entité pour dériver le paramètre d’emplacement à partir de ce paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="41bc8-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="41bc8-121">Ensuite, nous allons configurer le contrôle ListView pour afficher une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="41bc8-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="41bc8-122">Pour créer une expérience d’achat optimale, que nous allons compact plusieurs fonctionnalités concises dans chaque produit affiché dans notre ListVew.</span><span class="sxs-lookup"><span data-stu-id="41bc8-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="41bc8-123">Le nom de produit doit être un lien vers l’affichage des détails du produit.</span><span class="sxs-lookup"><span data-stu-id="41bc8-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="41bc8-124">Le prix du produit s’affiche.</span><span class="sxs-lookup"><span data-stu-id="41bc8-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="41bc8-125">Une image du produit s’affiche et nous allons sélectionner dynamiquement l’image à partir d’un répertoire d’images de catalogue dans notre application.</span><span class="sxs-lookup"><span data-stu-id="41bc8-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="41bc8-126">Il inclut un lien pour ajouter immédiatement le produit spécifique dans le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="41bc8-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="41bc8-127">Voici le balisage de notre instance du contrôle ListView.</span><span class="sxs-lookup"><span data-stu-id="41bc8-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="41bc8-128">Nous créons dynamiquement plusieurs liens pour chaque produit affiché.</span><span class="sxs-lookup"><span data-stu-id="41bc8-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="41bc8-129">En outre, avant de tester le propre nouvelle page, nous devons créer la structure de répertoire pour le produit des images du catalogue comme suit.</span><span class="sxs-lookup"><span data-stu-id="41bc8-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="41bc8-130">Une fois que les images de nos produits sont accessibles, nous pouvons tester notre page de liste de produits.</span><span class="sxs-lookup"><span data-stu-id="41bc8-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="41bc8-131">À partir de la page d’accueil du site, cliquez sur un des liens de liste de catégorie.</span><span class="sxs-lookup"><span data-stu-id="41bc8-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="41bc8-132">Nous devons à présent mettre en œuvre de la page ProductDetials.apsx et la fonctionnalité AddToCart.</span><span class="sxs-lookup"><span data-stu-id="41bc8-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="41bc8-133">Utilisez le fichier -&gt;nouveau pour créer un nom de page ProductDetails.aspx à l’aide de la Page maître de site comme nous l’avons fait précédemment.</span><span class="sxs-lookup"><span data-stu-id="41bc8-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="41bc8-134">Nous allons utiliser à nouveau d’un contrôle EntityDataSource pour accéder à l’enregistrement de produit spécifique dans la base de données, et nous allons utiliser un contrôle ASP.NET FormView pour afficher les données de produit comme suit.</span><span class="sxs-lookup"><span data-stu-id="41bc8-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="41bc8-135">Ne vous inquiétez pas si la mise en forme semble un peu amusantes à vous.</span><span class="sxs-lookup"><span data-stu-id="41bc8-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="41bc8-136">Le balisage ci-dessus laisse place dans la disposition de l’affichage pour quelques fonctionnalités que nous allons implémenter plus tard.</span><span class="sxs-lookup"><span data-stu-id="41bc8-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="41bc8-137">Le panier d’achat représente une logique plus complexe dans notre application.</span><span class="sxs-lookup"><span data-stu-id="41bc8-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="41bc8-138">Pour commencer, utilisez le fichier -&gt;nouveau pour créer une page nommée MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="41bc8-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="41bc8-139">Notez que nous choisissons pas le nom ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="41bc8-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="41bc8-140">Notre base de données contient une table nommée « ShoppingCart ».</span><span class="sxs-lookup"><span data-stu-id="41bc8-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="41bc8-141">Lorsque nous avons généré un Entity Data Model, une classe a été créée pour chaque table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="41bc8-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="41bc8-142">Par conséquent, l’Entity Data Model généré une classe d’entité nommée « ShoppingCart ».</span><span class="sxs-lookup"><span data-stu-id="41bc8-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="41bc8-143">Nous pouvons modifier le modèle afin que nous pourrions utiliser ce nom pour notre implémentation de panier d’achat ou de l’étendre à vos besoins, mais nous inclut à la place pour simplement sélectionnez un nom qui permet d’éviter le conflit.</span><span class="sxs-lookup"><span data-stu-id="41bc8-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="41bc8-144">Il est également important de noter que nous allons créer un panier d’achat simple et l’incorporation de la logique de panier d’achat avec l’affichage du panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="41bc8-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="41bc8-145">Nous pouvons également choisir d’implémenter notre panier d’achat dans une couche métier complètement distincte.</span><span class="sxs-lookup"><span data-stu-id="41bc8-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="41bc8-146">[Précédent](tailspin-spyworks-part-3.md)
> [Suivant](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="41bc8-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
