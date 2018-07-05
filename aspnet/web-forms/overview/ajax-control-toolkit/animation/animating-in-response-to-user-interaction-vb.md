---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animation en réponse à une Interaction utilisateur (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent étoiles...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a118d3dc44dd74780fafd00d139160f8fc3bc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366875"
---
<a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="c7afa-104">Animation en réponse à une Interaction utilisateur (VB)</span><span class="sxs-lookup"><span data-stu-id="c7afa-104">Animating in Response To User Interaction (VB)</span></span>
====================
<span data-ttu-id="c7afa-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c7afa-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c7afa-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c7afa-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="c7afa-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="c7afa-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c7afa-108">Les animations peuvent démarrer automatiquement, ou peuvent être déclenchées par l’utilisateur, par exemple, en cliquant avec la souris.</span><span class="sxs-lookup"><span data-stu-id="c7afa-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="c7afa-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c7afa-109">Overview</span></span>

<span data-ttu-id="c7afa-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="c7afa-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c7afa-111">Les animations peuvent démarrer automatiquement, ou peuvent être déclenchées par l’utilisateur, par exemple, en cliquant avec la souris.</span><span class="sxs-lookup"><span data-stu-id="c7afa-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="c7afa-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="c7afa-112">Steps</span></span>

<span data-ttu-id="c7afa-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="c7afa-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="c7afa-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="c7afa-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="c7afa-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="c7afa-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="c7afa-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="c7afa-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="c7afa-117">Dans le `<Animations>` nœud, il existe cinq façons de démarrer l’animation par le biais de l’interaction utilisateur (l’élément manquant est `<OnLoad>` qui est exécuté une fois que la page entière a été entièrement chargée) :</span><span class="sxs-lookup"><span data-stu-id="c7afa-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="c7afa-118">`<OnClick>` (clic de souris sur le contrôle)</span><span class="sxs-lookup"><span data-stu-id="c7afa-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="c7afa-119">`<OnHoverOut>` (la souris s’écarte du contrôle)</span><span class="sxs-lookup"><span data-stu-id="c7afa-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="c7afa-120">`<OnHoverOver>` (la souris pointe sur un contrôle, l’arrêt du `<OnHoverOut>` animation)</span><span class="sxs-lookup"><span data-stu-id="c7afa-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="c7afa-121">`<OnMouseOut>` (la souris quitte un contrôle)</span><span class="sxs-lookup"><span data-stu-id="c7afa-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="c7afa-122">`<OnMouseOver>` (la souris pointe sur un contrôle, ne pas l’arrêt du `<OnMouseOut>` animation)</span><span class="sxs-lookup"><span data-stu-id="c7afa-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="c7afa-123">Dans ce scénario, `<OnClick>` est utilisé.</span><span class="sxs-lookup"><span data-stu-id="c7afa-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="c7afa-124">Lorsque l’utilisateur clique sur le panneau de configuration, il est redimensionné et fondu en même temps.</span><span class="sxs-lookup"><span data-stu-id="c7afa-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


<span data-ttu-id="c7afa-125">[![Un clic de souris démarre l’animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c7afa-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="c7afa-126">Un clic de souris démarre l’animation ([cliquez pour afficher l’image en taille réelle](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c7afa-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c7afa-127">[Précédent](picking-one-animation-out-of-a-list-vb.md)
> [Suivant](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c7afa-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
