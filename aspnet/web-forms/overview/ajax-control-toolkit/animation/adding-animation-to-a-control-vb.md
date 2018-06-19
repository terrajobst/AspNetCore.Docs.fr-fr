---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Ajout d’une Animation à un contrôle (VB) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3da98e478c45213875b3829e51351d03571a05b8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870633"
---
<a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="b90af-104">Ajout d’une Animation à un contrôle (VB)</span><span class="sxs-lookup"><span data-stu-id="b90af-104">Adding Animation to a Control (VB)</span></span>
====================
<span data-ttu-id="b90af-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b90af-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b90af-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b90af-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="b90af-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="b90af-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b90af-108">Ce didacticiel montre comment configurer une animation de ce type.</span><span class="sxs-lookup"><span data-stu-id="b90af-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="b90af-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="b90af-109">Overview</span></span>

<span data-ttu-id="b90af-110">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="b90af-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b90af-111">Ce didacticiel montre comment configurer une animation de ce type.</span><span class="sxs-lookup"><span data-stu-id="b90af-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="b90af-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="b90af-112">Steps</span></span>

<span data-ttu-id="b90af-113">La première étape consiste comme d’habitude pour inclure la `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="b90af-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="b90af-114">L’animation dans ce scénario sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="b90af-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="b90af-115">La classe CSS associée pour le panneau de configuration définit une couleur d’arrière-plan et une largeur :</span><span class="sxs-lookup"><span data-stu-id="b90af-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="b90af-116">Ensuite, nous devons le `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="b90af-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="b90af-117">Après avoir entré un `ID` et classiques `runat="server"`, le `TargetControlID` attribut doit être défini pour le contrôle à animer dans notre cas, le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="b90af-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="b90af-118">L’animation entière est appliquée de façon déclarative, en utilisant une syntaxe XML, malheureusement n’est pas entièrement pris en charge par IntelliSense de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b90af-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="b90af-119">Le nœud racine est `<Animations>;` au sein de ce nœud, plusieurs événements sont autorisés pour déterminer quand l’ou les animations prennent place :</span><span class="sxs-lookup"><span data-stu-id="b90af-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="b90af-120">`OnClick` (clic de souris)</span><span class="sxs-lookup"><span data-stu-id="b90af-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="b90af-121">`OnHoverOut` (lorsque la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="b90af-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="b90af-122">`OnHoverOver` (lorsque la souris pointe sur un contrôle, l’arrêt du `OnHoverOut` animation)</span><span class="sxs-lookup"><span data-stu-id="b90af-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="b90af-123">`OnLoad` (lorsque la page a été chargée)</span><span class="sxs-lookup"><span data-stu-id="b90af-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="b90af-124">`OnMouseOut` (lorsque la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="b90af-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="b90af-125">`OnMouseOver` (lorsque la souris pointe sur un contrôle, ne pas l’arrêt du `OnMouseOut` animation)</span><span class="sxs-lookup"><span data-stu-id="b90af-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="b90af-126">Le framework est fourni avec un jeu d’animations, chacun d’eux représenté par son propre élément XML.</span><span class="sxs-lookup"><span data-stu-id="b90af-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="b90af-127">Voici une sélection :</span><span class="sxs-lookup"><span data-stu-id="b90af-127">Here is a selection:</span></span>

- <span data-ttu-id="b90af-128">`<Color>` (modification d’une couleur)</span><span class="sxs-lookup"><span data-stu-id="b90af-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="b90af-129">`<FadeIn>` (fondu dans)</span><span class="sxs-lookup"><span data-stu-id="b90af-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="b90af-130">`<FadeOut>` (fondu out)</span><span class="sxs-lookup"><span data-stu-id="b90af-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="b90af-131">`<Property>` (modification de propriété d’un contrôle)</span><span class="sxs-lookup"><span data-stu-id="b90af-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="b90af-132">`<Pulse>` (impulsions)</span><span class="sxs-lookup"><span data-stu-id="b90af-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="b90af-133">`<Resize>` (modification de la taille)</span><span class="sxs-lookup"><span data-stu-id="b90af-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="b90af-134">`<Scale>` (modification de la taille proportionnelle)</span><span class="sxs-lookup"><span data-stu-id="b90af-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="b90af-135">Dans cet exemple, le panneau de configuration doit disparaître. L’animation prennent 1,5 seconde (`Duration` attribut), affichage des 24 images (étapes de l’animation) par seconde (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="b90af-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="b90af-136">Voici le balisage complet pour le `AnimationExtender` contrôle :</span><span class="sxs-lookup"><span data-stu-id="b90af-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="b90af-137">Lorsque vous exécutez ce script, le panneau de configuration s’affiche et fondu dans un et demi secondes.</span><span class="sxs-lookup"><span data-stu-id="b90af-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="b90af-138">[![Le panneau est atténuant progressivement](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b90af-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="b90af-139">Le panneau est atténuant progressivement ([cliquez pour afficher l’image en taille réelle](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b90af-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b90af-140">[Précédent](dynamically-controlling-updatepanel-animations-cs.md)
> [Suivant](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b90af-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
