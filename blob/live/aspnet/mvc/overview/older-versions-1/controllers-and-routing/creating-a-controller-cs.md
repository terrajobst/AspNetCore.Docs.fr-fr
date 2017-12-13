---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: "Création d’un contrôleur (c#) | Documents Microsoft"
author: StephenWalther
description: "Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 9faaff1e00998ef9a77c4928a9eb36fc93ab97f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-controller-c"></a><span data-ttu-id="9429c-103">Création d’un contrôleur (c#)</span><span class="sxs-lookup"><span data-stu-id="9429c-103">Creating a Controller (C#)</span></span>
====================
<span data-ttu-id="9429c-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9429c-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9429c-105">Dans ce didacticiel, Stephen Walther montre comment vous pouvez ajouter un contrôleur à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9429c-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="9429c-106">L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer nouveau ASP.NET MVC contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="9429c-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="9429c-107">Vous apprenez à créer des contrôleurs à la fois à l’aide de l’option de menu Visual Studio ajouter un contrôleur et en créant un fichier de classe à la main.</span><span class="sxs-lookup"><span data-stu-id="9429c-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="9429c-108">À l’aide de l’ajouter l’Option de Menu de contrôleur</span><span class="sxs-lookup"><span data-stu-id="9429c-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="9429c-109">Le moyen le plus simple pour créer un nouveau contrôleur est pour le bouton droit sur le dossier contrôleurs dans la fenêtre Explorateur de solutions Visual Studio et sélectionnez le **ajouter, de contrôleur** option de menu (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="9429c-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="9429c-110">Cette option de menu ouvre le **ajouter un contrôleur** boîte de dialogue (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="9429c-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="9429c-111">[![La boîte de dialogue Nouveau projet](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9429c-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="9429c-112">**Figure 01**: ajoutez un nouveau contrôleur ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9429c-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>


<span data-ttu-id="9429c-113">[![La boîte de dialogue Nouveau projet](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9429c-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="9429c-114">**Figure 02**: boîte de dialogue Ajouter un contrôleur du ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="9429c-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>


<span data-ttu-id="9429c-115">Notez que la première partie du nom du contrôleur est mis en surbrillance dans le **ajouter un contrôleur** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="9429c-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="9429c-116">Chaque nom de contrôleur doit se terminer par le suffixe *contrôleur*.</span><span class="sxs-lookup"><span data-stu-id="9429c-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="9429c-117">Par exemple, vous pouvez créer un contrôleur nommé *ProductController* mais pas un contrôleur nommé *produit*.</span><span class="sxs-lookup"><span data-stu-id="9429c-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="9429c-118">Si vous créez un contrôleur qui manque le *contrôleur* suffixe ensuite, vous ne pourrez pas appeler le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="9429c-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="9429c-119">Ne le faites pas : j’ai perdu d’innombrables heures de ma vie après cette erreur.</span><span class="sxs-lookup"><span data-stu-id="9429c-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="9429c-120">**La liste 1 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="9429c-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="9429c-121">Vous devez toujours créer des contrôleurs dans le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="9429c-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="9429c-122">Sinon, vous allez être violer les conventions d’ASP.NET MVC et autres développeurs ont une heure plus difficile la présentation de votre application.</span><span class="sxs-lookup"><span data-stu-id="9429c-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="9429c-123">Méthodes d’Action de génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="9429c-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="9429c-124">Lorsque vous créez un contrôleur, vous pouvez générer automatiquement des méthodes d’action Create, Update et Details (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="9429c-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="9429c-125">Si vous sélectionnez cette option dans la liste 2, la classe de contrôleur est générée.</span><span class="sxs-lookup"><span data-stu-id="9429c-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="9429c-126">[![Création automatique de méthodes d’action](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9429c-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="9429c-127">**Figure 03**: création automatique des méthodes d’action ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9429c-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>


<span data-ttu-id="9429c-128">**Listing 2 - Controllers\CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="9429c-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="9429c-129">Ces méthodes générées sont les méthodes stub.</span><span class="sxs-lookup"><span data-stu-id="9429c-129">These generated methods are stub methods.</span></span> <span data-ttu-id="9429c-130">Vous devez ajouter la logique réelle de la création, mise à jour et l’affichage des détails pour un client vous-même.</span><span class="sxs-lookup"><span data-stu-id="9429c-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="9429c-131">Toutefois, les méthodes stub vous fournissent un point de départ intéressant.</span><span class="sxs-lookup"><span data-stu-id="9429c-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="9429c-132">Création d’une classe de contrôleur</span><span class="sxs-lookup"><span data-stu-id="9429c-132">Creating a Controller Class</span></span>

<span data-ttu-id="9429c-133">Le contrôleur ASP.NET MVC est simplement une classe.</span><span class="sxs-lookup"><span data-stu-id="9429c-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="9429c-134">Si vous préférez, vous pouvez ignorer la génération de modèles automatique pratique Visual Studio contrôleur et créer une classe de contrôleur manuellement.</span><span class="sxs-lookup"><span data-stu-id="9429c-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="9429c-135">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="9429c-135">Follow these steps:</span></span>

1. <span data-ttu-id="9429c-136">Cliquez sur le dossier contrôleurs, puis sélectionnez l’option de menu **ajouter, nouvel élément** et sélectionnez le **classe** modèle (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="9429c-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="9429c-137">Nommez la nouvelle classe PersonController.cs et cliquez sur le **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="9429c-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="9429c-138">Modifier le fichier résultant de la classe pour que la classe hérite de la classe de base System.Web.Mvc.Controller (voir la liste 3).</span><span class="sxs-lookup"><span data-stu-id="9429c-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="9429c-139">[![Création d’une nouvelle classe](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9429c-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="9429c-140">**Figure 04**: création d’une nouvelle classe ([cliquez pour afficher l’image en taille réelle](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="9429c-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>


<span data-ttu-id="9429c-141">**La liste 3 - Controllers\PersonController.cs**</span><span class="sxs-lookup"><span data-stu-id="9429c-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="9429c-142">Le contrôleur dans la liste 3 expose une action nommée Index() qui retourne la chaîne « Hello World ! ».</span><span class="sxs-lookup"><span data-stu-id="9429c-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="9429c-143">Vous pouvez appeler cette action de contrôleur par votre application en cours d’exécution et si vous demandez une URL comme suit :</span><span class="sxs-lookup"><span data-stu-id="9429c-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE] 
> 
> <span data-ttu-id="9429c-144">Le serveur de développement ASP.NET utilise un numéro de port aléatoire (par exemple, 40071).</span><span class="sxs-lookup"><span data-stu-id="9429c-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="9429c-145">Lorsque vous entrez une URL pour appeler un contrôleur, vous devez fournir le numéro de port de droite.</span><span class="sxs-lookup"><span data-stu-id="9429c-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="9429c-146">Vous pouvez déterminer le numéro de port en plaçant votre souris sur l’icône pour le serveur de développement ASP.NET dans la zone de Notification de Windows (en bas à droite de l’écran).</span><span class="sxs-lookup"><span data-stu-id="9429c-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9429c-147">[Précédent](adding-dynamic-content-to-a-cached-page-cs.md)
[Suivant](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9429c-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
