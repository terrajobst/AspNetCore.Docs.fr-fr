---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: "L’exécution de plusieurs Animations après les autres (c#) | Documents Microsoft"
author: wenz
description: "Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter la chute..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d4322690132fe3829e3454f0aa7ff38acd8eb04
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="3a0f3-104">L’exécution de plusieurs Animations après les autres (c#)</span><span class="sxs-lookup"><span data-stu-id="3a0f3-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="3a0f3-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3a0f3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3a0f3-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3a0f3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="3a0f3-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3a0f3-108">Il permet d’exécuter plusieurs animations un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="3a0f3-109">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3a0f3-110">Il permet d’exécuter plusieurs animations un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="3a0f3-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="3a0f3-111">Steps</span></span>

<span data-ttu-id="3a0f3-112">Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="3a0f3-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="3a0f3-113">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="3a0f3-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="3a0f3-114">Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="3a0f3-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="3a0f3-115">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant une `ID`, le `TargetControlID` attribut et le texte obligatoire`runat="server":`</span><span class="sxs-lookup"><span data-stu-id="3a0f3-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="3a0f3-116">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été complètement chargée.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="3a0f3-117">En règle générale, `<OnLoad>` accepte uniquement une animation.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="3a0f3-118">Le framework d’Animation vous permet de joindre plusieurs animations en un seul via le `<Sequence>` élément.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="3a0f3-119">Toutes les animations dans `<Sequence>` sont exécutées une après l’autre.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="3a0f3-120">Voici l’un balisage possible pour le `AnimationExtender` contrôle, la création du Panneau de configuration plus large et puis réduire sa hauteur :</span><span class="sxs-lookup"><span data-stu-id="3a0f3-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="3a0f3-121">Lorsque vous exécutez ce script, le panneau première obtient plus large et puis plus petits.</span><span class="sxs-lookup"><span data-stu-id="3a0f3-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="3a0f3-122">[![Tout d’abord la largeur est augmentée.](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3a0f3-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="3a0f3-123">Tout d’abord la largeur est augmentée ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3a0f3-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="3a0f3-124">[![Puis la hauteur est diminuée.](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="3a0f3-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="3a0f3-125">Puis la diminution de la hauteur ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3a0f3-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3a0f3-126">[Précédent](executing-several-animations-at-the-same-time-cs.md)
[Suivant](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="3a0f3-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
