---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "À l’aide de plusieurs contrôles de fenêtre contextuelle (c#) | Documents Microsoft"
author: wenz
description: "L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Il est également possible d’utiliser m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="99077-104">À l’aide de plusieurs contrôles de fenêtre contextuelle (c#)</span><span class="sxs-lookup"><span data-stu-id="99077-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="99077-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="99077-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="99077-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="99077-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="99077-107">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="99077-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="99077-108">Il est également possible d’utiliser plus d’un contrôle de menu contextuel sur une page.</span><span class="sxs-lookup"><span data-stu-id="99077-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="99077-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="99077-109">Overview</span></span>

<span data-ttu-id="99077-110">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="99077-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="99077-111">Il est également possible d’utiliser plus d’un contrôle de menu contextuel sur une page.</span><span class="sxs-lookup"><span data-stu-id="99077-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="99077-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="99077-112">Steps</span></span>

<span data-ttu-id="99077-113">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="99077-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="99077-114">Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="99077-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="99077-115">Dans le scénario en cours, le panneau de configuration contient un `Calendar` contrôle.</span><span class="sxs-lookup"><span data-stu-id="99077-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="99077-116">Afin d’éviter l’actualisation de la page a provoqué par les publications du calendrier, le panneau de configuration est placé dans un `UpdatePanel` contrôle :</span><span class="sxs-lookup"><span data-stu-id="99077-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="99077-117">La page contient également les deux zones de texte.</span><span class="sxs-lookup"><span data-stu-id="99077-117">The page also contains two text boxes.</span></span> <span data-ttu-id="99077-118">Chaque zone de texte, le menu contextuel du calendrier apparaît une fois que la zone de texte est activée.</span><span class="sxs-lookup"><span data-stu-id="99077-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="99077-119">À présent étendre les deux zones de texte avec un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="99077-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="99077-120">Le `TargetControlID` attribut fournit l’ID du contrôle lié à l’extendeur.</span><span class="sxs-lookup"><span data-stu-id="99077-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="99077-121">Le `PopupControlID` attribut contient l’ID du Panneau de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="99077-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="99077-122">Dans ce cas, les deux extendeurs affichent le même volet, mais différents panneaux est également possibles.</span><span class="sxs-lookup"><span data-stu-id="99077-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="99077-123">Maintenant lorsque vous cliquez dans un champ de texte, un calendrier s’affiche sous le champ, ce qui vous permet de sélectionner une date.</span><span class="sxs-lookup"><span data-stu-id="99077-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="99077-124">(Pour obtenir la date sélectionnée dans les zones de texte est abordée dans un autre didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="99077-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="99077-125">[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="99077-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="99077-126">Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="99077-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="99077-127">Next</span><span class="sxs-lookup"><span data-stu-id="99077-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
