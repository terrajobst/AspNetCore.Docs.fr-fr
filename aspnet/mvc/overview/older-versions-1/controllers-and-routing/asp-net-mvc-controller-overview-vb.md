---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Vue d’ensemble du contrôleur ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther vous présente les contrôleurs ASP.NET MVC. Vous allez apprendre à créer de nouveaux contrôleurs et de retourner différents types de res d’action...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: d1fa2e71ae37d9123b489511c57ae0f79c0802ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371776"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="ea44e-104">Vue d’ensemble du contrôleur ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="ea44e-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="ea44e-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ea44e-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ea44e-106">Dans ce didacticiel, Stephen Walther vous présente les contrôleurs ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ea44e-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="ea44e-107">Vous allez apprendre à créer de nouveaux contrôleurs et de retourner différents types de résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="ea44e-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="ea44e-108">Ce didacticiel explore le sujet de contrôleurs ASP.NET MVC, les actions de contrôleur et les résultats d’action.</span><span class="sxs-lookup"><span data-stu-id="ea44e-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="ea44e-109">Après avoir terminé ce didacticiel, vous comprendrez comment les contrôleurs sont utilisés pour contrôler la façon dont un visiteur interagit avec un site Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ea44e-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="ea44e-110">Présentation des contrôleurs</span><span class="sxs-lookup"><span data-stu-id="ea44e-110">Understanding Controllers</span></span>

