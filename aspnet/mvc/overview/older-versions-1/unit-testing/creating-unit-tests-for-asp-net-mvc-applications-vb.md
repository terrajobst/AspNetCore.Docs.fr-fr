---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Création de Tests unitaires pour les Applications ASP.NET MVC (VB) | Documents Microsoft
author: StephenWalther
description: Découvrez comment créer des tests unitaires pour les actions de contrôleur. Dans ce didacticiel, Stephen Walther montre comment tester si une action du contrôleur retourne une section...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869671"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a><span data-ttu-id="d2110-104">Création de Tests unitaires pour les Applications ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="d2110-104">Creating Unit Tests for ASP.NET MVC Applications (VB)</span></span>
====================
<span data-ttu-id="d2110-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d2110-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="d2110-106">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="d2110-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> <span data-ttu-id="d2110-107">Découvrez comment créer des tests unitaires pour les actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="d2110-108">Dans ce didacticiel, Stephen Walther montre comment tester si une action du contrôleur retourne une vue particulière, retourne un jeu de données particulier ou un autre type de résultat d’action.</span><span class="sxs-lookup"><span data-stu-id="d2110-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="d2110-109">L’objectif de ce didacticiel est d’illustrer le comment écrire des tests unitaires pour les contrôleurs dans votre MVC ASP.NET applications.</span><span class="sxs-lookup"><span data-stu-id="d2110-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="d2110-110">Nous expliquent comment générer trois différents types de tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="d2110-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="d2110-111">Vous apprenez à la vue retournée par une action de contrôleur de test afficher les données retournées par une action de contrôleur de test et à tester si une action de contrôleur vous redirige vers une deuxième action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="d2110-112">Création du contrôleur de Test</span><span class="sxs-lookup"><span data-stu-id="d2110-112">Creating the Controller under Test</span></span>

<span data-ttu-id="d2110-113">Commençons par créer le contrôleur que nous voulons tester.</span><span class="sxs-lookup"><span data-stu-id="d2110-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="d2110-114">Le nom du contrôleur, le `ProductController`, est contenue dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="d2110-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="d2110-115">**La liste 1 : `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="d2110-115">**Listing 1 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

<span data-ttu-id="d2110-116">Le `ProductController` contient deux méthodes d’action `Index()` et `Details()`.</span><span class="sxs-lookup"><span data-stu-id="d2110-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="d2110-117">Les deux méthodes d’action retournent une vue.</span><span class="sxs-lookup"><span data-stu-id="d2110-117">Both action methods return a view.</span></span> <span data-ttu-id="d2110-118">Notez que le `Details()` action accepte un paramètre nommé ID.</span><span class="sxs-lookup"><span data-stu-id="d2110-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="d2110-119">La vue de test retourné par un contrôleur</span><span class="sxs-lookup"><span data-stu-id="d2110-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="d2110-120">Imaginez que vous voulez tester ou non la `ProductController` retourne la vue de droite.</span><span class="sxs-lookup"><span data-stu-id="d2110-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="d2110-121">Nous voulons que quand le `ProductController.Details()` action est appelée, l’affichage des détails est retourné.</span><span class="sxs-lookup"><span data-stu-id="d2110-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="d2110-122">La classe de test dans la liste 2 contienne un test unitaire pour tester la vue retournée par le `ProductController.Details()` action.</span><span class="sxs-lookup"><span data-stu-id="d2110-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="d2110-123">**Liste 2 : `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="d2110-123">**Listing 2 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

