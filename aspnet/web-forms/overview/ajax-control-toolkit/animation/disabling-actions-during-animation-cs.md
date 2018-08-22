---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Désactivation d’Actions pendant une Animation (c#) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Il prend également en charge d’action...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834599"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="29bf8-104">Désactivation d’Actions pendant une Animation (c#)</span><span class="sxs-lookup"><span data-stu-id="29bf8-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="29bf8-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="29bf8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="29bf8-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="29bf8-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="29bf8-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="29bf8-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="29bf8-108">Il prend également en charge les actions, telles que les clics de souris.</span><span class="sxs-lookup"><span data-stu-id="29bf8-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="29bf8-109">Toutefois lorsqu’un clic de souris démarre une animation, il est souhaitable de désactiver les clics de souris lors de l’animation.</span><span class="sxs-lookup"><span data-stu-id="29bf8-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="29bf8-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="29bf8-110">Overview</span></span>

<span data-ttu-id="29bf8-111">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="29bf8-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="29bf8-112">Il prend également en charge les actions, telles que les clics de souris.</span><span class="sxs-lookup"><span data-stu-id="29bf8-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="29bf8-113">Toutefois lorsqu’un clic de souris démarre une animation, il est souhaitable de désactiver les clics de souris lors de l’animation.</span><span class="sxs-lookup"><span data-stu-id="29bf8-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="29bf8-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="29bf8-114">Steps</span></span>

<span data-ttu-id="29bf8-115">Tout d’abord inclure le `ScriptManager` dans la page ; ensuite, la bibliothèque AJAX ASP.NET est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="29bf8-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="29bf8-116">L’animation sera appliquée à un bouton HTML comme suit :</span><span class="sxs-lookup"><span data-stu-id="29bf8-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="29bf8-117">Notez qu’un contrôle HTML est utilisé au lieu d’un contrôle Web dans la mesure où nous ne voulons pas le bouton pour créer une publication (postback) ; Il est simplement lancer l’animation côté client pour nous.</span><span class="sxs-lookup"><span data-stu-id="29bf8-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="29bf8-118">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant un `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="29bf8-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="29bf8-119">Dans le `<Animations>` nœud, `<OnClick>` est l’élément approprié à gérer le clic de souris.</span><span class="sxs-lookup"><span data-stu-id="29bf8-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="29bf8-120">Toutefois, le bouton peut être activé lors de l’animation.</span><span class="sxs-lookup"><span data-stu-id="29bf8-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="29bf8-121">Le `<EnableAction>` peut prendre en charge de cet élément.</span><span class="sxs-lookup"><span data-stu-id="29bf8-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="29bf8-122">Paramètre `Enabled="false"` désactive le bouton dans le cadre de l’animation.</span><span class="sxs-lookup"><span data-stu-id="29bf8-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="29bf8-123">Étant donné que nous utilisons plusieurs animations individuelles (en désactivant le bouton et les animations réelles), le `<Parallel>` élément est requis pour les animations uniques ensemble en une seule de type glue.</span><span class="sxs-lookup"><span data-stu-id="29bf8-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="29bf8-124">Voici le balisage complet pour `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="29bf8-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="29bf8-125">Il serait également possible d’activer de nouveau au bouton après l’animation, à l’aide de l’élément XML suivant à la fin de la liste :</span><span class="sxs-lookup"><span data-stu-id="29bf8-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="29bf8-126">Toutefois dans le scénario fourni cela serait inutile puisque le bouton Fondu et n’est pas visible à la fin de l’animation.</span><span class="sxs-lookup"><span data-stu-id="29bf8-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="29bf8-127">[![Le bouton est désactivé dès que l’animation s’exécute](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="29bf8-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="29bf8-128">Le bouton est désactivé dès que l’animation s’exécute ([cliquez pour afficher l’image en taille réelle](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="29bf8-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="29bf8-129">[Précédent](animating-in-response-to-user-interaction-cs.md)
> [Suivant](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="29bf8-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
