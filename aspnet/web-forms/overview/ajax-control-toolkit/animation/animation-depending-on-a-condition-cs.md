---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animation selon une Condition (c#) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Si une animation est en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: b530239e76654bc68a8fa6ac900a20df1d5699b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="08d4b-104">Animation selon une Condition (c#)</span><span class="sxs-lookup"><span data-stu-id="08d4b-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="08d4b-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="08d4b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="08d4b-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="08d4b-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="08d4b-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="08d4b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="08d4b-108">Si une animation est exécutée ou non peut également dépendre une condition sous forme d’un code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08d4b-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="08d4b-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="08d4b-109">Overview</span></span>

<span data-ttu-id="08d4b-110">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="08d4b-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="08d4b-111">Si une animation est exécutée ou non peut également dépendre une condition sous forme d’un code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08d4b-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="08d4b-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="08d4b-112">Steps</span></span>

<span data-ttu-id="08d4b-113">Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="08d4b-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="08d4b-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="08d4b-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="08d4b-115">Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="08d4b-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="08d4b-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant une `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="08d4b-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="08d4b-117">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été complètement chargée.</span><span class="sxs-lookup"><span data-stu-id="08d4b-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="08d4b-118">Au lieu d’un des animations régulières, le `<Condition>` élément entre en jeu.</span><span class="sxs-lookup"><span data-stu-id="08d4b-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="08d4b-119">Le code JavaScript fourni comme valeur de la `ConditionScript` attribut est exécuté lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="08d4b-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="08d4b-120">Si sa valeur est true, l’animation est exécutée, sinon pas.</span><span class="sxs-lookup"><span data-stu-id="08d4b-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="08d4b-121">Le balisage suivant fournit deux animations, chacun d’eux en cours d’exécution de 50 % des cas à aléatoire.</span><span class="sxs-lookup"><span data-stu-id="08d4b-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="08d4b-122">Dans la mesure où il ne peut exister qu’une animation dans `<OnLoad>`, les deux `<Condition>` animations sont jointes à l’aide de la `<Sequence>` élément :</span><span class="sxs-lookup"><span data-stu-id="08d4b-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="08d4b-123">Notez que le signe inférieur à (`<`) dans le `ConditionScript` attribut doit être la séquence d’échappement ().</span><span class="sxs-lookup"><span data-stu-id="08d4b-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="08d4b-124">Lorsque vous exécutez ce script, aucune animation s’exécute, ou l’un des deux n’ou tous deux faire.</span><span class="sxs-lookup"><span data-stu-id="08d4b-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="08d4b-125">[![Le panneau est fondu sans redimensionnement, donc n’a pas de la deuxième animation s’exécute l'](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="08d4b-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="08d4b-126">Le panneau est fondu sans redimensionnement, donc n’a pas de la deuxième animation s’exécute le ([cliquez pour afficher l’image en taille réelle](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="08d4b-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="08d4b-127">[Précédent](executing-several-animations-after-each-other-cs.md)
> [Suivant](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="08d4b-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
