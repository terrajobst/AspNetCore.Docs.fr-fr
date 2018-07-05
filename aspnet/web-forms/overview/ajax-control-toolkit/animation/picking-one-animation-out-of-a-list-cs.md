---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Sélection d’une liste (c#) une Animation | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Le framework utoriser également...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 4b0193f47697e072605ed8c01a37fbfa128eb0e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380744"
---
<a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="f3797-104">Sélection d’une liste (c#) une Animation</span><span class="sxs-lookup"><span data-stu-id="f3797-104">Picking One Animation Out Of a List (C#)</span></span>
====================
<span data-ttu-id="f3797-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f3797-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f3797-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f3797-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="f3797-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="f3797-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f3797-108">L’infrastructure permet également au programmeur de choisir une animation dans une liste d’animations, selon l’évaluation d’un code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f3797-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="f3797-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f3797-109">Overview</span></span>

<span data-ttu-id="f3797-110">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="f3797-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f3797-111">L’infrastructure permet également au programmeur de choisir une animation dans une liste d’animations, selon l’évaluation d’un code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f3797-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="f3797-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="f3797-112">Steps</span></span>

<span data-ttu-id="f3797-113">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="f3797-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="f3797-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="f3797-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="f3797-115">Dans la classe CSS associée pour le panneau, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau :</span><span class="sxs-lookup"><span data-stu-id="f3797-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="f3797-116">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="f3797-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="f3797-117">Dans le `<Animations>` nœud, utilisez `<OnLoad>` pour exécuter les animations, une fois que la page a été entièrement chargée.</span><span class="sxs-lookup"><span data-stu-id="f3797-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="f3797-118">Au lieu d’un des animations régulières, le `<Case>` élément entre en jeu.</span><span class="sxs-lookup"><span data-stu-id="f3797-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="f3797-119">La valeur de son attribut SelectScript est évaluée ; la valeur de retour doit être numérique.</span><span class="sxs-lookup"><span data-stu-id="f3797-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="f3797-120">En fonction de ce nombre, parmi les sous-animations dans &lt;cas&gt; est exécutée.</span><span class="sxs-lookup"><span data-stu-id="f3797-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="f3797-121">Par exemple, si SelectScript a la valeur 2, les outils de contrôle s’exécute l’animation troisième dans &lt;cas&gt; (comptage démarre à 0).</span><span class="sxs-lookup"><span data-stu-id="f3797-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="f3797-122">Le balisage suivant définit trois sous-animations : redimensionnement de la largeur, de redimensionnement de la hauteur et de fondu. Le code JavaScript (`Math.floor(3 * Math.random())`) puis sélectionne un nombre compris entre 0 et 2, afin qu’un des trois animations est exécuté :</span><span class="sxs-lookup"><span data-stu-id="f3797-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


<span data-ttu-id="f3797-123">[![Un des trois animations possible : le panneau obtient plus large](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f3797-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="f3797-124">Un des trois animations possible : le panneau obtient plus large ([cliquez pour afficher l’image en taille réelle](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f3797-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f3797-125">[Précédent](animation-depending-on-a-condition-cs.md)
> [Suivant](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f3797-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
