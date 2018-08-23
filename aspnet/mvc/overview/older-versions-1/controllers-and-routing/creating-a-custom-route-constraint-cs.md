---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Création d’une contrainte de Route personnalisée (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisé. Nous implémentons un simple contrainte personnalisée qui empêche un itinéraire mis en correspondance w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 0a0b6b706fdb212a745346ffaefc118e85c2a245
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831734"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="0c026-104">Création d’une contrainte de Route personnalisée (c#)</span><span class="sxs-lookup"><span data-stu-id="0c026-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="0c026-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="0c026-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="0c026-106">Stephen Walther montre comment vous pouvez créer une contrainte d’itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="0c026-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="0c026-107">Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire à partir de la mise en correspondance lors de l’exécution d’une demande de navigateur à partir d’un ordinateur distant.</span><span class="sxs-lookup"><span data-stu-id="0c026-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="0c026-108">L’objectif de ce didacticiel consiste à montrer comment vous pouvez créer une contrainte d’itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="0c026-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="0c026-109">Une contrainte d’itinéraire personnalisé vous pouvez ainsi empêcher un itinéraire à partir de la mise en correspondance, sauf si une condition personnalisée est mis en correspondance.</span><span class="sxs-lookup"><span data-stu-id="0c026-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="0c026-110">Dans ce didacticiel, créons une contrainte de route de Localhost.</span><span class="sxs-lookup"><span data-stu-id="0c026-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="0c026-111">La contrainte d’itinéraire Localhost correspond uniquement aux requêtes effectuées à partir de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="0c026-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="0c026-112">Les demandes à distance à partir de sur Internet ne correspondent pas.</span><span class="sxs-lookup"><span data-stu-id="0c026-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="0c026-113">Vous implémentez une contrainte d’itinéraire personnalisée en implémentant l’interface IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="0c026-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="0c026-114">Il s’agit d’une interface très simple qui décrit une méthode unique :</span><span class="sxs-lookup"><span data-stu-id="0c026-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="0c026-115">La méthode retourne une valeur booléenne.</span><span class="sxs-lookup"><span data-stu-id="0c026-115">The method returns a Boolean value.</span></span> <span data-ttu-id="0c026-116">Si vous retournez la valeur false, l’itinéraire associé à la contrainte ne correspondra à la demande de navigateur.</span><span class="sxs-lookup"><span data-stu-id="0c026-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="0c026-117">La contrainte de Localhost est contenue dans le Listing 1.</span><span class="sxs-lookup"><span data-stu-id="0c026-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="0c026-118">**Liste 1 - LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="0c026-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="0c026-119">La contrainte dans le Listing 1 tire parti de la propriété IsLocal exposée par la classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="0c026-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="0c026-120">Cette propriété retourne la valeur true lorsque l’adresse IP de la demande est soit 127.0.0.1 ou lorsque l’adresse IP de la demande est identique à l’adresse IP du serveur.</span><span class="sxs-lookup"><span data-stu-id="0c026-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="0c026-121">Vous utilisez une contrainte personnalisée au sein d’un itinéraire défini dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="0c026-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="0c026-122">Le fichier Global.asax dans le Listing 2 utilise la contrainte de Localhost pour empêcher toute personne demandant une page d’administration, à moins qu’ils envoient la demande à partir du serveur local.</span><span class="sxs-lookup"><span data-stu-id="0c026-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="0c026-123">Par exemple, une demande de /Admin/DeleteAll échouera lorsque effectuées à partir d’un serveur distant.</span><span class="sxs-lookup"><span data-stu-id="0c026-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="0c026-124">**Listing 2 - Global.asax**</span><span class="sxs-lookup"><span data-stu-id="0c026-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="0c026-125">La contrainte de Localhost est utilisée dans la définition de l’itinéraire de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="0c026-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="0c026-126">Cet itinéraire ne doit correspondre à une demande de navigateur à distance.</span><span class="sxs-lookup"><span data-stu-id="0c026-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="0c026-127">Sachez, cependant, que les autres itinéraires définis dans Global.asax peuvent correspondent à la même demande.</span><span class="sxs-lookup"><span data-stu-id="0c026-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="0c026-128">Il est important de comprendre qu’une contrainte empêche un itinéraire particulier à partir d’une demande de mise en correspondance, et pas tous les itinéraires définis dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="0c026-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="0c026-129">Notez que l’itinéraire par défaut a été commenté à partir du fichier Global.asax dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="0c026-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="0c026-130">Si vous incluez l’itinéraire par défaut, l’itinéraire par défaut serait correspond aux requêtes pour le contrôleur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="0c026-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="0c026-131">Dans ce cas, les utilisateurs distants peuvent toujours invoquer des actions du contrôleur Admin même si leurs demandes ne correspondent pas l’itinéraire de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="0c026-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c026-132">[Précédent](creating-a-route-constraint-cs.md)
> [Suivant](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0c026-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
