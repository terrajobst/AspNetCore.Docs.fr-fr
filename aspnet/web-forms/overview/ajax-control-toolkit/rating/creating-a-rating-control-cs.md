---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Création d’un contrôle d’évaluation (c#) | Documents Microsoft
author: wenz
description: De nombreux sites Web, de commerce électronique pour les sites de Communauté, offrent à leurs utilisateurs aux articles de taux ou aux éléments. Cela nécessite généralement des efforts de codage, mais nous avons le...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a48cf0ed9402e2875e87ba7bdb76afc5f501a670
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879593"
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="c26db-104">Création d’un contrôle d’évaluation (c#)</span><span class="sxs-lookup"><span data-stu-id="c26db-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="c26db-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c26db-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c26db-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c26db-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="c26db-107">De nombreux sites Web, de commerce électronique pour les sites de Communauté, offrent à leurs utilisateurs aux articles de taux ou aux éléments.</span><span class="sxs-lookup"><span data-stu-id="c26db-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="c26db-108">Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="c26db-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="c26db-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c26db-109">Overview</span></span>

<span data-ttu-id="c26db-110">De nombreux sites Web, de commerce électronique pour les sites de Communauté, offrent à leurs utilisateurs aux articles de taux ou aux éléments.</span><span class="sxs-lookup"><span data-stu-id="c26db-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="c26db-111">Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="c26db-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="c26db-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="c26db-112">Steps</span></span>

<span data-ttu-id="c26db-113">Tout d’abord, vous avez besoin (au moins) deux types d’images : un pour remplie à l’élément de contrôle d’accès et pour un élément de contrôle d’accès vide.</span><span class="sxs-lookup"><span data-stu-id="c26db-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="c26db-114">Un élément de contrôle d’accès est généralement une étoile ou une émoticône.</span><span class="sxs-lookup"><span data-stu-id="c26db-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="c26db-115">Pour ce scénario, vous trouver trois fichiers, smiley.png et empty.png et émoticône-done.png dans le cadre des téléchargements de code source pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c26db-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="c26db-116">Ensuite, créez un nouveau fichier ASP.NET et commencez par ajouter un `ScriptManager` contrôle à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="c26db-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="c26db-117">Ensuite, ajoutez le `Rating` contrôle à partir de la boîte à outils de contrôle ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="c26db-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="c26db-118">Les attributs suivants doivent être définis pour cet exemple :</span><span class="sxs-lookup"><span data-stu-id="c26db-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="c26db-119">`CurrentRating` l’évaluation initiale à utiliser</span><span class="sxs-lookup"><span data-stu-id="c26db-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="c26db-120">`MaxRating` la classification maximale</span><span class="sxs-lookup"><span data-stu-id="c26db-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="c26db-121">`EmptyStarCssClass` la classe CSS à utiliser lorsqu’un élément de contrôle d’accès (en étoile) est vide</span><span class="sxs-lookup"><span data-stu-id="c26db-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="c26db-122">`FilledStarCssClass` la classe CSS à utiliser lorsqu’un élément de contrôle d’accès (en étoile) est renseignée.</span><span class="sxs-lookup"><span data-stu-id="c26db-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="c26db-123">`StarCssClass` la classe CSS à utiliser pour un état visible</span><span class="sxs-lookup"><span data-stu-id="c26db-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="c26db-124">`WaitingStarCssClass` la classe CSS à utiliser pendant un nombre d’étoiles est envoyé sur le serveur</span><span class="sxs-lookup"><span data-stu-id="c26db-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="c26db-125">Et Voici le balisage qui crée un contrôle d’évaluation avec cinq éléments (smileys) dont aucun n’est remplie initialement :</span><span class="sxs-lookup"><span data-stu-id="c26db-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="c26db-126">Les trois classes CSS référencés devant maintenant afficher les fichiers d’image appropriée, qui est facile à effectuer à l’aide de CSS :</span><span class="sxs-lookup"><span data-stu-id="c26db-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="c26db-127">Assurez-vous que vous fournissez la largeur et la hauteur des trois images, sinon l’affichage peut sembler un peu tordue.</span><span class="sxs-lookup"><span data-stu-id="c26db-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="c26db-128">Enfin, le résultat de l’évaluation doit être affiché à l’utilisateur (ou, au moins enregistré dans une base de données).</span><span class="sxs-lookup"><span data-stu-id="c26db-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="c26db-129">Par conséquent, ajoutez une étiquette pour la sortie d’un message texte et un bouton Envoyer le formulaire d’évaluation sur le serveur de publication :</span><span class="sxs-lookup"><span data-stu-id="c26db-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="c26db-130">Dans le code côté serveur, accéder au contrôle de l’évaluation via son `ID` , puis accédez à son `CurrentRating` propriété qui correspond au nombre des éléments de contrôle d’accès sélectionné, dans notre exemple, une valeur comprise entre 0 et 5.</span><span class="sxs-lookup"><span data-stu-id="c26db-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="c26db-131">Enregistrez la page et chargez-le dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="c26db-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="c26db-132">Lorsque vous pointez sur les éléments de contrôle d’accès (vide), un effet JavaScript se produit : les modifications de contrôle d’accès.</span><span class="sxs-lookup"><span data-stu-id="c26db-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="c26db-133">Lorsque vous cliquez sur l’ensemble des étoiles, l’évaluation actuelle est conservée.</span><span class="sxs-lookup"><span data-stu-id="c26db-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="c26db-134">Enfin, lorsque vous envoyez le formulaire, le code côté serveur génère le classement sélectionné.</span><span class="sxs-lookup"><span data-stu-id="c26db-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="c26db-135">[![Création d’un système d’évaluation avec un minimum de code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c26db-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="c26db-136">Création d’un système d’évaluation avec un minimum de code ([cliquez pour afficher l’image en taille réelle](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c26db-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c26db-137">Next</span><span class="sxs-lookup"><span data-stu-id="c26db-137">Next</span></span>](creating-a-rating-control-vb.md)
