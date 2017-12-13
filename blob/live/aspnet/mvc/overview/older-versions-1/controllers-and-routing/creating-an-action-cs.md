---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: "Création d’une Action (c#) | Documents Microsoft"
author: microsoft
description: "Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC. En savoir plus sur la configuration requise pour une méthode d’action."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b751dc7e34951be33e7c27a3429c383a3e1e1c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-c"></a><span data-ttu-id="7bdb3-104">Création d’une Action (c#)</span><span class="sxs-lookup"><span data-stu-id="7bdb3-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="7bdb3-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7bdb3-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7bdb3-106">Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="7bdb3-107">En savoir plus sur la configuration requise pour une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="7bdb3-108">L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="7bdb3-109">Vous en savoir plus sur la configuration requise d’une méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="7bdb3-110">Vous apprenez également à empêcher une méthode d’être exposé en tant qu’action.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="7bdb3-111">Ajout d’une Action à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="7bdb3-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="7bdb3-112">Vous ajoutez une nouvelle action à un contrôleur en ajoutant une nouvelle méthode au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="7bdb3-113">Par exemple, le contrôleur dans la liste 1 contient une action nommée Index() et qu’une action nommée SayHello().</span><span class="sxs-lookup"><span data-stu-id="7bdb3-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="7bdb3-114">Les deux méthodes sont exposées en tant qu’actions.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="7bdb3-115">**La liste 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="7bdb3-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="7bdb3-116">Pour pouvoir être exposés à l’univers en tant qu’action, une méthode doit remplir certaines conditions :</span><span class="sxs-lookup"><span data-stu-id="7bdb3-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="7bdb3-117">La méthode doit être publique.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-117">The method must be public.</span></span>
- <span data-ttu-id="7bdb3-118">La méthode ne peut pas être une méthode statique.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="7bdb3-119">La méthode ne peut pas être une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="7bdb3-120">La méthode ne peut pas être un constructeur, getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="7bdb3-121">La méthode ne peut pas avoir de types génériques ouverts.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="7bdb3-122">La méthode n’est pas une méthode de la classe de base du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="7bdb3-123">La méthode ne peut pas contenir **ref** ou **hors** paramètres.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="7bdb3-124">Notez qu’il n’existe aucune restriction sur le type de retour d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="7bdb3-125">Une action de contrôleur peut retourner une chaîne, une valeur DateTime, une instance de la classe Random, ou void.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="7bdb3-126">L’infrastructure ASP.NET MVC convertit tout type de retour n’est pas un résultat d’action dans une chaîne et restituer la chaîne dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="7bdb3-127">Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée comme une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="7bdb3-128">Soyez prudent ici.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-128">Be careful here.</span></span> <span data-ttu-id="7bdb3-129">Une action de contrôleur peut être appelée par toute personne connectée à Internet.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="7bdb3-130">Ne créez pas le cas, par exemple, une action de contrôleur DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="7bdb3-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="7bdb3-131">Empêche une méthode publique à partir de l’appelé</span><span class="sxs-lookup"><span data-stu-id="7bdb3-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="7bdb3-132">Si vous devez créer une méthode publique dans une classe de contrôleur et que vous ne souhaitez pas exposer la méthode comme une action de contrôleur puis vous pouvez empêcher la méthode appelée à l’aide de l’attribut [NonAction].</span><span class="sxs-lookup"><span data-stu-id="7bdb3-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="7bdb3-133">Par exemple, le contrôleur dans la liste 2 contient une méthode publique nommée CompanySecrets() est décorée avec l’attribut [NonAction].</span><span class="sxs-lookup"><span data-stu-id="7bdb3-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="7bdb3-134">**Listing 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="7bdb3-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="7bdb3-135">Si vous tentez d’appeler l’action du contrôleur CompanySecrets() en tapant /Work/CompanySecrets dans la barre d’adresses de votre navigateur, vous allez récupérer le message d’erreur dans la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="7bdb3-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="7bdb3-136">[![Appel d’une méthode NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7bdb3-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="7bdb3-137">**Figure 01**: appeler une méthode NonAction ([cliquez pour afficher l’image en taille réelle](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7bdb3-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7bdb3-138">[Précédent](creating-a-controller-cs.md)
[Suivant](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7bdb3-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
