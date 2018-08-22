---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Partie 4 : Liste des produits | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 4 couvre la liste des produits avec le contrat de GridView...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ca7eccd684473d9a1ec4a8adfd8690b291fe702f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823688"
---
<a name="part-4-listing-products"></a><span data-ttu-id="e3a82-104">Partie 4 : Liste des produits</span><span class="sxs-lookup"><span data-stu-id="e3a82-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="e3a82-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e3a82-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e3a82-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="e3a82-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e3a82-107">Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="e3a82-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e3a82-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e3a82-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e3a82-109">Partie 4 couvre la liste des produits avec le contrôle GridView.</span><span class="sxs-lookup"><span data-stu-id="e3a82-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="e3a82-110">Liste des produits avec le contrôle GridView</span><span class="sxs-lookup"><span data-stu-id="e3a82-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="e3a82-111">Nous allons commencer l’implémentation de notre page ProductsList.aspx en « Cliquant avec le bouton droit sur » sur notre solution et en sélectionnant « Ajouter » et « Nouvel élément ».</span><span class="sxs-lookup"><span data-stu-id="e3a82-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="e3a82-112">Choisissez « Formulaire à l’aide de Page maître Web » et entrez un nom de la page de ProductsList.aspx ».</span><span class="sxs-lookup"><span data-stu-id="e3a82-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="e3a82-113">Cliquez sur « Ajouter ».</span><span class="sxs-lookup"><span data-stu-id="e3a82-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="e3a82-114">Ensuite, choisissez le dossier « Styles » où nous avons placé la page Site.Master et sélectionnez-le dans la fenêtre « Contenu du dossier ».</span><span class="sxs-lookup"><span data-stu-id="e3a82-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="e3a82-115">Cliquez sur « Ok » pour créer la page.</span><span class="sxs-lookup"><span data-stu-id="e3a82-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="e3a82-116">Notre base de données est remplie avec les données de produit, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="e3a82-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="e3a82-117">Après la création de notre page Nous allons utiliser à nouveau d’une Source de données d’entité pour accéder aux données de ce produit, mais dans ce cas, nous avons besoin sélectionner les entités Product et nous avons besoin limiter les éléments qui sont retournés à ceux de la catégorie sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="e3a82-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="e3a82-118">Pour effectuer cette opération nous indiquerons contrôle EntityDataSource génération automatique de la clause WHERE, et nous indiquons le WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="e3a82-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="e3a82-119">Vous vous rappellerez que lorsque nous avons créé les éléments de Menu dans notre « Menu de catégorie de produit » nous créée dynamiquement le lien en ajoutant le CatagoryID à la chaîne de requête pour chaque lien.</span><span class="sxs-lookup"><span data-stu-id="e3a82-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="e3a82-120">Nous vous indique la Source de données d’entité de dériver le paramètre d’emplacement de ce paramètre de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="e3a82-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="e3a82-121">Ensuite, nous allons configurer le contrôle ListView pour afficher une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="e3a82-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="e3a82-122">Pour créer une expérience d’achat optimale, que nous allons compact plusieurs fonctionnalités concises dans chaque produit affiché dans notre ListVew.</span><span class="sxs-lookup"><span data-stu-id="e3a82-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="e3a82-123">Le nom du produit sera un lien vers l’affichage des détails du produit.</span><span class="sxs-lookup"><span data-stu-id="e3a82-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="e3a82-124">Le prix du produit s’affiche.</span><span class="sxs-lookup"><span data-stu-id="e3a82-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="e3a82-125">Une image du produit s’affiche et nous allons sélectionner dynamiquement l’image à partir d’un répertoire d’images de catalogue dans notre application.</span><span class="sxs-lookup"><span data-stu-id="e3a82-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="e3a82-126">Nous inclurons un lien pour ajouter immédiatement le produit spécifique pour le panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="e3a82-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="e3a82-127">Voici le balisage de notre instance de contrôle ListView.</span><span class="sxs-lookup"><span data-stu-id="e3a82-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="e3a82-128">Nous allons créer dynamiquement plusieurs liens pour chaque produit affiché.</span><span class="sxs-lookup"><span data-stu-id="e3a82-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="e3a82-129">En outre, avant de tester le propre nouvelle page Nous devons créer, la structure de répertoire pour le produit des images de catalogue comme suit.</span><span class="sxs-lookup"><span data-stu-id="e3a82-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="e3a82-130">Une fois que nos images de produit sont accessibles, nous pouvons tester notre page de liste de produits.</span><span class="sxs-lookup"><span data-stu-id="e3a82-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="e3a82-131">À partir de la page d’accueil du site, cliquez sur l’un des liens de liste de catégorie.</span><span class="sxs-lookup"><span data-stu-id="e3a82-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="e3a82-132">Nous devons à présent implémenter la page ProductDetials.apsx et la fonctionnalité AddToCart.</span><span class="sxs-lookup"><span data-stu-id="e3a82-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="e3a82-133">Utilisez fichier -&gt;nouveau pour créer un nom de page ProductDetails.aspx à l’aide de la Page maître du site comme nous l’avons fait précédemment.</span><span class="sxs-lookup"><span data-stu-id="e3a82-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="e3a82-134">Nous allons utiliser à nouveau d’un contrôle EntityDataSource pour accéder à l’enregistrement de produit spécifique dans la base de données et nous allons utiliser un contrôle FormView d’ASP.NET pour afficher les données de produit comme suit.</span><span class="sxs-lookup"><span data-stu-id="e3a82-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="e3a82-135">Ne vous inquiétez pas si la mise en forme semble un peu amusantes à vous.</span><span class="sxs-lookup"><span data-stu-id="e3a82-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="e3a82-136">Le balisage ci-dessus laisse la place dans la disposition de l’affichage pour quelques fonctionnalités, nous allons implémenter par la suite.</span><span class="sxs-lookup"><span data-stu-id="e3a82-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="e3a82-137">Le panier représentera une logique plus complexe dans notre application.</span><span class="sxs-lookup"><span data-stu-id="e3a82-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="e3a82-138">Pour commencer, utilisez fichier -&gt;nouveau pour créer une page appelée MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="e3a82-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="e3a82-139">Notez que nous ne choisissons pas le nom ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="e3a82-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="e3a82-140">Notre base de données contient une table nommée « ShoppingCart ».</span><span class="sxs-lookup"><span data-stu-id="e3a82-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="e3a82-141">Lorsque nous avons généré un Entity Data Model, une classe a été créée pour chaque table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e3a82-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="e3a82-142">Par conséquent, l’Entity Data Model généré une classe d’entité nommée « ShoppingCart ».</span><span class="sxs-lookup"><span data-stu-id="e3a82-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="e3a82-143">Nous pouvons modifier le modèle afin que nous pourrions utiliser ce nom pour notre implémentation de panier d’achat ou l’étendre pour nos besoins, mais nous choisissons d’au lieu de cela pour simplement sélectionnez un nom qui permet d’éviter le conflit.</span><span class="sxs-lookup"><span data-stu-id="e3a82-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="e3a82-144">Il est également important de noter que nous allons créer un panier d’achat simple et en incorporant la logique de panier d’achat avec l’affichage de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="e3a82-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="e3a82-145">Nous pouvons également choisir d’implémenter notre panier d’achat dans une couche métier totalement distincte.</span><span class="sxs-lookup"><span data-stu-id="e3a82-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e3a82-146">[Précédent](tailspin-spyworks-part-3.md)
> [Suivant](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="e3a82-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
