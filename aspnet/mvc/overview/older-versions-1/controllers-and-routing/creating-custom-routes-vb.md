---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: "Création des itinéraires personnalisés (VB) | Documents Microsoft"
author: microsoft
description: "Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d3161bd1bf74df425d3c53873875a1abcfbfa05
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="20b9a-104">Création des itinéraires personnalisés (VB)</span><span class="sxs-lookup"><span data-stu-id="20b9a-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="20b9a-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="20b9a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="20b9a-106">Découvrez comment ajouter des itinéraires personnalisés à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="20b9a-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="20b9a-107">Dans ce didacticiel, vous allez apprendre à modifier la table d’itinéraires par défaut dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="20b9a-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="20b9a-108">Dans ce didacticiel, vous allez apprendre à ajouter un itinéraire personnalisé à une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="20b9a-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="20b9a-109">Vous apprenez à modifier la table d’itinéraires par défaut dans le fichier Global.asax avec un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="20b9a-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="20b9a-110">Dans les applications ASP.NET MVC, la table d’itinéraires par défaut fonctionne parfaitement.</span><span class="sxs-lookup"><span data-stu-id="20b9a-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="20b9a-111">Toutefois, vous pouvez découvrir que vous avez routage des besoins spécifiques.</span><span class="sxs-lookup"><span data-stu-id="20b9a-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="20b9a-112">Dans ce cas, vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="20b9a-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="20b9a-113">Par exemple, imaginez que vous créez une application de blog.</span><span class="sxs-lookup"><span data-stu-id="20b9a-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="20b9a-114">Vous pouvez souhaiter gérer les demandes entrantes ressemblent à ceci :</span><span class="sxs-lookup"><span data-stu-id="20b9a-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="20b9a-115">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="20b9a-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="20b9a-116">Lorsqu’un utilisateur entre cette demande, vous souhaitez renvoyer l’entrée de blog qui correspond à la date 12/25/2009.</span><span class="sxs-lookup"><span data-stu-id="20b9a-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="20b9a-117">Afin de gérer ce type de requête, vous devez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="20b9a-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="20b9a-118">Le fichier Global.asax dans la liste 1 contient un nouvel itinéraire personnalisé, nommé Blog, qui traite les requêtes qui ressemblent à /Archive/*date d’entrée*.</span><span class="sxs-lookup"><span data-stu-id="20b9a-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="20b9a-119">**La liste 1 - Global.asax (avec itinéraire personnalisé)**</span><span class="sxs-lookup"><span data-stu-id="20b9a-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="20b9a-120">L’ordre des itinéraires que vous ajoutez à la table de routage est important.</span><span class="sxs-lookup"><span data-stu-id="20b9a-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="20b9a-121">Notre nouvel itinéraire Blog personnalisé est ajouté avant l’itinéraire par défaut existant.</span><span class="sxs-lookup"><span data-stu-id="20b9a-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="20b9a-122">Si vous le l’ordre inverse, puis l’itinéraire par défaut est toujours appelé au lieu de l’itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="20b9a-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="20b9a-123">L’itinéraire de Blog personnalisée correspond à toute demande qui commence par/archivage.</span><span class="sxs-lookup"><span data-stu-id="20b9a-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="20b9a-124">Par conséquent, il correspond à toutes les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="20b9a-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="20b9a-125">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="20b9a-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="20b9a-126">/ Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="20b9a-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="20b9a-127">/ Archive/apple</span><span class="sxs-lookup"><span data-stu-id="20b9a-127">/Archive/apple</span></span>

<span data-ttu-id="20b9a-128">L’itinéraire personnalisé mappe la demande entrante à un contrôleur nommé Archive et appelle l’action Entry().</span><span class="sxs-lookup"><span data-stu-id="20b9a-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="20b9a-129">Lorsque la méthode Entry() est appelée, la date d’entrée est passée comme un paramètre nommé entryDate.</span><span class="sxs-lookup"><span data-stu-id="20b9a-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="20b9a-130">Vous pouvez utiliser l’itinéraire personnalisé Blog avec le contrôleur dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="20b9a-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="20b9a-131">**Listing 2 - ArchiveController.vb**</span><span class="sxs-lookup"><span data-stu-id="20b9a-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="20b9a-132">Notez que la méthode Entry() dans la liste 2 accepte un paramètre de type DateTime.</span><span class="sxs-lookup"><span data-stu-id="20b9a-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="20b9a-133">L’infrastructure MVC est assez intelligente pour convertir automatiquement la date d’entrée à partir de l’URL en une valeur DateTime.</span><span class="sxs-lookup"><span data-stu-id="20b9a-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="20b9a-134">Si le paramètre de date d’entrée à partir de l’URL ne peut pas être converti en une valeur DateTime, une erreur est générée (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="20b9a-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="20b9a-135">**Figure 1 : erreur de conversion de paramètre**</span><span class="sxs-lookup"><span data-stu-id="20b9a-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="20b9a-136">[![La boîte de dialogue Nouveau projet](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="20b9a-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="20b9a-137">**Figure 01**: erreur de conversion de paramètre ([cliquez pour afficher l’image en taille réelle](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="20b9a-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="20b9a-138">Résumé</span><span class="sxs-lookup"><span data-stu-id="20b9a-138">Summary</span></span>

<span data-ttu-id="20b9a-139">L’objectif de ce didacticiel a pour illustrer comment vous pouvez créer un itinéraire personnalisé.</span><span class="sxs-lookup"><span data-stu-id="20b9a-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="20b9a-140">Vous avez appris comment ajouter un itinéraire personnalisé à la table de routage dans le fichier Global.asax qui représente les entrées de blog.</span><span class="sxs-lookup"><span data-stu-id="20b9a-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="20b9a-141">Nous avons expliqué comment mapper des demandes d’entrées de blog à un contrôleur nommé ArchiveController et une action de contrôleur nommé Entry().</span><span class="sxs-lookup"><span data-stu-id="20b9a-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="20b9a-142">[Précédent](asp-net-mvc-controller-overview-vb.md)
[Suivant](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="20b9a-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
