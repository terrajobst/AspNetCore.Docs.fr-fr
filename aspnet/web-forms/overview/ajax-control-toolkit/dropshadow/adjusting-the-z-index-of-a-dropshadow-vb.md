---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: "Ajustement de l’Index Z d’un DropShadow (VB) | Documents Microsoft"
author: wenz
description: "Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée. Toutefois cette ombre parfois est en conflit avec d’autres contrôles, d’insta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 844ea00c2ef1c974aa72c7dd627819b0429d612e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="a26d3-104">Ajustement de l’Index Z d’un DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="a26d3-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="a26d3-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a26d3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a26d3-106">[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a26d3-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="a26d3-107">Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="a26d3-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a26d3-108">Toutefois cette ombre parfois entre en conflit avec d’autres contrôles, par exemple le contrôle Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a26d3-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="a26d3-109">Lorsqu’une entrée de menu s’affiche, il apparaît derrière l’ombre.</span><span class="sxs-lookup"><span data-stu-id="a26d3-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="a26d3-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a26d3-110">Overview</span></span>

<span data-ttu-id="a26d3-111">Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="a26d3-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a26d3-112">Toutefois cette ombre parfois entre en conflit avec d’autres contrôles, par exemple le contrôle Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a26d3-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="a26d3-113">Lorsqu’une entrée de menu s’affiche, il apparaît derrière l’ombre.</span><span class="sxs-lookup"><span data-stu-id="a26d3-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="a26d3-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="a26d3-114">Steps</span></span>

<span data-ttu-id="a26d3-115">Le code commence avec le panneau de configuration lui-même, contenant suffisamment de texte afin que le panneau de configuration contient suffisamment de texte pour l’effet soit visible :</span><span class="sxs-lookup"><span data-stu-id="a26d3-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="a26d3-116">Un autre panneau est placé directement avant la `panelShadow` Panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="a26d3-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="a26d3-117">Il contient un menu avec l’orientation horizontale afin que les entrées de menu s’affiche au-dessus (ou plutôt : sous) le `dropShadow` Panneau de configuration) :</span><span class="sxs-lookup"><span data-stu-id="a26d3-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="a26d3-118">Ensuite, la `DropShadowExtender` est ajouté pour étendre le `panelShadow` Panneau de configuration avec un effet d’ombre portée :</span><span class="sxs-lookup"><span data-stu-id="a26d3-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="a26d3-119">Enfin, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle à utiliser :</span><span class="sxs-lookup"><span data-stu-id="a26d3-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="a26d3-120">Lorsque vous exécutez ce script, les entrées de menu s’affichent sous le panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="a26d3-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="a26d3-121">Toutefois le menu utilise la classe CSS `panel` où vous devez définir deux choses à faire apparaître devant l’autre panneau les éléments :</span><span class="sxs-lookup"><span data-stu-id="a26d3-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="a26d3-122">Positionnement relatif</span><span class="sxs-lookup"><span data-stu-id="a26d3-122">Relative positioning</span></span>
- <span data-ttu-id="a26d3-123">Un z-index positif</span><span class="sxs-lookup"><span data-stu-id="a26d3-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="a26d3-124">Ensuite, la `DropShadowExtender` contrôle ne sont pas en conflit plus avec le contrôle de Menu.</span><span class="sxs-lookup"><span data-stu-id="a26d3-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="a26d3-125">[![Avant : L’entrée de menu n’est pas visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a26d3-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="a26d3-126">Avant : L’entrée de menu n’est pas visible ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a26d3-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="a26d3-127">[![Après : L’entrée de menu s’affiche](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a26d3-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="a26d3-128">Après : L’entrée de menu s’affiche ([cliquez pour afficher l’image en taille réelle](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a26d3-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a26d3-129">[Précédent](manipulating-dropshadow-properties-from-client-code-cs.md)
[Suivant](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a26d3-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
