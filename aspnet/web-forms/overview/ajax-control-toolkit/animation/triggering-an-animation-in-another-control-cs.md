---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Déclenchement d’une Animation dans un autre contrôle (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, en lançant un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 37473217062b48a1e7829efcf07015be12e0c650
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375117"
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="35e64-104">Déclenchement d’une Animation dans un autre contrôle (c#)</span><span class="sxs-lookup"><span data-stu-id="35e64-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="35e64-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="35e64-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="35e64-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="35e64-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="35e64-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="35e64-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="35e64-108">En règle générale, le lancement d’une animation est déclenchée par l’interaction utilisateur avec le même contrôle.</span><span class="sxs-lookup"><span data-stu-id="35e64-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="35e64-109">Il est cependant également possible d’interagir avec un contrôle, puis d’animation à un autre contrôle.</span><span class="sxs-lookup"><span data-stu-id="35e64-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="35e64-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="35e64-110">Overview</span></span>

<span data-ttu-id="35e64-111">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="35e64-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="35e64-112">En règle générale, le lancement d’une animation est déclenchée par l’interaction utilisateur avec le même contrôle.</span><span class="sxs-lookup"><span data-stu-id="35e64-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="35e64-113">Il est cependant également possible d’interagir avec un contrôle, puis d’animation à un autre contrôle.</span><span class="sxs-lookup"><span data-stu-id="35e64-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="35e64-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="35e64-114">Steps</span></span>

<span data-ttu-id="35e64-115">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="35e64-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="35e64-116">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="35e64-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="35e64-117">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="35e64-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="35e64-118">Pour commencer à animer le panneau de configuration, un bouton HTML est utilisé.</span><span class="sxs-lookup"><span data-stu-id="35e64-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="35e64-119">Notez que `<input type="button" />` est favorisée sur `<asp:Button />` dans la mesure où nous ne voulons pas une publication (postback) lorsque l’utilisateur clique sur ce bouton.</span><span class="sxs-lookup"><span data-stu-id="35e64-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="35e64-120">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="35e64-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="35e64-121">Il est important de définir `TargetControlID` à l’ID du bouton (l’élément déclencher l’animation), pas à l’ID du panneau (l’élément en cours d’animation)</span><span class="sxs-lookup"><span data-stu-id="35e64-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="35e64-122">Dans le `<Animations>` nœud, les animations sur place comme d’habitude.</span><span class="sxs-lookup"><span data-stu-id="35e64-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="35e64-123">Afin de les mettre à modifier le panneau de configuration, pas le bouton, définissez la `AnimationTarget` attribut pour chaque élément d’animation dans `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="35e64-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="35e64-124">La valeur de `AnimationTarget` est l’ID du panneau, bien sûr.</span><span class="sxs-lookup"><span data-stu-id="35e64-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="35e64-125">De cette façon, les animations se produisent avec le panneau, pas avec le bouton de déclenchement.</span><span class="sxs-lookup"><span data-stu-id="35e64-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="35e64-126">Voici le `AnimationExtender` balisage pour ce scénario :</span><span class="sxs-lookup"><span data-stu-id="35e64-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="35e64-127">Notez l’ordre spécial dans lequel apparaissent les animations individuelles.</span><span class="sxs-lookup"><span data-stu-id="35e64-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="35e64-128">Tout d’abord, le bouton est désactivé une fois que l’animation s’exécute.</span><span class="sxs-lookup"><span data-stu-id="35e64-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="35e64-129">Dans la mesure où il existe aucune `AnimationTarget` d’attribut dans le `<EnableAction>` élément, cette animation est appliquée pour le contrôle d’origine : le bouton.</span><span class="sxs-lookup"><span data-stu-id="35e64-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="35e64-130">Les étapes de le deux animation sont effectués en (`<Parallel>` élément).</span><span class="sxs-lookup"><span data-stu-id="35e64-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="35e64-131">Les deux ont leur `AnimationTarget` les attributs définis pour `"Panel1"`, animer ainsi le panneau de configuration, et pas sur le bouton.</span><span class="sxs-lookup"><span data-stu-id="35e64-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="35e64-132">[![Un clic de souris sur le bouton démarre l’animation de panneau](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="35e64-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="35e64-133">Un clic de souris sur le bouton démarre l’animation du Panneau de configuration ([cliquez pour afficher l’image en taille réelle](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="35e64-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="35e64-134">[Précédent](disabling-actions-during-animation-cs.md)
> [Suivant](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="35e64-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
