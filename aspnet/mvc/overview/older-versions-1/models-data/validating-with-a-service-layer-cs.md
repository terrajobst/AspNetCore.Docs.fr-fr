---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validation avec une couche de Service (c#) | Microsoft Docs
author: StephenWalther
description: Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct. Dans ce didacticiel, Stephen Walther explique comment vous...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 69ff78949589017d12a791231e38b400b49f2917
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827448"
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="19d77-104">Validation avec une couche de Service (c#)</span><span class="sxs-lookup"><span data-stu-id="19d77-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="19d77-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="19d77-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="19d77-106">Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct.</span><span class="sxs-lookup"><span data-stu-id="19d77-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="19d77-107">Dans ce didacticiel, Stephen Walther explique comment vous pouvez conserver une sharp séparation des préoccupations en isolant votre couche de service à partir de votre couche de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="19d77-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="19d77-108">L’objectif de ce didacticiel est de décrire une méthode pour la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="19d77-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="19d77-109">Dans ce didacticiel, vous allez apprendre à déplacer votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct.</span><span class="sxs-lookup"><span data-stu-id="19d77-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="19d77-110">Séparation des préoccupations</span><span class="sxs-lookup"><span data-stu-id="19d77-110">Separating Concerns</span></span>

<span data-ttu-id="19d77-111">Lorsque vous générez une application ASP.NET MVC, vous ne devez pas placer votre logique de base de données à l’intérieur de vos actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="19d77-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="19d77-112">Mélange de votre logique de base de données et de contrôleur rend votre application plus difficile à gérer au fil du temps.</span><span class="sxs-lookup"><span data-stu-id="19d77-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="19d77-113">Il est recommandé que vous placez toute votre logique de base de données dans une couche de référentiels distincts.</span><span class="sxs-lookup"><span data-stu-id="19d77-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="19d77-114">Par exemple, le Listing 1 contient un référentiel simple nommé le ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="19d77-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="19d77-115">Le référentiel de produit contient tout le code d’accès de données pour l’application.</span><span class="sxs-lookup"><span data-stu-id="19d77-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="19d77-116">La liste inclut également l’interface IProductRepository qui implémente le référentiel de produit.</span><span class="sxs-lookup"><span data-stu-id="19d77-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="19d77-117">**Liste 1--Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="19d77-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="19d77-118">Le contrôleur dans le Listing 2 utilise la couche de référentiels dans ses Index() et Create() actions.</span><span class="sxs-lookup"><span data-stu-id="19d77-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="19d77-119">Notez que ce contrôleur ne contient pas de logique de base de données.</span><span class="sxs-lookup"><span data-stu-id="19d77-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="19d77-120">Création d’une couche de référentiel vous permet de maintenir une séparation claire des préoccupations.</span><span class="sxs-lookup"><span data-stu-id="19d77-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="19d77-121">Les contrôleurs sont chargés pour la logique de contrôle de flux application et le référentiel est responsable de la logique d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="19d77-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="19d77-122">**Listing 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="19d77-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="19d77-123">Création d’une couche de Service</span><span class="sxs-lookup"><span data-stu-id="19d77-123">Creating a Service Layer</span></span>

<span data-ttu-id="19d77-124">Par conséquent, logique de contrôle de flux application appartienne à un contrôleur et appartienne de logique d’accès aux données dans un référentiel.</span><span class="sxs-lookup"><span data-stu-id="19d77-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="19d77-125">Dans ce cas, où placer votre logique de validation ?</span><span class="sxs-lookup"><span data-stu-id="19d77-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="19d77-126">Une option consiste à placer votre logique de validation dans un *couche de service*.</span><span class="sxs-lookup"><span data-stu-id="19d77-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="19d77-127">Une couche de service est une couche supplémentaire dans une application ASP.NET MVC qui sert d’intermédiaire pour la communication entre un contrôleur et une couche de référentiels.</span><span class="sxs-lookup"><span data-stu-id="19d77-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="19d77-128">La couche de service contient la logique métier.</span><span class="sxs-lookup"><span data-stu-id="19d77-128">The service layer contains business logic.</span></span> <span data-ttu-id="19d77-129">En particulier, il contient la logique de validation.</span><span class="sxs-lookup"><span data-stu-id="19d77-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="19d77-130">Par exemple, la couche de service de produit dans le Listing 3 a une méthode CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="19d77-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="19d77-131">La méthode CreateProduct() appelle la méthode ValidateProduct() pour valider un nouveau produit avant de passer le produit dans le référentiel de produit.</span><span class="sxs-lookup"><span data-stu-id="19d77-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="19d77-132">**Liste 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="19d77-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="19d77-133">Le contrôleur de produit a été mis à jour sur la liste de 4 à utiliser la couche de service au lieu de la couche de référentiels.</span><span class="sxs-lookup"><span data-stu-id="19d77-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="19d77-134">La couche de contrôleur communique avec la couche de service.</span><span class="sxs-lookup"><span data-stu-id="19d77-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="19d77-135">La couche de service communique avec la couche de référentiels.</span><span class="sxs-lookup"><span data-stu-id="19d77-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="19d77-136">Chaque couche a une responsabilité distincte.</span><span class="sxs-lookup"><span data-stu-id="19d77-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="19d77-137">**Liste 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="19d77-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="19d77-138">Notez que le service de produit est créé dans le constructeur de contrôleur du produit.</span><span class="sxs-lookup"><span data-stu-id="19d77-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="19d77-139">Lorsque le service de produit est créé, le dictionnaire d’états de modèle est passé au service.</span><span class="sxs-lookup"><span data-stu-id="19d77-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="19d77-140">Le service de produit utilise l’état du modèle pour transmettre des messages d’erreur de validation sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="19d77-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="19d77-141">Découplage de la couche de Service</span><span class="sxs-lookup"><span data-stu-id="19d77-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="19d77-142">Nous avons a échoué isoler le contrôleur et les couches de service dans un aspect.</span><span class="sxs-lookup"><span data-stu-id="19d77-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="19d77-143">Le contrôleur et les couches de service communiquent via le modèle d’état.</span><span class="sxs-lookup"><span data-stu-id="19d77-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="19d77-144">En d’autres termes, la couche de service a une dépendance sur une fonctionnalité particulière de l’infrastructure ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="19d77-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="19d77-145">Nous souhaitons isoler la couche de service à partir de notre couche contrôleur autant que possible.</span><span class="sxs-lookup"><span data-stu-id="19d77-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="19d77-146">En théorie, nous devrions être en mesure d’utiliser la couche de service avec n’importe quel type d’application et non seulement une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="19d77-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="19d77-147">Par exemple, à l’avenir, nous pourrions créer un WPF frontal pour notre application.</span><span class="sxs-lookup"><span data-stu-id="19d77-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="19d77-148">Nous devons trouver un moyen de supprimer la dépendance sur ASP.NET MVC état du modèle à partir de la couche de notre service.</span><span class="sxs-lookup"><span data-stu-id="19d77-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="19d77-149">Dans la liste 5, la couche de service a été mise à jour afin qu’il n’utilise plus l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="19d77-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="19d77-150">Au lieu de cela, il utilise n’importe quelle classe qui implémente l’interface IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="19d77-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="19d77-151">**Liste 5 - Models\ProductService.cs (découplé)**</span><span class="sxs-lookup"><span data-stu-id="19d77-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="19d77-152">L’interface IValidationDictionary est définie dans la liste 6.</span><span class="sxs-lookup"><span data-stu-id="19d77-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="19d77-153">Cette interface simple possède une méthode unique et une seule propriété.</span><span class="sxs-lookup"><span data-stu-id="19d77-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="19d77-154">**Liste 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="19d77-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="19d77-155">La classe dans la liste 7, nommé la classe ModelStateWrapper, implémente l’interface IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="19d77-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="19d77-156">Vous pouvez instancier la classe ModelStateWrapper en passant un dictionnaire d’états de modèle au constructeur.</span><span class="sxs-lookup"><span data-stu-id="19d77-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="19d77-157">**Liste 7 - Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="19d77-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="19d77-158">Enfin, le contrôleur mis à jour dans la liste 8 utilise le ModelStateWrapper lors de la création de la couche de service dans son constructeur.</span><span class="sxs-lookup"><span data-stu-id="19d77-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="19d77-159">**Liste 8 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="19d77-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="19d77-160">À l’aide de la IValidationDictionary interface et la classe ModelStateWrapper nous permet d’isoler totalement notre couche de service à partir de la couche de notre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="19d77-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="19d77-161">La couche de service n’est plus dépendante de l’état du modèle.</span><span class="sxs-lookup"><span data-stu-id="19d77-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="19d77-162">Vous pouvez passer n’importe quelle classe qui implémente l’interface IValidationDictionary à la couche de service.</span><span class="sxs-lookup"><span data-stu-id="19d77-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="19d77-163">Par exemple, une application WPF peut implémenter l’interface IValidationDictionary avec une classe de collection simple.</span><span class="sxs-lookup"><span data-stu-id="19d77-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="19d77-164">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="19d77-164">Summary</span></span>

<span data-ttu-id="19d77-165">L’objectif de ce didacticiel a été pour présenter une approche de la validation dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="19d77-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="19d77-166">Dans ce didacticiel, vous avez appris à déplacer toute votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct.</span><span class="sxs-lookup"><span data-stu-id="19d77-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="19d77-167">Vous avez également appris comment isoler votre couche de service à partir de votre couche de contrôleur en créant une classe ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="19d77-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="19d77-168">[Précédent](validating-with-the-idataerrorinfo-interface-cs.md)
> [Suivant](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="19d77-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
