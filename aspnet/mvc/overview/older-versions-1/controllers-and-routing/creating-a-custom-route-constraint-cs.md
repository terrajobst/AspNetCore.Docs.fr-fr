---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Création d’une contrainte d’itinéraire de personnalisés (c#) | Documents Microsoft
author: StephenWalther
description: Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisées. Nous implémentons un simple contrainte personnalisé qui empêche un itinéraire mis en correspondance w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="ab34d-104">Création d’une contrainte d’itinéraire de personnalisés (c#)</span><span class="sxs-lookup"><span data-stu-id="ab34d-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="ab34d-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ab34d-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ab34d-106">Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisées.</span><span class="sxs-lookup"><span data-stu-id="ab34d-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="ab34d-107">Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire à partir de la mise en correspondance lors d’une demande de navigateur à partir d’un ordinateur distant.</span><span class="sxs-lookup"><span data-stu-id="ab34d-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="ab34d-108">L’objectif de ce didacticiel est de montrer comment vous pouvez créer une contrainte d’itinéraire personnalisée.</span><span class="sxs-lookup"><span data-stu-id="ab34d-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="ab34d-109">Une contrainte d’itinéraire personnalisée vous permet de vous permet d’empêcher un itinéraire à partir de la mise en correspondance, sauf si une condition personnalisée est mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="ab34d-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="ab34d-110">Dans ce didacticiel, nous créer une contrainte d’itinéraire Localhost.</span><span class="sxs-lookup"><span data-stu-id="ab34d-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="ab34d-111">La contrainte d’itinéraire Localhost correspond uniquement aux demandes effectuées à partir de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ab34d-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="ab34d-112">Requêtes distantes à partir de sur Internet ne correspondent pas.</span><span class="sxs-lookup"><span data-stu-id="ab34d-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="ab34d-113">Vous implémentez une contrainte d’itinéraire personnalisée en implémentant l’interface IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="ab34d-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="ab34d-114">Il s’agit d’une interface très simple qui décrit une méthode unique :</span><span class="sxs-lookup"><span data-stu-id="ab34d-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="ab34d-115">La méthode retourne une valeur booléenne.</span><span class="sxs-lookup"><span data-stu-id="ab34d-115">The method returns a Boolean value.</span></span> <span data-ttu-id="ab34d-116">Si vous retournez la valeur est false, l’itinéraire associé à la contrainte ne correspond à la demande du navigateur.</span><span class="sxs-lookup"><span data-stu-id="ab34d-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="ab34d-117">La contrainte de Localhost est contenue dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="ab34d-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="ab34d-118">**La liste 1 - LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="ab34d-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="ab34d-119">La contrainte dans la liste 1 tire parti de la propriété IsLocal exposée par la classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="ab34d-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="ab34d-120">Cette propriété retourne la valeur true lorsque l’adresse IP de la demande est soit 127.0.0.1 ou lorsque l’adresse IP de la demande est le même que l’adresse IP du serveur.</span><span class="sxs-lookup"><span data-stu-id="ab34d-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="ab34d-121">Vous utilisez une contrainte personnalisée au sein d’un itinéraire défini dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ab34d-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="ab34d-122">Le fichier Global.asax dans le Listing 2 utilise la contrainte de Localhost pour empêcher toute personne de demander une page d’administration, sauf si elles effectuer la demande à partir du serveur local.</span><span class="sxs-lookup"><span data-stu-id="ab34d-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="ab34d-123">Par exemple, une demande de /Admin/DeleteAll échoue lors d’un serveur distant.</span><span class="sxs-lookup"><span data-stu-id="ab34d-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="ab34d-124">**Listing 2 - Global.asax**</span><span class="sxs-lookup"><span data-stu-id="ab34d-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="ab34d-125">La contrainte de Localhost est utilisée dans la définition de l’itinéraire de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="ab34d-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="ab34d-126">Cet itinéraire ne doit correspondre à une demande de navigateur à distance.</span><span class="sxs-lookup"><span data-stu-id="ab34d-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="ab34d-127">Toutefois, sachez qu’autres itinéraires définis dans Global.asax peuvent correspondre à la même demande.</span><span class="sxs-lookup"><span data-stu-id="ab34d-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="ab34d-128">Il est important de comprendre qu’une contrainte empêche un itinéraire particulier à partir d’une demande de mise en correspondance, et pas tous les itinéraires définis dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ab34d-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="ab34d-129">Notez que l’itinéraire par défaut a été commenté à partir du fichier Global.asax dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="ab34d-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="ab34d-130">Si vous incluez l’itinéraire par défaut, l’itinéraire par défaut correspondrait demandes pour le contrôleur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="ab34d-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="ab34d-131">Dans ce cas, les utilisateurs distants peuvent toujours invoquer des actions du contrôleur Admin même si leurs demandes ne correspond à l’itinéraire de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="ab34d-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ab34d-132">[Précédent](creating-a-route-constraint-cs.md)
> [Suivant](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ab34d-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
