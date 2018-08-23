---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Liaison de données le contrôle Slider (VB) | Microsoft Docs
author: wenz
description: Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible de lier la position en cours...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 284a650d1b63ceed887deb3ddd639fe9fd27019f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836220"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="68ae3-104">Liaison de données le contrôle Slider (VB)</span><span class="sxs-lookup"><span data-stu-id="68ae3-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="68ae3-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="68ae3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="68ae3-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="68ae3-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="68ae3-107">Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="68ae3-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="68ae3-108">Il est possible de lier la position actuelle du curseur vers un autre contrôle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="68ae3-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="68ae3-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="68ae3-109">Overview</span></span>

<span data-ttu-id="68ae3-110">Le contrôle Slider dans AJAX Control Toolkit fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="68ae3-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="68ae3-111">Il est possible de lier la position actuelle du curseur vers un autre contrôle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="68ae3-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="68ae3-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="68ae3-112">Steps</span></span>

<span data-ttu-id="68ae3-113">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="68ae3-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="68ae3-114">Ensuite, ajoutez deux `TextBox` contrôles à la page.</span><span class="sxs-lookup"><span data-stu-id="68ae3-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="68ae3-115">Un est transformé en un contrôle slider graphique et autres celui contiendra la position du curseur.</span><span class="sxs-lookup"><span data-stu-id="68ae3-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="68ae3-116">L’étape suivante est déjà la dernière étape.</span><span class="sxs-lookup"><span data-stu-id="68ae3-116">The next step is already the final step.</span></span> <span data-ttu-id="68ae3-117">Le `SliderExtender` rend un curseur en dehors de la première zone de texte de contrôle à partir d’ASP.NET AJAX Control Toolkit et met à jour automatiquement la deuxième zone de texte lorsque le curseur de position des modifications.</span><span class="sxs-lookup"><span data-stu-id="68ae3-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="68ae3-118">Dans l’ordre pour que cela fonctionne, le `SliderExtender`de `TargetControlID` attribut doit être défini à l’ID de la première zone de texte ; le `BoundControlID` attribut doit être défini à l’ID de la deuxième zone de texte.</span><span class="sxs-lookup"><span data-stu-id="68ae3-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="68ae3-119">Comme vous pouvez le voir dans le navigateur, la liaison de données fonctionne dans les deux sens : entrant une nouvelle valeur dans la zone de texte des mises à jour la position du curseur.</span><span class="sxs-lookup"><span data-stu-id="68ae3-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="68ae3-120">Si vous apportez à la deuxième zone de texte en lecture seule, vous pouvez ajouter une protection faible pour le champ de texte afin qu’il soit plus difficile pour l’utilisateur à mettre à jour manuellement la valeur y.</span><span class="sxs-lookup"><span data-stu-id="68ae3-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="68ae3-121">[![Curseur et zone de texte sont synchronisés](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68ae3-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="68ae3-122">Curseur et zone de texte sont synchronisés ([cliquez pour afficher l’image en taille réelle](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="68ae3-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="68ae3-123">Précédent</span><span class="sxs-lookup"><span data-stu-id="68ae3-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
