---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Vue d’ensemble du routage ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes du navigateur pour les actions de contrôleur.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 7511bc98667ac3acfccf78ea74b1369cf47bdc3b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813389"
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="abd27-103">Vue d’ensemble du routage ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="abd27-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="abd27-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="abd27-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="abd27-105">Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes du navigateur pour les actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="abd27-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="abd27-106">Dans ce didacticiel, vous découvrirez une fonctionnalité importante de toutes les applications ASP.NET MVC appelée *routage ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="abd27-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="abd27-107">Le module de routage ASP.NET est chargé du mappage des demandes du navigateur entrantes à des actions de contrôleur MVC particuliers.</span><span class="sxs-lookup"><span data-stu-id="abd27-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="abd27-108">À la fin de ce didacticiel, vous comprendrez comment la table de routage standard mappe les requêtes à actions du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="abd27-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="abd27-109">À l’aide de la Table d’itinéraire par défaut</span><span class="sxs-lookup"><span data-stu-id="abd27-109">Using the Default Route Table</span></span>

<span data-ttu-id="abd27-110">Lorsque vous créez une application ASP.NET MVC, l’application est déjà configurée pour utiliser le routage ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="abd27-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="abd27-111">ASP.NET Routing est configuré dans deux emplacements.</span><span class="sxs-lookup"><span data-stu-id="abd27-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="abd27-112">Tout d’abord, le routage ASP.NET est activé dans le fichier de configuration de votre application Web (fichier Web.config).</span><span class="sxs-lookup"><span data-stu-id="abd27-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="abd27-113">Il existe quatre sections dans le fichier de configuration qui sont pertinentes pour le routage : la section system.web.httpModules, la section system.web.httpHandlers, la section system.webserver.modules et la section system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="abd27-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="abd27-114">Veillez à ne pas supprimer ces sections, car sans ces sections routage ne fonctionnera plus.</span><span class="sxs-lookup"><span data-stu-id="abd27-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="abd27-115">Deuxièmement et plus important encore, une table de routage est créée dans le fichier Global.asax de l’application.</span><span class="sxs-lookup"><span data-stu-id="abd27-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="abd27-116">Le fichier Global.asax est un fichier spécial qui contient les gestionnaires d’événements pour les événements de cycle de vie des applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="abd27-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="abd27-117">La table de routage est créée lors de l’événement de démarrer l’Application.</span><span class="sxs-lookup"><span data-stu-id="abd27-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="abd27-118">Le fichier dans le Listing 1 contient le fichier Global.asax de valeur par défaut pour une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="abd27-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="abd27-119">**Liste 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="abd27-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="abd27-120">Quand une application MVC commence, l’Application\_méthode Start() est appelée.</span><span class="sxs-lookup"><span data-stu-id="abd27-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="abd27-121">Cette méthode, appelle à son tour, la méthode RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="abd27-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="abd27-122">La méthode RegisterRoutes() crée la table de routage.</span><span class="sxs-lookup"><span data-stu-id="abd27-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="abd27-123">La table d’itinéraires par défaut contient un itinéraire unique (nommé par défaut).</span><span class="sxs-lookup"><span data-stu-id="abd27-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="abd27-124">L’itinéraire par défaut mappe le premier segment d’URL à un nom de contrôleur, le deuxième segment d’URL à une action de contrôleur et le troisième segment à un paramètre nommé **id**.</span><span class="sxs-lookup"><span data-stu-id="abd27-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="abd27-125">Imaginez que vous entrez l’URL suivante dans la barre d’adresses de votre navigateur web :</span><span class="sxs-lookup"><span data-stu-id="abd27-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="abd27-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="abd27-126">/Home/Index/3</span></span>

<span data-ttu-id="abd27-127">L’itinéraire par défaut est mappé à cette URL pour les paramètres suivants :</span><span class="sxs-lookup"><span data-stu-id="abd27-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="abd27-128">contrôleur = Home</span><span class="sxs-lookup"><span data-stu-id="abd27-128">controller = Home</span></span>

- <span data-ttu-id="abd27-129">action = Index</span><span class="sxs-lookup"><span data-stu-id="abd27-129">action = Index</span></span>

- <span data-ttu-id="abd27-130">ID = 3</span><span class="sxs-lookup"><span data-stu-id="abd27-130">id = 3</span></span>

<span data-ttu-id="abd27-131">Lorsque vous demandez l’URL de base/Index/3, le code suivant est exécuté :</span><span class="sxs-lookup"><span data-stu-id="abd27-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="abd27-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="abd27-132">HomeController.Index(3)</span></span>

