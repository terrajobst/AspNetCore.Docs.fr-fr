---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Exécution de plusieurs Animations après l’autre (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter la chute...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 89412c078bbe40f06d31327d0a17bf3ea8bc8314
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831134"
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="93ebd-104">Exécution de plusieurs Animations après l’autre (VB)</span><span class="sxs-lookup"><span data-stu-id="93ebd-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="93ebd-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="93ebd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="93ebd-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="93ebd-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="93ebd-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="93ebd-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="93ebd-108">Il permet d’exécuter plusieurs animations un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="93ebd-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="93ebd-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="93ebd-109">Overview</span></span>

<span data-ttu-id="93ebd-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="93ebd-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="93ebd-111">Il permet d’exécuter plusieurs animations un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="93ebd-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="93ebd-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="93ebd-112">Steps</span></span>

<span data-ttu-id="93ebd-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="93ebd-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="93ebd-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="93ebd-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="93ebd-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="93ebd-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="93ebd-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="93ebd-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="93ebd-117">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="93ebd-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="93ebd-118">En règle générale, `<OnLoad>` accepte uniquement une animation.</span><span class="sxs-lookup"><span data-stu-id="93ebd-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="93ebd-119">Le framework d’Animation vous permet de joindre plusieurs animations en un seul via le `<Sequence>` élément.</span><span class="sxs-lookup"><span data-stu-id="93ebd-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="93ebd-120">Toutes les animations dans `<Sequence>` sont exécutées une après l’autre.</span><span class="sxs-lookup"><span data-stu-id="93ebd-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="93ebd-121">Voici l’un balisage possible pour le `AnimationExtender` contrôle, tout d’abord effectuer le panneau plus large et puis diminuant leur hauteur :</span><span class="sxs-lookup"><span data-stu-id="93ebd-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="93ebd-122">Lorsque vous exécutez ce script, le panneau première obtient ensuite petite et plus large.</span><span class="sxs-lookup"><span data-stu-id="93ebd-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="93ebd-123">[![Tout d’abord la largeur est augmentée.](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="93ebd-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="93ebd-124">Tout d’abord la largeur est augmentée ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="93ebd-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="93ebd-125">[![Puis diminue la hauteur](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="93ebd-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="93ebd-126">Puis diminue la hauteur ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="93ebd-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="93ebd-127">[Précédent](executing-several-animations-at-the-same-time-vb.md)
> [Suivant](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="93ebd-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
