---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Création d’un contrôleur (VB) | Documents Microsoft
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="6e902-103">Création d’un contrôleur (VB)</span><span class="sxs-lookup"><span data-stu-id="6e902-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="6e902-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="6e902-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="6e902-105">Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6e902-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="6e902-106">L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer nouveau ASP.NET MVC contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6e902-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="6e902-107">Vous apprenez à créer des contrôleurs à la fois à l’aide de l’option de menu Visual Studio ajouter un contrôleur et en créant un fichier de classe à la main.</span><span class="sxs-lookup"><span data-stu-id="6e902-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="6e902-108">À l’aide de l’ajouter l’Option de Menu de contrôleur</span><span class="sxs-lookup"><span data-stu-id="6e902-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="6e902-109">Le moyen le plus simple pour créer un nouveau contrôleur est pour le bouton droit sur le dossier contrôleurs dans la fenêtre Explorateur de solutions Visual Studio et sélectionnez le **ajouter, de contrôleur** option de menu (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="6e902-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="6e902-110">Cette option de menu ouvre le **ajouter un contrôleur** boîte de dialogue (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="6e902-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="6e902-111">[![La boîte de dialogue Nouveau projet](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6e902-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="6e902-112">**Figure 01**: ajoutez un nouveau contrôleur ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6e902-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="6e902-113">[![La boîte de dialogue Nouveau projet](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="6e902-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="6e902-114">**Figure 02**: boîte de dialogue Ajouter un contrôleur du ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="6e902-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="6e902-115">Notez que la première partie du nom du contrôleur est mis en surbrillance dans le **ajouter un contrôleur** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="6e902-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="6e902-116">Chaque nom de contrôleur doit se terminer par le suffixe *contrôleur*.</span><span class="sxs-lookup"><span data-stu-id="6e902-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="6e902-117">Par exemple, vous pouvez créer un contrôleur nommé *ProductController* mais pas un contrôleur nommé *produit*.</span><span class="sxs-lookup"><span data-stu-id="6e902-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="6e902-118">Si vous créez un contrôleur qui manque le *contrôleur* suffixe ensuite, vous ne pourrez pas appeler le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e902-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="6e902-119">Ne le faites pas : j’ai perdu d’innombrables heures de ma vie après cette erreur.</span><span class="sxs-lookup"><span data-stu-id="6e902-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="6e902-120">**La liste 1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="6e902-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="6e902-121">Vous devez toujours créer des contrôleurs dans le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6e902-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="6e902-122">Sinon, vous allez être violer les conventions d’ASP.NET MVC et autres développeurs ont une heure plus difficile la présentation de votre application.</span><span class="sxs-lookup"><span data-stu-id="6e902-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="6e902-123">Méthodes d’Action de génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="6e902-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="6e902-124">Lorsque vous créez un contrôleur, vous pouvez générer automatiquement des méthodes d’action Create, Update et Details (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="6e902-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="6e902-125">Si vous sélectionnez cette option dans la liste 2, la classe de contrôleur est générée.</span><span class="sxs-lookup"><span data-stu-id="6e902-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="6e902-126">[![Création automatique de méthodes d’action](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="6e902-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="6e902-127">**Figure 03**: création automatique des méthodes d’action ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6e902-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="6e902-128">**Listing 2 - Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="6e902-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="6e902-129">Ces méthodes générées sont les méthodes stub.</span><span class="sxs-lookup"><span data-stu-id="6e902-129">These generated methods are stub methods.</span></span> <span data-ttu-id="6e902-130">Vous devez ajouter la logique réelle de la création, mise à jour et l’affichage des détails pour un client vous-même.</span><span class="sxs-lookup"><span data-stu-id="6e902-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="6e902-131">Toutefois, les méthodes stub vous fournissent un point de départ intéressant.</span><span class="sxs-lookup"><span data-stu-id="6e902-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="6e902-132">Création d’une classe de contrôleur</span><span class="sxs-lookup"><span data-stu-id="6e902-132">Creating a Controller Class</span></span>

<span data-ttu-id="6e902-133">Le contrôleur ASP.NET MVC est simplement une classe.</span><span class="sxs-lookup"><span data-stu-id="6e902-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="6e902-134">Si vous préférez, vous pouvez ignorer la génération de modèles automatique pratique Visual Studio contrôleur et créer une classe de contrôleur manuellement.</span><span class="sxs-lookup"><span data-stu-id="6e902-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="6e902-135">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e902-135">Follow these steps:</span></span>

1. <span data-ttu-id="6e902-136">Cliquez sur le dossier contrôleurs, puis sélectionnez l’option de menu **ajouter, nouvel élément** et sélectionnez le **classe** modèle (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="6e902-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="6e902-137">Nommez la nouvelle classe PersonController.vb et cliquez sur le **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="6e902-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="6e902-138">Modifier le fichier résultant de la classe pour que la classe hérite de la classe de base System.Web.Mvc.Controller (voir la liste 3).</span><span class="sxs-lookup"><span data-stu-id="6e902-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="6e902-139">[![Création d’une nouvelle classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="6e902-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="6e902-140">**Figure 04**: création d’une nouvelle classe ([cliquez pour afficher l’image en taille réelle](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="6e902-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="6e902-141">**Listing 3 - Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="6e902-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="6e902-142">Le contrôleur dans la liste 3 expose une action nommée Index() qui retourne la chaîne « Hello World ! ».</span><span class="sxs-lookup"><span data-stu-id="6e902-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="6e902-143">Vous pouvez appeler cette action de contrôleur par votre application en cours d’exécution et si vous demandez une URL comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e902-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="6e902-144">Le serveur de développement ASP.NET utilise un numéro de port aléatoire (par exemple, 40071).</span><span class="sxs-lookup"><span data-stu-id="6e902-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="6e902-145">Lorsque vous entrez une URL pour appeler un contrôleur, vous devez fournir le numéro de port de droite.</span><span class="sxs-lookup"><span data-stu-id="6e902-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="6e902-146">Vous pouvez déterminer le numéro de port en plaçant votre souris sur l’icône pour le serveur de développement ASP.NET dans la zone de Notification de Windows (en bas à droite de l’écran).</span><span class="sxs-lookup"><span data-stu-id="6e902-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="6e902-147">[Précédent](adding-dynamic-content-to-a-cached-page-vb.md)
> [Suivant](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6e902-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