<span data-ttu-id="ea44e-111">Contrôleurs MVC sont chargés de répondre aux requêtes effectuées par rapport à un site Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ea44e-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="ea44e-112">Chaque demande de navigateur est mappée à un contrôleur spécifique.</span><span class="sxs-lookup"><span data-stu-id="ea44e-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="ea44e-113">Par exemple, imaginez que vous entrez l’URL suivante dans la barre d’adresses de votre navigateur :</span><span class="sxs-lookup"><span data-stu-id="ea44e-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="ea44e-114">Dans ce cas, un contrôleur nommé ProductController est appelé.</span><span class="sxs-lookup"><span data-stu-id="ea44e-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="ea44e-115">Le ProductController est responsable de la génération de la réponse à la demande de navigateur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="ea44e-116">Par exemple, le contrôleur peut retourner une vue particulière dans le navigateur ou le contrôleur peut rediriger l’utilisateur vers un autre contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="ea44e-117">Listing 1 contient un simple contrôleur nommé ProductController.</span><span class="sxs-lookup"><span data-stu-id="ea44e-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="ea44e-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="ea44e-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="ea44e-119">Comme vous pouvez le voir à partir de la liste 1, un contrôleur est simplement une classe (une classe Visual Basic .NET ou c#).</span><span class="sxs-lookup"><span data-stu-id="ea44e-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="ea44e-120">Un contrôleur est une classe qui dérive de la classe de base System.Web.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="ea44e-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="ea44e-121">Un contrôleur hérite de cette classe de base, un contrôleur hérite de plusieurs méthodes utiles gratuitement (nous y reviendrons ces méthodes dans un instant).</span><span class="sxs-lookup"><span data-stu-id="ea44e-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="ea44e-122">Présentation des Actions de contrôleur</span><span class="sxs-lookup"><span data-stu-id="ea44e-122">Understanding Controller Actions</span></span>

<span data-ttu-id="ea44e-123">Un contrôleur expose les actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-123">A controller exposes controller actions.</span></span> <span data-ttu-id="ea44e-124">Une action est une méthode sur un contrôleur qui est appelée lorsque vous entrez une URL particulière dans la barre d’adresse de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="ea44e-125">Par exemple, imaginez que vous effectuez une requête pour l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="ea44e-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="ea44e-126">Dans ce cas, la méthode Index() est appelée sur la classe ProductController.</span><span class="sxs-lookup"><span data-stu-id="ea44e-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="ea44e-127">La méthode Index() est un exemple d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="ea44e-128">Une action de contrôleur doit être une méthode publique d’une classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="ea44e-129">Méthodes de Visual Basic.NET, par défaut, sont des méthodes publiques.</span><span class="sxs-lookup"><span data-stu-id="ea44e-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="ea44e-130">Notez que toute méthode publique que vous ajoutez à une classe de contrôleur est exposée comme une action de contrôleur automatiquement (vous devez veiller à ce sujet dans la mesure où une action de contrôleur peut être appelée par n’importe qui dans l’univers en tapant l’URL de droite dans une barre d’adresses du navigateur).</span><span class="sxs-lookup"><span data-stu-id="ea44e-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="ea44e-131">Il existe certaines exigences supplémentaires qui doivent être satisfaites par une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="ea44e-132">Une méthode utilisée comme une action de contrôleur ne peut pas être surchargée.</span><span class="sxs-lookup"><span data-stu-id="ea44e-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="ea44e-133">En outre, une action de contrôleur ne peut pas être une méthode statique.</span><span class="sxs-lookup"><span data-stu-id="ea44e-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="ea44e-134">Autrement, vous pouvez utiliser pratiquement n’importe quelle méthode comme une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="ea44e-135">Présentation des résultats d’Action</span><span class="sxs-lookup"><span data-stu-id="ea44e-135">Understanding Action Results</span></span>

<span data-ttu-id="ea44e-136">Une action de contrôleur retourne ce que l'on appelle un *résultat d’action*.</span><span class="sxs-lookup"><span data-stu-id="ea44e-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="ea44e-137">Un résultat d’action est ce que retourne une action de contrôleur en réponse à une demande de navigateur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="ea44e-138">L’infrastructure ASP.NET MVC prend en charge plusieurs types de résultats d’action, notamment :</span><span class="sxs-lookup"><span data-stu-id="ea44e-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="ea44e-139">ViewResult - représente HTML et le balisage.</span><span class="sxs-lookup"><span data-stu-id="ea44e-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="ea44e-140">EmptyResult - ne représente aucun résultat.</span><span class="sxs-lookup"><span data-stu-id="ea44e-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="ea44e-141">RedirectResult - représente une redirection vers une nouvelle URL.</span><span class="sxs-lookup"><span data-stu-id="ea44e-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="ea44e-142">JsonResult - représente un résultat de JavaScript Object Notation qui peut être utilisé dans une application AJAX.</span><span class="sxs-lookup"><span data-stu-id="ea44e-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="ea44e-143">JavaScriptResult - représente un script JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ea44e-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="ea44e-144">ContentResult - représente un résultat de texte.</span><span class="sxs-lookup"><span data-stu-id="ea44e-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="ea44e-145">FileContentResult - représente un fichier téléchargeable (avec le contenu binaire).</span><span class="sxs-lookup"><span data-stu-id="ea44e-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="ea44e-146">FilePathResult - représente un fichier téléchargeable (avec un chemin d’accès).</span><span class="sxs-lookup"><span data-stu-id="ea44e-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="ea44e-147">FileStreamResult - représente un fichier téléchargeable (avec un flux de fichier).</span><span class="sxs-lookup"><span data-stu-id="ea44e-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="ea44e-148">Tous ces résultats d’action héritent de la classe ActionResult base.</span><span class="sxs-lookup"><span data-stu-id="ea44e-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="ea44e-149">Dans la plupart des cas, une action de contrôleur retourne un ViewResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="ea44e-150">Par exemple, l’action de contrôleur d’Index dans la liste 2 Retourne un ViewResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="ea44e-151">**Listing 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="ea44e-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="ea44e-152">Lorsqu’une action retourne un ViewResult, HTML est retourné au navigateur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="ea44e-153">La méthode Index() dans le Listing 2 Retourne une vue nommée des Index dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="ea44e-154">Notez que l’action Index() dans le Listing 2 ne retourne pas un ViewResult().</span><span class="sxs-lookup"><span data-stu-id="ea44e-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="ea44e-155">Au lieu de cela, la méthode View() de la classe de base de contrôleur est appelée.</span><span class="sxs-lookup"><span data-stu-id="ea44e-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="ea44e-156">Normalement, vous ne retournez un résultat d’action directement.</span><span class="sxs-lookup"><span data-stu-id="ea44e-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="ea44e-157">Au lieu de cela, vous appelez une des méthodes suivantes de la classe de base du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="ea44e-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="ea44e-158">Vue - retourne un résultat d’action ViewResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="ea44e-159">Rediriger - retourne un résultat d’action RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="ea44e-160">RedirectToAction - retourne un résultat d’action RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="ea44e-161">RedirectToRoute - retourne un résultat d’action RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="ea44e-162">JSON - retourne un résultat d’action JsonResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="ea44e-163">JavaScriptResult - retourne un JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="ea44e-164">Contenu : retourne un résultat d’action ContentResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="ea44e-165">Fichier - retourne un FileContentResult, FilePathResult ou FileStreamResult selon les paramètres transmis à la méthode.</span><span class="sxs-lookup"><span data-stu-id="ea44e-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="ea44e-166">Par conséquent, si vous souhaitez renvoyer un affichage dans le navigateur, vous appelez la méthode View().</span><span class="sxs-lookup"><span data-stu-id="ea44e-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="ea44e-167">Si vous souhaitez rediriger l’utilisateur à partir de l’action d’un contrôleur à l’autre, vous appelez la méthode RedirectToAction().</span><span class="sxs-lookup"><span data-stu-id="ea44e-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="ea44e-168">Par exemple, l’action Details() dans le Listing 3 affiche une vue ou redirige l’utilisateur vers l’action Index() selon que le paramètre Id a une valeur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="ea44e-169">**Liste 3 - CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="ea44e-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="ea44e-170">Le résultat d’action ContentResult est spécial.</span><span class="sxs-lookup"><span data-stu-id="ea44e-170">The ContentResult action result is special.</span></span> <span data-ttu-id="ea44e-171">Vous pouvez utiliser le résultat d’action ContentResult pour retourner un résultat d’action en tant que texte brut.</span><span class="sxs-lookup"><span data-stu-id="ea44e-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="ea44e-172">Par exemple, la méthode Index() sur la liste 4 retourne un message en tant que texte brut et pas au format HTML.</span><span class="sxs-lookup"><span data-stu-id="ea44e-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="ea44e-173">**Liste 4 - Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="ea44e-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="ea44e-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="ea44e-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="ea44e-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="ea44e-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="ea44e-176">Lorsque l’action StatusController.Index() est appelée, une vue n’est pas renvoyée.</span><span class="sxs-lookup"><span data-stu-id="ea44e-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="ea44e-177">Au lieu de cela, le texte brut « Hello World ! »</span><span class="sxs-lookup"><span data-stu-id="ea44e-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="ea44e-178">est retourné au navigateur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-178">is returned to the browser.</span></span>

<span data-ttu-id="ea44e-179">Si une action de contrôleur retourne un résultat qui n’est pas un résultat d’action - par exemple, une date ou un entier - le résultat est encapsulé dans un ContentResult automatiquement.</span><span class="sxs-lookup"><span data-stu-id="ea44e-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="ea44e-180">Par exemple, lorsque l’action Index() de la WorkController dans la liste 5 est appelée, la date est retournée automatiquement comme un ContentResult.</span><span class="sxs-lookup"><span data-stu-id="ea44e-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="ea44e-181">**Liste 5 - WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="ea44e-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="ea44e-182">L’action Index() dans la liste 5 retourne un objet DateTime.</span><span class="sxs-lookup"><span data-stu-id="ea44e-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="ea44e-183">L’infrastructure ASP.NET MVC convertit l’objet DateTime en une chaîne et encapsule la valeur de date/heure dans un ContentResult automatiquement.</span><span class="sxs-lookup"><span data-stu-id="ea44e-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="ea44e-184">Le navigateur reçoit la date et l’heure en tant que texte brut.</span><span class="sxs-lookup"><span data-stu-id="ea44e-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="ea44e-185">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="ea44e-185">Summary</span></span>

<span data-ttu-id="ea44e-186">L’objectif de ce didacticiel a été pour vous présenter les concepts de contrôleurs ASP.NET MVC, les actions de contrôleur et les résultats d’action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="ea44e-187">Dans la première section, vous avez appris à ajouter de nouveaux contrôleurs pour un projet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ea44e-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="ea44e-188">Ensuite, vous avez appris les méthodes publics d’un contrôleur sont exposés à l’univers en tant qu’actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="ea44e-189">Enfin, nous avons abordé les différents types de résultats d’action qui peuvent être retournés à partir d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="ea44e-190">En particulier, nous avons vu comment renvoyer un ViewResult, RedirectToActionResult et ContentResult à partir d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ea44e-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ea44e-191">[Précédent](creating-a-custom-route-constraint-cs.md)
> [Suivant](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ea44e-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