<span data-ttu-id="d2110-124">La classe de liste 2 inclut une méthode de test nommée `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="d2110-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="d2110-125">Cette méthode contient trois lignes de code.</span><span class="sxs-lookup"><span data-stu-id="d2110-125">This method contains three lines of code.</span></span> <span data-ttu-id="d2110-126">La première ligne de code crée une nouvelle instance de la `ProductController` classe.</span><span class="sxs-lookup"><span data-stu-id="d2110-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="d2110-127">La deuxième ligne de code appelle du contrôleur `Details()` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="d2110-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="d2110-128">Enfin, la dernière ligne de contrôle de code ou non la vue retournée par le `Details()` action est le mode Détails.</span><span class="sxs-lookup"><span data-stu-id="d2110-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="d2110-129">Le `ViewResult.ViewName` propriété représente le nom de la vue retourné par un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="d2110-130">Un avertissement sur le test de cette propriété.</span><span class="sxs-lookup"><span data-stu-id="d2110-130">One big warning about testing this property.</span></span> <span data-ttu-id="d2110-131">Il existe deux façons qu’un contrôleur peut retourner une vue.</span><span class="sxs-lookup"><span data-stu-id="d2110-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="d2110-132">Un contrôleur peut retourner explicitement une vue comme suit :</span><span class="sxs-lookup"><span data-stu-id="d2110-132">A controller can explicitly return a view like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

<span data-ttu-id="d2110-133">Vous pouvez également le nom de la vue peut être déduit à partir du nom de l’action de contrôleur comme suit :</span><span class="sxs-lookup"><span data-stu-id="d2110-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

<span data-ttu-id="d2110-134">Cette action de contrôleur retourne également une vue nommée `Details`.</span><span class="sxs-lookup"><span data-stu-id="d2110-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="d2110-135">Toutefois, le nom de la vue est déduit à partir du nom d’action.</span><span class="sxs-lookup"><span data-stu-id="d2110-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="d2110-136">Si vous souhaitez tester le nom de la vue, vous devez retourner explicitement le nom de la vue à partir de l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="d2110-137">Vous pouvez exécuter le test unitaire dans la liste 2 en entrant la combinaison de touches **Ctrl + R, A** ou en cliquant sur le **exécuter tous les Tests de la Solution** bouton (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="d2110-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="d2110-138">Si le test réussit, vous verrez la fenêtre Résultats des tests dans la Figure 2.</span><span class="sxs-lookup"><span data-stu-id="d2110-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="d2110-139">[![Exécuter tous les Tests dans la Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d2110-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)</span></span>

<span data-ttu-id="d2110-140">**Figure 01**: exécuter tous les Tests de la Solution ([cliquez pour afficher l’image en taille réelle](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d2110-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))</span></span>


<span data-ttu-id="d2110-141">[![Opération réussie](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d2110-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)</span></span>

<span data-ttu-id="d2110-142">**Figure 02**: opération réussie !</span><span class="sxs-lookup"><span data-stu-id="d2110-142">**Figure 02**: Success!</span></span> <span data-ttu-id="d2110-143">([Cliquez pour afficher l’image en taille réelle](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d2110-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="d2110-144">La vue de données de test retourné par un contrôleur</span><span class="sxs-lookup"><span data-stu-id="d2110-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="d2110-145">Contrôleur MVC transmet des données à une vue à l’aide d’un élément appelé *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="d2110-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="d2110-146">Par exemple, imaginez que vous souhaitez afficher les détails d’un produit particulier quand vous appelez le `ProductController Details()` action.</span><span class="sxs-lookup"><span data-stu-id="d2110-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="d2110-147">Dans ce cas, vous pouvez créer une instance d’un `Product` classe (défini dans votre modèle) et passez l’instance à la `Details` vue en tirant parti des `View Data`.</span><span class="sxs-lookup"><span data-stu-id="d2110-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="d2110-148">Modifié `ProductController` dans la liste 3 inclut une mise à jour `Details()` action qui retourne un produit.</span><span class="sxs-lookup"><span data-stu-id="d2110-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="d2110-149">**La liste 3 : `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="d2110-149">**Listing 3 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

<span data-ttu-id="d2110-150">Tout d’abord, le `Details()` action crée une nouvelle instance de la `Product` classe qui représente un ordinateur portable.</span><span class="sxs-lookup"><span data-stu-id="d2110-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="d2110-151">Ensuite, l’instance de la `Product` classe est passée en tant que second paramètre de la `View()` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d2110-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="d2110-152">Vous pouvez écrire des tests unitaires pour vérifier si les données attendues seront contenue dans la vue données.</span><span class="sxs-lookup"><span data-stu-id="d2110-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="d2110-153">Le test unitaire dans les tests de liste 4 ou non un produit qui représente un ordinateur portable est retourné lorsque vous appelez le `ProductController Details()` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="d2110-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="d2110-154">**La liste 4 – `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="d2110-154">**Listing 4 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

