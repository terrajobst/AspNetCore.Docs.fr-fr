---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animer un contrôle UpdatePanel (VB) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="dad29-104">Animer un contrôle UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="dad29-104">Animating an UpdatePanel Control (VB)</span></span>
====================
<span data-ttu-id="dad29-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dad29-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dad29-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="dad29-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="dad29-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="dad29-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dad29-108">Pour le contenu de UpdatePanel, un extendeur spécial existe qui s’appuie fortement sur le framework d’animation : UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="dad29-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="dad29-109">Ce didacticiel montre comment configurer une animation de ce type pour un UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="dad29-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="dad29-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="dad29-110">Overview</span></span>

<span data-ttu-id="dad29-111">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="dad29-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dad29-112">Pour le contenu d’un `UpdatePanel`, un extendeur spécial existe qui s’appuie fortement sur le framework d’animation : `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="dad29-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="dad29-113">Ce didacticiel montre comment configurer ce type d’une animation pour un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="dad29-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="dad29-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="dad29-114">Steps</span></span>

<span data-ttu-id="dad29-115">La première étape consiste comme d’habitude pour inclure la `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="dad29-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="dad29-116">L’animation dans ce scénario s’appliqueront à ASP.NET `Wizard` contrôle web résidant dans un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="dad29-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="dad29-117">Trois étapes (arbitraires) fournissent suffisamment options pour déclencher des publications (postback) :</span><span class="sxs-lookup"><span data-stu-id="dad29-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="dad29-118">Le balisage nécessaire pour le `UpdatePanelAnimationExtender` contrôle est très semblable à celui utilisé pour le `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="dad29-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="dad29-119">Dans le `TargetControlID` attribut nous fournir le `ID` de la `UpdatePanel` pour animer ; dans le `UpdatePanelAnimationExtender` (contrôle), le `<Animations>` élément contient le balisage XML pour les ou les animations.</span><span class="sxs-lookup"><span data-stu-id="dad29-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="dad29-120">Toutefois, il existe une différence : la quantité d’événements et gestionnaires d’événements est limitée par rapport à `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="dad29-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="dad29-121">Pour `UpdatePanels`, seulement deux d'entre elles existent :</span><span class="sxs-lookup"><span data-stu-id="dad29-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="dad29-122">`<OnUpdated>` Lorsque le contrôle UpdatePanel a été mis à jour</span><span class="sxs-lookup"><span data-stu-id="dad29-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="dad29-123">`<OnUpdating>` UpdatePanel démarrage de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="dad29-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="dad29-124">Dans ce scénario, le contenu de la `UpdatePanel` (après la publication) est en fondu.</span><span class="sxs-lookup"><span data-stu-id="dad29-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="dad29-125">Il s’agit de la balise nécessaire pour que :</span><span class="sxs-lookup"><span data-stu-id="dad29-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="dad29-126">Désormais chaque fois qu’une publication (postback) se produit dans le contrôle UpdatePanel, le nouveau contenu dans le panneau de configuration Fondu léger.</span><span class="sxs-lookup"><span data-stu-id="dad29-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="dad29-127">[![L’atténuation de l’étape suivante de l’Assistant](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dad29-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="dad29-128">L’étape suivante de l’Assistant est du fondu ([cliquez pour afficher l’image en taille réelle](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dad29-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dad29-129">[Précédent](changing-an-animation-using-client-side-code-vb.md)
> [Suivant](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dad29-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
