---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Exécution de plusieurs Animations après l’autre (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il permet d’exécuter la chute...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 2317a029d4b12ba66d2d06e5012bb0bf892086a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807601"
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="2da02-104">Exécution de plusieurs Animations après l’autre (c#)</span><span class="sxs-lookup"><span data-stu-id="2da02-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="2da02-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2da02-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2da02-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2da02-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="2da02-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2da02-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2da02-108">Il permet d’exécuter plusieurs animations un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="2da02-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="2da02-109">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="2da02-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2da02-110">Il permet d’exécuter plusieurs animations un après l’autre.</span><span class="sxs-lookup"><span data-stu-id="2da02-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="2da02-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="2da02-111">Steps</span></span>

<span data-ttu-id="2da02-112">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="2da02-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="2da02-113">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="2da02-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="2da02-114">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="2da02-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="2da02-115">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="2da02-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="2da02-116">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="2da02-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="2da02-117">En règle générale, `<OnLoad>` accepte uniquement une animation.</span><span class="sxs-lookup"><span data-stu-id="2da02-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="2da02-118">Le framework d’Animation vous permet de joindre plusieurs animations en un seul via le `<Sequence>` élément.</span><span class="sxs-lookup"><span data-stu-id="2da02-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="2da02-119">Toutes les animations dans `<Sequence>` sont exécutées une après l’autre.</span><span class="sxs-lookup"><span data-stu-id="2da02-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="2da02-120">Voici l’un balisage possible pour le `AnimationExtender` contrôle, tout d’abord effectuer le panneau plus large et puis diminuant leur hauteur :</span><span class="sxs-lookup"><span data-stu-id="2da02-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="2da02-121">Lorsque vous exécutez ce script, le panneau première obtient ensuite petite et plus large.</span><span class="sxs-lookup"><span data-stu-id="2da02-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="2da02-122">[![Tout d’abord la largeur est augmentée.](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2da02-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="2da02-123">Tout d’abord la largeur est augmentée ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2da02-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="2da02-124">[![Puis diminue la hauteur](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2da02-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="2da02-125">Puis diminue la hauteur ([cliquez pour afficher l’image en taille réelle](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2da02-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2da02-126">[Précédent](executing-several-animations-at-the-same-time-cs.md)
> [Suivant](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2da02-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
