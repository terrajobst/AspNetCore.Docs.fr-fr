---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: "Déclenchement d’une Animation dans un autre contrôle (c#) | Documents Microsoft"
author: wenz
description: "Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, lancer un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d243eebc42b66f1e86b38a1b7531e527144ea7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="fc5d6-104">Déclenchement d’une Animation dans un autre contrôle (c#)</span><span class="sxs-lookup"><span data-stu-id="fc5d6-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="fc5d6-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fc5d6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fc5d6-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fc5d6-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="fc5d6-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fc5d6-108">En règle générale, lancer une animation est déclenchée par l’utilisateur avec le même contrôle.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="fc5d6-109">Il est également possible d’interagir avec un contrôle et des animations d’un autre contrôle.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="fc5d6-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="fc5d6-110">Overview</span></span>

<span data-ttu-id="fc5d6-111">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fc5d6-112">En règle générale, lancer une animation est déclenchée par l’utilisateur avec le même contrôle.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="fc5d6-113">Il est également possible d’interagir avec un contrôle et des animations d’un autre contrôle.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="fc5d6-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="fc5d6-114">Steps</span></span>

<span data-ttu-id="fc5d6-115">Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="fc5d6-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="fc5d6-116">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="fc5d6-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="fc5d6-117">Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="fc5d6-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="fc5d6-118">Pour démarrer l’animation du Panneau de configuration, un bouton HTML est utilisé.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="fc5d6-119">Notez que `<input type="button" />` est favorisée sur `<asp:Button />` , car nous ne voulons pas d’une publication (postback) lorsque l’utilisateur clique sur ce bouton.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="fc5d6-120">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant une `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="fc5d6-121">Il est important de définir `TargetControlID` à l’ID du bouton (l’élément déclenchant l’animation), pas à l’ID du panneau (l’élément en cours d’animation)</span><span class="sxs-lookup"><span data-stu-id="fc5d6-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="fc5d6-122">Dans la `<Animations>` nœud, place les animations comme d’habitude.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="fc5d6-123">Afin qu’elles soient de modifier le panneau de configuration, pas sur le bouton, définissez la `AnimationTarget` attribut pour chaque élément d’animation dans `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="fc5d6-124">La valeur de `AnimationTarget` est l’ID du panneau, bien sûr.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="fc5d6-125">De cette façon, les animations se produisent avec le panneau de configuration, pas avec le bouton de déclenchement.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="fc5d6-126">Voici le `AnimationExtender` balisage pour ce scénario :</span><span class="sxs-lookup"><span data-stu-id="fc5d6-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="fc5d6-127">Notez l’ordre spécial dans lequel les animations individuelles.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="fc5d6-128">Tout d’abord, le bouton est désactivé une fois l’animation s’exécute.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="fc5d6-129">Étant donné qu’aucun `AnimationTarget` d’attribut dans le `<EnableAction>` élément, cette animation est appliquée au contrôle d’origine : le bouton.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="fc5d6-130">Les étapes de le deux animation sont effectués en (`<Parallel>` élément).</span><span class="sxs-lookup"><span data-stu-id="fc5d6-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="fc5d6-131">Ont tous deux leur `AnimationTarget` attributs `"Panel1"`, par conséquent, l’animation le panneau de configuration, et non sur le bouton.</span><span class="sxs-lookup"><span data-stu-id="fc5d6-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="fc5d6-132">[![Un clic de souris sur le bouton démarre l’animation du Panneau de configuration](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc5d6-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="fc5d6-133">Un clic de souris sur le bouton démarre l’animation du Panneau de configuration ([cliquez pour afficher l’image en taille réelle](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fc5d6-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fc5d6-134">[Précédent](disabling-actions-during-animation-cs.md)
[Suivant](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="fc5d6-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