<span data-ttu-id="d2110-155">Dans la liste 4, le `TestDetailsView()` méthode teste les données d’affichage retournée en appelant le `Details()` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d2110-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="d2110-156">Le `ViewData` est exposée en tant que propriété sur le `ViewResult` retournée en appelant le `Details()` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d2110-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="d2110-157">Le `ViewData.Model` propriété contient le produit passé à la vue.</span><span class="sxs-lookup"><span data-stu-id="d2110-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="d2110-158">Le test vérifie simplement que le produit contenu dans les données d’affichage a le nom d’ordinateur portable.</span><span class="sxs-lookup"><span data-stu-id="d2110-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="d2110-159">Le résultat d’Action de test retourné par un contrôleur</span><span class="sxs-lookup"><span data-stu-id="d2110-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="d2110-160">Une action de contrôleur plus complexe peut-être retourner différents types de résultats d’action en fonction des valeurs de paramètres transmis à l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="d2110-161">Une action de contrôleur peut retourner une variété de types de résultats d’action, notamment une `ViewResult`, `RedirectToRouteResult`, ou `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="d2110-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="d2110-162">Par exemple, la modification `Details()` action dans la liste 5 retourne le `Details` afficher quand vous passez un Id de produit valide à l’action.</span><span class="sxs-lookup"><span data-stu-id="d2110-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="d2110-163">Si vous passez un produit non valide Id--un Id avec une valeur inférieure à 1, alors que vous êtes redirigé vers la `Index()` action.</span><span class="sxs-lookup"><span data-stu-id="d2110-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="d2110-164">**La liste 5 : `ProductController.vb`**</span><span class="sxs-lookup"><span data-stu-id="d2110-164">**Listing 5 – `ProductController.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

<span data-ttu-id="d2110-165">Vous pouvez tester le comportement de la `Details()` action avec le test unitaire dans la liste 6.</span><span class="sxs-lookup"><span data-stu-id="d2110-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="d2110-166">Le test unitaire dans la liste 6 vérifie que vous êtes redirigé vers la `Index` afficher lorsqu’un Id avec la valeur -1 est passé à la `Details()` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d2110-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="d2110-167">**La liste 6 : `ProductControllerTest.vb`**</span><span class="sxs-lookup"><span data-stu-id="d2110-167">**Listing 6 – `ProductControllerTest.vb`**</span></span>

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

<span data-ttu-id="d2110-168">Lorsque vous appelez le `RedirectToAction()` méthode dans une action de contrôleur, l’action du contrôleur retourne un `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="d2110-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="d2110-169">Les contrôles de test si le `RedirectToRouteResult` redirige l’utilisateur à une action de contrôleur nommée `Index`.</span><span class="sxs-lookup"><span data-stu-id="d2110-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="d2110-170">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="d2110-170">Summary</span></span>

<span data-ttu-id="d2110-171">Dans ce didacticiel, vous avez appris à créer des tests unitaires pour les actions de contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="d2110-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="d2110-172">Tout d’abord, vous avez appris comment vérifier si la vue de droite est retournée par une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="d2110-173">Vous avez appris à utiliser le `ViewResult.ViewName` propriété pour vérifier le nom d’une vue.</span><span class="sxs-lookup"><span data-stu-id="d2110-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="d2110-174">Ensuite, nous avons examiné comment vous pouvez tester le contenu de `View Data`.</span><span class="sxs-lookup"><span data-stu-id="d2110-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="d2110-175">Vous avez appris comment vérifier si le produit de droite a été retourné dans `View Data` après l’appel à une action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="d2110-176">Enfin, nous avons expliqué comment vous pouvez tester si les différents types de résultats d’action sont renvoyées à partir d’une action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d2110-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="d2110-177">Vous avez appris comment tester si un contrôleur retourne un `ViewResult` ou `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="d2110-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d2110-178">Précédent</span><span class="sxs-lookup"><span data-stu-id="d2110-178">Previous</span></span>](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