<span data-ttu-id="abd27-133">L’itinéraire par défaut inclut les valeurs par défaut pour tous les trois paramètres.</span><span class="sxs-lookup"><span data-stu-id="abd27-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="abd27-134">Si vous ne fournissez pas un contrôleur, puis le paramètre de contrôleur par défaut la valeur **accueil**.</span><span class="sxs-lookup"><span data-stu-id="abd27-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="abd27-135">Si vous ne fournissez pas une action, le paramètre d’action par défaut la valeur **Index**.</span><span class="sxs-lookup"><span data-stu-id="abd27-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="abd27-136">Enfin, si vous ne fournissez pas un id, le paramètre id par défaut est une chaîne vide.</span><span class="sxs-lookup"><span data-stu-id="abd27-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="abd27-137">Examinons quelques exemples montrant comment l’itinéraire par défaut mappe des URL à actions du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="abd27-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="abd27-138">Imaginez que vous entrez l’URL suivante dans votre barre d’adresses du navigateur :</span><span class="sxs-lookup"><span data-stu-id="abd27-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="abd27-139">/ Édition familiale</span><span class="sxs-lookup"><span data-stu-id="abd27-139">/Home</span></span>

<span data-ttu-id="abd27-140">En raison de l’itinéraire par défaut, paramètres par défaut entrer cette URL entraîne la méthode Index() de la classe HomeController dans la liste de 2 à appeler.</span><span class="sxs-lookup"><span data-stu-id="abd27-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="abd27-141">**Listing 2 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="abd27-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="abd27-142">Dans la liste 2, la classe HomeController inclut une méthode nommée Index() qui accepte un paramètre unique nommé ID. L’URL de base indique à la méthode Index() soit appelée avec la valeur Nothing comme valeur du paramètre Id.</span><span class="sxs-lookup"><span data-stu-id="abd27-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="abd27-143">En raison de la façon que l’infrastructure MVC appelle les actions de contrôleur, l’URL de base correspond également à la méthode Index() de la classe HomeController dans le Listing 3.</span><span class="sxs-lookup"><span data-stu-id="abd27-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="abd27-144">**Liste 3 - HomeController.vb (action Index sans paramètre)**</span><span class="sxs-lookup"><span data-stu-id="abd27-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="abd27-145">La méthode Index() dans le Listing 3 n’accepte pas de tous les paramètres.</span><span class="sxs-lookup"><span data-stu-id="abd27-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="abd27-146">L’URL de base entraîne cette méthode Index() à appeler.</span><span class="sxs-lookup"><span data-stu-id="abd27-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="abd27-147">L’URL de base/Index/3 appelle également cette méthode (l’Id est ignoré).</span><span class="sxs-lookup"><span data-stu-id="abd27-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="abd27-148">La base de l’URL correspond également à la méthode Index() de la classe HomeController sur la liste 4.</span><span class="sxs-lookup"><span data-stu-id="abd27-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="abd27-149">**Liste 4 - HomeController.vb (action Index avec le paramètre nullable)**</span><span class="sxs-lookup"><span data-stu-id="abd27-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="abd27-150">Dans la liste 4, la méthode Index() a un paramètre de type entier.</span><span class="sxs-lookup"><span data-stu-id="abd27-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="abd27-151">Étant donné que le paramètre est un paramètre nullable (peut avoir la valeur Nothing), le Index() peut être appelée sans provoquer d’erreur.</span><span class="sxs-lookup"><span data-stu-id="abd27-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="abd27-152">Enfin, l’appel à la méthode Index() dans la liste des 5 avec la base de l’URL entraîne une exception depuis le paramètre Id *n’est pas* un paramètre nullable.</span><span class="sxs-lookup"><span data-stu-id="abd27-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="abd27-153">Si vous tentez d’appeler la méthode Index() puis vous obtenez l’erreur affichée dans la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="abd27-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="abd27-154">**Liste 5 - HomeController.vb (action Index avec le paramètre Id)**</span><span class="sxs-lookup"><span data-stu-id="abd27-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="abd27-155">[![Appel d’une action de contrôleur qui attend une valeur de paramètre](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="abd27-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="abd27-156">**Figure 01**: appel d’une action de contrôleur qui attend une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="abd27-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="abd27-157">L’URL de base/Index/3, quant à eux, fonctionne parfaitement avec l’action de contrôleur d’Index dans la liste 5.</span><span class="sxs-lookup"><span data-stu-id="abd27-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="abd27-158">La demande /Home/Index/3 indique à la méthode Index() à être appelée avec un paramètre d’Id qui a la valeur 3.</span><span class="sxs-lookup"><span data-stu-id="abd27-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="abd27-159">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="abd27-159">Summary</span></span>

<span data-ttu-id="abd27-160">L’objectif de ce didacticiel est de vous fournir une brève introduction au routage ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="abd27-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="abd27-161">Nous avons examiné la table d’itinéraires par défaut que vous obtenez avec une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="abd27-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="abd27-162">Vous avez appris comment l’itinéraire par défaut mappe des URL à actions du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="abd27-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="abd27-163">[Précédent](creating-an-action-cs.md)
> [Suivant](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="abd27-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
