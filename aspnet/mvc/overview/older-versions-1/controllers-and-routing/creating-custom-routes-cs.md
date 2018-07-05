---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Création de Routes personnalisées (c#) | Microsoft Docs
author: microsoft
description: Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bb694b08d595b2ce75123c3da0e9b8e8d60a652
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399951"
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="ffa7e-104">Création de Routes personnalisées (c#)</span><span class="sxs-lookup"><span data-stu-id="ffa7e-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="ffa7e-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ffa7e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ffa7e-106">Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="ffa7e-107">Dans ce didacticiel, vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="ffa7e-108">Dans ce didacticiel, vous allez apprendre à ajouter un itinéraire personnalisé à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="ffa7e-109">Vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax avec un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="ffa7e-110">Pour de nombreuses applications ASP.NET MVC simples, la table d’itinéraires par défaut fonctionnera parfaitement.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="ffa7e-111">Toutefois, vous pouvez découvrir que vous avez routage des besoins spécifiques.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="ffa7e-112">Dans ce cas, vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="ffa7e-113">Par exemple, imaginez que vous générez une application de blog.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="ffa7e-114">Vous souhaiterez peut-être gérer les demandes entrantes qui ressemblent à ceci :</span><span class="sxs-lookup"><span data-stu-id="ffa7e-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="ffa7e-115">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="ffa7e-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="ffa7e-116">Lorsqu’un utilisateur entre cette demande, vous souhaitez retourner l’entrée de blog qui correspond à la date 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="ffa7e-117">Pour pouvoir traiter ce type de demande, vous devez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="ffa7e-118">Le fichier Global.asax dans le Listing 1 contient un nouvel itinéraire personnalisé, nommé Blog, qui traite les requêtes qui ressemblent à /Archive/*date d’entrée*.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="ffa7e-119">**Liste 1 - Global.asax (avec itinéraire personnalisé)**</span><span class="sxs-lookup"><span data-stu-id="ffa7e-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="ffa7e-120">L’ordre des itinéraires que vous ajoutez à la table de routage est importante.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="ffa7e-121">Notre nouvel itinéraire Blog personnalisé est ajouté avant l’itinéraire par défaut existant.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="ffa7e-122">Si vous inversons l’ordre, puis l’itinéraire par défaut sera toujours appelée au lieu de l’itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="ffa7e-123">L’itinéraire de Blog personnalisée correspond à toute demande qui commence par/Archive /.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="ffa7e-124">Par conséquent, elle correspond à toutes les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="ffa7e-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="ffa7e-125">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="ffa7e-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="ffa7e-126">/ Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="ffa7e-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="ffa7e-127">/ Archive/apple</span><span class="sxs-lookup"><span data-stu-id="ffa7e-127">/Archive/apple</span></span>

<span data-ttu-id="ffa7e-128">L’itinéraire personnalisé mappe la requête entrante à un contrôleur nommé Archive et appelle l’action Entry().</span><span class="sxs-lookup"><span data-stu-id="ffa7e-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="ffa7e-129">Lorsque la méthode Entry() est appelée, la date d’entrée est transmise en tant que paramètre nommé entryDate.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="ffa7e-130">Vous pouvez utiliser l’itinéraire personnalisé Blog avec le contrôleur dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="ffa7e-131">**Listing 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="ffa7e-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="ffa7e-132">Notez que la méthode Entry() dans le Listing 2 accepte un paramètre de type DateTime.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="ffa7e-133">L’infrastructure MVC est suffisamment intelligent pour convertir automatiquement la date d’entrée à partir de l’URL en une valeur DateTime.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="ffa7e-134">Si le paramètre de date d’entrée à partir de l’URL ne peut pas être converti en une valeur DateTime, une erreur est générée (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="ffa7e-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="ffa7e-135">**Figure 1 : erreur de conversion de paramètre**</span><span class="sxs-lookup"><span data-stu-id="ffa7e-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="ffa7e-136">[![La boîte de dialogue Nouveau projet](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ffa7e-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="ffa7e-137">**Figure 01**: erreur de conversion de paramètre ([cliquez pour afficher l’image en taille réelle](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="ffa7e-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="ffa7e-138">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="ffa7e-138">Summary</span></span>

<span data-ttu-id="ffa7e-139">L’objectif de ce didacticiel a été pour illustrer comment vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="ffa7e-140">Vous avez appris comment ajouter un itinéraire personnalisé à la table de routage dans le fichier Global.asax qui représente les entrées de blog.</span><span class="sxs-lookup"><span data-stu-id="ffa7e-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="ffa7e-141">Nous avons vu comment mapper les requêtes pour les entrées de blog à un contrôleur nommé ArchiveController et une action de contrôleur nommé Entry().</span><span class="sxs-lookup"><span data-stu-id="ffa7e-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ffa7e-142">[Précédent](aspnet-mvc-controllers-overview-cs.md)
> [Suivant](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ffa7e-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
