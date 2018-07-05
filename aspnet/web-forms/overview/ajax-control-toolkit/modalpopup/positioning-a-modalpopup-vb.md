---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Positionnement d’un ModalPopup (VB) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Toutefois le contrôle n’offre pas un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1578a1bd6d5e3b595eba526552b8723daefb2ba1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387830"
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="d7f23-104">Positionnement d’un ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="d7f23-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="d7f23-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d7f23-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d7f23-106">[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d7f23-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="d7f23-107">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="d7f23-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d7f23-108">Toutefois le contrôle n’offre pas une fonctionnalité intégrée pour positionner la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="d7f23-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="d7f23-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="d7f23-109">Overview</span></span>

<span data-ttu-id="d7f23-110">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="d7f23-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d7f23-111">Toutefois le contrôle n’offre pas une fonctionnalité intégrée pour positionner la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="d7f23-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="d7f23-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="d7f23-112">Steps</span></span>

<span data-ttu-id="d7f23-113">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="d7f23-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="d7f23-114">contrôle doit être placé n’importe où sur la page (mais dans les limites du `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="d7f23-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="d7f23-115">Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale.</span><span class="sxs-lookup"><span data-stu-id="d7f23-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="d7f23-116">Un bouton est utilisé pour fermer la fenêtre contextuelle :</span><span class="sxs-lookup"><span data-stu-id="d7f23-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="d7f23-117">Chaque fois que la fenêtre contextuelle s’affiche, il doit être positionné sur un lieu donné dans la page.</span><span class="sxs-lookup"><span data-stu-id="d7f23-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="d7f23-118">Pour cette tâche, une fonction de JavaScript côté client est créée.</span><span class="sxs-lookup"><span data-stu-id="d7f23-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="d7f23-119">Il essaie tout d’abord accéder au panneau.</span><span class="sxs-lookup"><span data-stu-id="d7f23-119">It first tries to access the panel.</span></span> <span data-ttu-id="d7f23-120">Si elle réussit, la position du panneau est définie à l’aide de CSS et JavaScript (modification de la position de la fenêtre contextuelle à sera).</span><span class="sxs-lookup"><span data-stu-id="d7f23-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="d7f23-121">Toutefois, le `ModalPopupExtender` contrôle tente également de positionner la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="d7f23-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="d7f23-122">Par conséquent, le code JavaScript positionne à plusieurs reprises de la fenêtre contextuelle, chaque dixième de seconde.</span><span class="sxs-lookup"><span data-stu-id="d7f23-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="d7f23-123">Comme vous pouvez le voir, la valeur de retour de la `setTimeout()` méthode JavaScript est enregistré dans une variable globale.</span><span class="sxs-lookup"><span data-stu-id="d7f23-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="d7f23-124">Cela permet d’arrêter le positionnement répétées de la fenêtre contextuelle à la demande, à l’aide de la `clearTimeout()` méthode :</span><span class="sxs-lookup"><span data-stu-id="d7f23-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="d7f23-125">Maintenant, tout ce qui reste à faire consiste à appeler ces fonctions dès que possible le navigateur.</span><span class="sxs-lookup"><span data-stu-id="d7f23-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="d7f23-126">Le `movePanel()` fonction JavaScript doit être appelée lorsque le bouton qui déclenche le panneau :</span><span class="sxs-lookup"><span data-stu-id="d7f23-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="d7f23-127">Et le `stopMoving()` fonction qu’intervient la fermeture de la fenêtre contextuelle peut être déclenchée dans le `ModalPopupExtender` contrôle :</span><span class="sxs-lookup"><span data-stu-id="d7f23-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="d7f23-128">[![La fenêtre contextuelle modale s’affiche à la position désignée](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d7f23-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="d7f23-129">La fenêtre contextuelle modale s’affiche à la position désignée ([cliquez pour afficher l’image en taille réelle](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d7f23-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d7f23-130">Précédent</span><span class="sxs-lookup"><span data-stu-id="d7f23-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
