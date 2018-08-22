---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Création d’une contrainte d’itinéraire (c#) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont le navigateur demande itinéraires de correspondance en créant des contraintes de routage avec des expressions régulières.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51d76248a59968e1d6befd5d5404f7bf6c2878e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823681"
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="e4aaf-103">Création d’une contrainte d’itinéraire (c#)</span><span class="sxs-lookup"><span data-stu-id="e4aaf-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="e4aaf-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e4aaf-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e4aaf-105">Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont le navigateur demande itinéraires de correspondance en créant des contraintes de routage avec des expressions régulières.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="e4aaf-106">Contraintes de routage vous permet de limiter les demandes du navigateur qui correspondent à un itinéraire particulier.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="e4aaf-107">Vous pouvez utiliser une expression régulière pour spécifier une contrainte d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="e4aaf-108">Par exemple, imaginez que vous avez défini l’itinéraire dans la liste 1 dans votre fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="e4aaf-109">**Liste 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="e4aaf-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="e4aaf-110">Listing 1 contient un itinéraire nommé produit.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="e4aaf-111">Vous pouvez utiliser la gamme de produits pour mapper les requêtes de navigateur à le ProductController contenu dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="e4aaf-112">**Listing 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="e4aaf-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="e4aaf-113">Notez que l’action Details() exposée par le contrôleur produit accepte un paramètre unique nommé productId.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="e4aaf-114">Ce paramètre est un paramètre de type entier.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="e4aaf-115">L’itinéraire défini dans la liste 1 correspond à une des URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4aaf-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="e4aaf-116">/ Produit/23</span><span class="sxs-lookup"><span data-stu-id="e4aaf-116">/Product/23</span></span>
- <span data-ttu-id="e4aaf-117">/ Product/7</span><span class="sxs-lookup"><span data-stu-id="e4aaf-117">/Product/7</span></span>

<span data-ttu-id="e4aaf-118">Malheureusement, l’itinéraire correspond également les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4aaf-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="e4aaf-119">/ Product/texte</span><span class="sxs-lookup"><span data-stu-id="e4aaf-119">/Product/blah</span></span>
- <span data-ttu-id="e4aaf-120">/ Product/apple</span><span class="sxs-lookup"><span data-stu-id="e4aaf-120">/Product/apple</span></span>

<span data-ttu-id="e4aaf-121">Étant donné que l’action Details() attend un paramètre de type entier, qui effectue une requête qui contient l’autre chose qu’une valeur entière provoque une erreur.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="e4aaf-122">Par exemple, si vous tapez l’URL /Product/apple dans votre navigateur, vous obtiendrez la page d’erreur dans la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="e4aaf-123">[![La boîte de dialogue Nouveau projet](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e4aaf-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="e4aaf-124">**Figure 01**: voyez une page explode ([cliquez pour afficher l’image en taille réelle](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e4aaf-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="e4aaf-125">Ce que vous voulez vraiment faire est uniquement en correspondance les URL qui contiennent un productId entier approprié.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="e4aaf-126">Vous pouvez utiliser une contrainte lors de la définition d’un itinéraire pour restreindre les URL qui correspondent à l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="e4aaf-127">L’itinéraire de produit modifié dans la liste 3 contient une contrainte d’expression régulière qui correspond uniquement à des entiers.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="e4aaf-128">**Liste 3 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="e4aaf-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="e4aaf-129">L’expression régulière \d+ correspond à un ou plusieurs entiers.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="e4aaf-130">Cette contrainte provoque la gamme de produits faire correspondre les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4aaf-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="e4aaf-131">/ Product/3</span><span class="sxs-lookup"><span data-stu-id="e4aaf-131">/Product/3</span></span>
- <span data-ttu-id="e4aaf-132">/ Product/8999</span><span class="sxs-lookup"><span data-stu-id="e4aaf-132">/Product/8999</span></span>

<span data-ttu-id="e4aaf-133">Mais pas les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="e4aaf-133">But not the following URLs:</span></span>

- <span data-ttu-id="e4aaf-134">/ Product/apple</span><span class="sxs-lookup"><span data-stu-id="e4aaf-134">/Product/apple</span></span>
- <span data-ttu-id="e4aaf-135">/ Produit</span><span class="sxs-lookup"><span data-stu-id="e4aaf-135">/Product</span></span>

- <span data-ttu-id="e4aaf-136">Ces demandes du navigateur seront gérées par un autre itinéraire ou, si aucun itinéraire correspondant, un *Impossible de trouver la ressource* erreur est renvoyée.</span><span class="sxs-lookup"><span data-stu-id="e4aaf-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4aaf-137">[Précédent](creating-custom-routes-cs.md)
> [Suivant](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e4aaf-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
