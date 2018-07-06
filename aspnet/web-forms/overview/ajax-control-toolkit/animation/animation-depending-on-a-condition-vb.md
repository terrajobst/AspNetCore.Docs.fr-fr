---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animation dépendant d’une Condition (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Si une animation est...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b815d7000d94ceec8864b29f0dddf0171344de1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836286"
---
<a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="9fa8b-104">Animation dépendant d’une Condition (VB)</span><span class="sxs-lookup"><span data-stu-id="9fa8b-104">Animation Depending On a Condition (VB)</span></span>
====================
<span data-ttu-id="9fa8b-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9fa8b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9fa8b-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9fa8b-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="9fa8b-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9fa8b-108">Si une animation est exécutée ou non peut également dépendre une condition sous forme de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="9fa8b-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="9fa8b-109">Overview</span></span>

<span data-ttu-id="9fa8b-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9fa8b-111">Si une animation est exécutée ou non peut également dépendre une condition sous forme de code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="9fa8b-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="9fa8b-112">Steps</span></span>

<span data-ttu-id="9fa8b-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="9fa8b-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="9fa8b-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="9fa8b-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="9fa8b-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="9fa8b-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="9fa8b-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="9fa8b-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="9fa8b-117">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="9fa8b-118">Au lieu d’un des animations régulières, le `<Condition>` élément entre en jeu.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="9fa8b-119">Le code JavaScript fourni comme valeur de la `ConditionScript` attribut est exécuté lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="9fa8b-120">Si sa valeur est true, l’animation est exécutée, sinon pas.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="9fa8b-121">Le balisage suivant fournit deux animations, chacun d’eux en cours d’exécution de 50 % des cas sur aléatoire.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="9fa8b-122">Dans la mesure où il ne peut exister qu’une animation dans `<OnLoad>`, les deux `<Condition>` animations sont jointes via la `<Sequence>` élément :</span><span class="sxs-lookup"><span data-stu-id="9fa8b-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="9fa8b-123">Notez que le signe inférieur à (`<`) dans le `ConditionScript` l’attribut doit être () avec séquence d’échappement.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="9fa8b-124">Lorsque vous exécutez ce script, aucune animation s’exécute, ou l’un des deux n’ou utilisent pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="9fa8b-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="9fa8b-125">[![Le panneau est fondu sans le redimensionner, donc n’a pas de l’exécution du deuxième animation, la première condition](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9fa8b-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="9fa8b-126">Le panneau est fondu sans le redimensionner, donc n’a pas de l’exécution du deuxième animation, la première condition ([cliquez pour afficher l’image en taille réelle](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9fa8b-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9fa8b-127">[Précédent](executing-several-animations-after-each-other-vb.md)
> [Suivant](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9fa8b-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
