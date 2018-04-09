---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Partie 3 : Mise en page et Menu catégorie | Documents Microsoft'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks. Partie 3 traite de l’ajout de mise en page et un menu de la catégorie.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="3c240-104">Partie 3 : Mise en page et Menu catégorie</span><span class="sxs-lookup"><span data-stu-id="3c240-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="3c240-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3c240-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3c240-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plateforme .NET.</span><span class="sxs-lookup"><span data-stu-id="3c240-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3c240-107">Il illustre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="3c240-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3c240-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="3c240-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3c240-109">Partie 3 traite de l’ajout de mise en page et un menu de la catégorie.</span><span class="sxs-lookup"><span data-stu-id="3c240-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="3c240-110">Ajout d’une mise en page et un Menu de la catégorie</span><span class="sxs-lookup"><span data-stu-id="3c240-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="3c240-111">Dans notre page maître du site, nous allons ajouter un élément div de la colonne de gauche qui contiendra notre menu de catégorie de produit.</span><span class="sxs-lookup"><span data-stu-id="3c240-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="3c240-112">Notez que l’alignement souhaité et autres mises en forme seront fournies par la classe CSS que nous avons ajouté à notre fichier Style.css.</span><span class="sxs-lookup"><span data-stu-id="3c240-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="3c240-113">Le menu de catégorie de produit sera être créé de façon dynamique lors de l’exécution en interrogeant la base de données de Commerce pour les catégories de produits et créer les éléments de menu et existants correspondant est liée.</span><span class="sxs-lookup"><span data-stu-id="3c240-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="3c240-114">Pour ce faire, nous allons utiliser deux des ASP. Contrôles de données puissants de NET.</span><span class="sxs-lookup"><span data-stu-id="3c240-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="3c240-115">Le contrôle de la « Source de données d’entité » et le contrôle de la « Liste ».</span><span class="sxs-lookup"><span data-stu-id="3c240-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="3c240-116">Nous allons en « Mode de conception » et les programmes d’assistance permet de configurer les contrôles.</span><span class="sxs-lookup"><span data-stu-id="3c240-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="3c240-117">Nous allons définir la propriété ID du contrôle EntityDataSource pour EDS\_catégorie\_Menu et cliquez sur « Configurer la Source de données ».</span><span class="sxs-lookup"><span data-stu-id="3c240-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="3c240-118">Sélectionnez la connexion CommerceEntities qui a été créé pour nous lorsque nous avons créé le modèle de Source de données entité pour notre base de données de Commerce, puis cliquez sur « Suivant ».</span><span class="sxs-lookup"><span data-stu-id="3c240-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="3c240-119">Sélectionnez le nom de jeu d’entités de « Catégories » et laisser le reste des options comme valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="3c240-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="3c240-120">Cliquez sur « Terminer ».</span><span class="sxs-lookup"><span data-stu-id="3c240-120">Click "Finish".</span></span>

<span data-ttu-id="3c240-121">Maintenant nous allons définir la propriété ID de l’instance du contrôle ListView qui nous les avons placées sur notre page relative à ListView\_ProductsMenu et activer ses d’assistance.</span><span class="sxs-lookup"><span data-stu-id="3c240-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="3c240-122">Bien que nous pouvons utiliser les options de contrôle à mettre en forme l’affichage d’élément de données et mise en forme, la création de notre menu ne nécessite que balisage simple pour sera entrer le code dans la vue de source.</span><span class="sxs-lookup"><span data-stu-id="3c240-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="3c240-123">Notez l’instruction « Eval » : &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="3c240-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="3c240-124">La syntaxe ASP.NET &lt;% # %&gt; est une convention de raccourci qui indique à l’exécution pour exécuter tout ce qui est contenu dans et les résultats « dans la ligne ».</span><span class="sxs-lookup"><span data-stu-id="3c240-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="3c240-125">L’instruction Eval("CategoryName") fait en sorte que, pour l’entrée actuelle dans la collection liée d’éléments de données, d’extraire la valeur des noms d’élément de modèle d’entité « CatagoryName ».</span><span class="sxs-lookup"><span data-stu-id="3c240-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="3c240-126">Il s’agit d’une syntaxe concise pour une fonctionnalité très puissante.</span><span class="sxs-lookup"><span data-stu-id="3c240-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="3c240-127">Permet d’exécuter l’application maintenant.</span><span class="sxs-lookup"><span data-stu-id="3c240-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="3c240-128">Notez que notre menu de catégorie de produit s’affiche désormais lorsque nous pointez sur un des éléments de menu de catégorie nous pouvons voir les points de lien d’élément menu à une page, il n’est pas encore implémenter nommé ProductsList.aspx et que nous avons créé un argument de chaîne de requête dynamique qui contient le  id de catégorie.</span><span class="sxs-lookup"><span data-stu-id="3c240-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c240-129">[Précédent](tailspin-spyworks-part-2.md)
> [Suivant](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="3c240-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
