---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Utilisation de plusieurs contrôles de fenêtre contextuelle (c#) | Microsoft Docs
author: wenz
description: L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Il est également possible d’utiliser m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cf7f929b696240e6805a74fb576ad56738491fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825766"
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="5ce1b-104">Utilisation de plusieurs contrôles de fenêtre contextuelle (c#)</span><span class="sxs-lookup"><span data-stu-id="5ce1b-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="5ce1b-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5ce1b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5ce1b-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5ce1b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="5ce1b-107">L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5ce1b-108">Il est également possible d’utiliser plus d’un contrôle popup sur une seule page.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="5ce1b-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="5ce1b-109">Overview</span></span>

<span data-ttu-id="5ce1b-110">L’extendeur PopupControl dans les outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="5ce1b-111">Il est également possible d’utiliser plus d’un contrôle popup sur une seule page.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="5ce1b-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="5ce1b-112">Steps</span></span>

<span data-ttu-id="5ce1b-113">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="5ce1b-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="5ce1b-114">Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="5ce1b-115">Dans le scénario en cours, le panneau contient un `Calendar` contrôle.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="5ce1b-116">Afin d’éviter les actualisations de la page a provoqué par des publications (postback) du calendrier, le panneau est placé dans un `UpdatePanel` contrôle :</span><span class="sxs-lookup"><span data-stu-id="5ce1b-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="5ce1b-117">La page contient également deux zones de texte.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-117">The page also contains two text boxes.</span></span> <span data-ttu-id="5ce1b-118">Chaque zone de texte, la fenêtre de calendrier apparaît une fois que la zone de texte est activée.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="5ce1b-119">À présent étendre les deux zones de texte avec un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="5ce1b-120">Le `TargetControlID` attribut fournit l’ID du contrôle lié à l’extendeur.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="5ce1b-121">Le `PopupControlID` attribut contient l’ID du Panneau de menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="5ce1b-122">Dans ce cas, les deux extendeurs affichent le même panneau, mais différents panneaux est également possibles.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="5ce1b-123">Chaque fois que vous cliquez dans un champ de texte, un calendrier apparaît désormais sous le champ, ce qui vous permet de sélectionner une date.</span><span class="sxs-lookup"><span data-stu-id="5ce1b-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="5ce1b-124">(Retour de la date sélectionnée dans les zones de texte sera développée dans un autre didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="5ce1b-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="5ce1b-125">[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5ce1b-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="5ce1b-126">Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5ce1b-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5ce1b-127">Next</span><span class="sxs-lookup"><span data-stu-id="5ce1b-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
