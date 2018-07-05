---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Contrôle dynamique des Animations UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Pour le contenu d’un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 79a256f5d5c62123184beffce3365ade0ac13e46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365939"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="d4eff-104">Contrôle dynamique des Animations UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="d4eff-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="d4eff-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d4eff-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d4eff-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d4eff-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="d4eff-107">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="d4eff-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d4eff-108">Pour le contenu d’un UpdatePanel, il existe un extendeur spéciaux qui s’appuie fortement sur l’infrastructure d’animation : UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="d4eff-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="d4eff-109">Il est également utilisable avec des déclencheurs UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="d4eff-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="d4eff-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="d4eff-110">Overview</span></span>

<span data-ttu-id="d4eff-111">Le contrôle d’Animation dans ASP.NET AJAX Control Toolkit n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="d4eff-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d4eff-112">Pour le contenu d’un `UpdatePanel`, un extendeur spécial existe qui s’appuie fortement sur l’infrastructure d’animation : `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="d4eff-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="d4eff-113">Il peut également fonctionner conjointement avec `UpdatePanel` déclencheurs.</span><span class="sxs-lookup"><span data-stu-id="d4eff-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="d4eff-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="d4eff-114">Steps</span></span>

<span data-ttu-id="d4eff-115">La première étape consiste comme d’habitude à inclure le `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="d4eff-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="d4eff-116">L’animation dans ce scénario s’appliqueront à un affichage de l’heure actuelle.</span><span class="sxs-lookup"><span data-stu-id="d4eff-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="d4eff-117">Ces informations peuvent être écrits dans une étiquette à l’aide de la `Page_Load()` (méthode), ou (par souci de simplicité) le code inline suivant est utilisé :</span><span class="sxs-lookup"><span data-stu-id="d4eff-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="d4eff-118">En outre, un bouton pour déclencher la mise à jour de l’heure est créé :</span><span class="sxs-lookup"><span data-stu-id="d4eff-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="d4eff-119">Ce code est ensuite placé dans le `<ContentTemplate>` section d’un `UpdatePanel` élément.</span><span class="sxs-lookup"><span data-stu-id="d4eff-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="d4eff-120">Le panneau `UpdateMode` attribut doit être défini sur `"Conditional"`, étant donné que seuls les déclencheurs peuvent mettre à jour le contenu du panneau.</span><span class="sxs-lookup"><span data-stu-id="d4eff-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="d4eff-121">Dans le `<Triggers>` section de la `UpdatePanel`, un déclencheur de publication (postback) asynchrone est créé et lié à la `Click` événements du bouton.</span><span class="sxs-lookup"><span data-stu-id="d4eff-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="d4eff-122">Par conséquent, si l’utilisateur clique sur le bouton, le `UpdatePanel` est actualisé.</span><span class="sxs-lookup"><span data-stu-id="d4eff-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="d4eff-123">Voici le balisage pour le `UpdatePanel` contrôle :</span><span class="sxs-lookup"><span data-stu-id="d4eff-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="d4eff-124">Enfin, le `UpdatePanelAnimationExtender` doit être configuré : définir le `TargetControlID` d’attribut à l’ID du panneau et définissez une animation au sein de l’extendeur.</span><span class="sxs-lookup"><span data-stu-id="d4eff-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="d4eff-125">Effet de fondu rend sens, ce qui crée une agréable visual mettant l’accent sur l’heure de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="d4eff-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="d4eff-126">Votre balisage d’extendeur peut ensuite ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="d4eff-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="d4eff-127">Exécutez le fichier dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="d4eff-127">Run the file in the browser.</span></span> <span data-ttu-id="d4eff-128">Chaque fois que vous cliquez sur le bouton, l’heure actuelle est indiqué dans le panneau de configuration, toujours fondu pour la durée d’une seconde.</span><span class="sxs-lookup"><span data-stu-id="d4eff-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="d4eff-129">[![L’heure actuelle est fondu](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d4eff-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="d4eff-130">L’heure actuelle est fondu ([cliquez pour afficher l’image en taille réelle](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d4eff-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d4eff-131">Précédent</span><span class="sxs-lookup"><span data-stu-id="d4eff-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
