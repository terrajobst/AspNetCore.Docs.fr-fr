---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: "La gestion des publications (postback) à partir d’un contrôle Popup sans UpdatePanel (VB) | Documents Microsoft"
author: wenz
description: "L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Si une publication (postback) se produit dans su..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c4afee37eab33036e5e563e78f873275951700b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a><span data-ttu-id="45d3b-104">La gestion des publications (postback) à partir d’un contrôle Popup sans UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="45d3b-104">Handling Postbacks from A Popup Control Without an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="45d3b-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="45d3b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="45d3b-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="45d3b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)</span></span>

> <span data-ttu-id="45d3b-107">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="45d3b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="45d3b-108">Lorsqu’une publication (postback) se produit dans un panneau de ce type et il existe plusieurs panneaux dans la page, il est difficile de déterminer le panneau qui a été activé.</span><span class="sxs-lookup"><span data-stu-id="45d3b-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="45d3b-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="45d3b-109">Overview</span></span>

<span data-ttu-id="45d3b-110">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="45d3b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="45d3b-111">Lorsqu’une publication (postback) se produit dans un panneau de ce type et il existe plusieurs panneaux dans la page, il est difficile de déterminer le panneau qui a été activé.</span><span class="sxs-lookup"><span data-stu-id="45d3b-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="45d3b-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="45d3b-112">Steps</span></span>

<span data-ttu-id="45d3b-113">Lorsque vous utilisez un `PopupControl` avec une publication (postback), mais sans avoir un `UpdatePanel` sur la page, la boîte à outils de contrôle n’offre pas un moyen de déterminer quel élément client a déclenché la fenêtre contextuelle qui à son tour a provoqué la publication (postback).</span><span class="sxs-lookup"><span data-stu-id="45d3b-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="45d3b-114">Toutefois, une petite astuce fournit une solution de contournement pour ce scénario.</span><span class="sxs-lookup"><span data-stu-id="45d3b-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="45d3b-115">Tout d’abord, voici la configuration de base : deux zones de texte qui déclenchent le même message, un calendrier.</span><span class="sxs-lookup"><span data-stu-id="45d3b-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="45d3b-116">Deux `PopupControlExtenders` rassembler les zones de texte et de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="45d3b-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="45d3b-117">L’idée de base consiste à ajouter un champ de formulaire masqué dans le &lt; `form` &gt; élément qui contient la zone de texte qui a lancé la fenêtre contextuelle :</span><span class="sxs-lookup"><span data-stu-id="45d3b-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="45d3b-118">Lorsque la page est chargée, le code JavaScript ajoute un gestionnaire d’événements pour les deux zones de texte : chaque fois que l’utilisateur clique sur une zone de texte, son nom est écrit dans le champ de formulaire masqué :</span><span class="sxs-lookup"><span data-stu-id="45d3b-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

<span data-ttu-id="45d3b-119">Dans le code côté serveur, la valeur du champ masqué doit être lues.</span><span class="sxs-lookup"><span data-stu-id="45d3b-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="45d3b-120">Étant donné que les champs de formulaire masqués sont très facile à manipuler, une approche d’autorisation pour valider la valeur masquée est requise.</span><span class="sxs-lookup"><span data-stu-id="45d3b-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="45d3b-121">Une fois que la zone de texte correct a été identifiée, la date du calendrier est écrit.</span><span class="sxs-lookup"><span data-stu-id="45d3b-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


<span data-ttu-id="45d3b-122">[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45d3b-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="45d3b-123">Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="45d3b-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="45d3b-124">[![En cliquant sur une date place dans la zone de texte](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="45d3b-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="45d3b-125">En cliquant sur une date place dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="45d3b-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="45d3b-126">Précédent</span><span class="sxs-lookup"><span data-stu-id="45d3b-126">Previous</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
