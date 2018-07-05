---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Lancement d’une fenêtre contextuelle modale à partir du Code de serveur (c#) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Toutefois, certains scénarios nécessitent que t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: a13915f5748f8ad9b79fb787cc5e0682faa19388
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374089"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="c8c9a-104">Lancement d’une fenêtre contextuelle modale à partir du Code de serveur (c#)</span><span class="sxs-lookup"><span data-stu-id="c8c9a-104">Launching a Modal Popup Window from Server Code (C#)</span></span>
====================
<span data-ttu-id="c8c9a-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c8c9a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c8c9a-106">[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c8c9a-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="c8c9a-107">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c8c9a-108">Toutefois certains scénarios nécessitent que l’ouverture de la fenêtre contextuelle modale est déclenchée sur le côté serveur.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="c8c9a-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c8c9a-109">Overview</span></span>

<span data-ttu-id="c8c9a-110">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c8c9a-111">Toutefois certains scénarios nécessitent que l’ouverture de la fenêtre contextuelle modale est déclenchée sur le côté serveur.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="c8c9a-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="c8c9a-112">Steps</span></span>

<span data-ttu-id="c8c9a-113">Tout d’abord un contrôle web ASP.NET est requis pour illustrer comment le contrôle ModalPopup fonctionne.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="c8c9a-114">Ajouter un bouton de ce type dans le &lt;formulaire&gt; élément sur une nouvelle page :</span><span class="sxs-lookup"><span data-stu-id="c8c9a-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="c8c9a-115">Ensuite, vous devez le balisage de la fenêtre contextuelle que vous souhaitez créer.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="c8c9a-116">Définir en tant qu’un `<asp:Panel>` contrôler et assurez-vous qu’il inclut un contrôle bouton.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="c8c9a-117">Le contrôle ModalPopup offre les fonctionnalités pour rendre ce bouton Fermer la fenêtre contextuelle ; Sinon, il n’existe aucun moyen facile pour lui faire disparaître.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="c8c9a-118">Ensuite, ajoutez le contrôle ModalPopup d’ASP.NET AJAX Toolkit à la page.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="c8c9a-119">Définir des propriétés pour le bouton qui charge le contrôle, le bouton qui a fait disparaître et l’ID de la fenêtre contextuelle réelle.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="c8c9a-120">Comme avec toutes les pages web basées sur ASP.NET AJAX ; le Gestionnaire de Script est requis pour charger les bibliothèques JavaScript nécessaires pour les navigateurs cible différent :</span><span class="sxs-lookup"><span data-stu-id="c8c9a-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="c8c9a-121">Exécuter l’exemple dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-121">Run the example in the browser.</span></span> <span data-ttu-id="c8c9a-122">Lorsque vous cliquez sur le bouton, la fenêtre contextuelle modale s’affiche.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="c8c9a-123">Afin d’obtenir le même effet à l’aide de code côté serveur, un nouveau bouton est nécessaire :</span><span class="sxs-lookup"><span data-stu-id="c8c9a-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="c8c9a-124">Comme vous pouvez le voir, un clic sur le bouton génère une publication (postback) et exécute la `ServerButton_Click()` méthode sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="c8c9a-125">Dans cette méthode, une fonction JavaScript appelée `launchModal()` est exécutée pour être précis, la fonction JavaScript sera exécutée une fois que la page a été chargée :</span><span class="sxs-lookup"><span data-stu-id="c8c9a-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="c8c9a-126">Le travail de `launchModal()` consiste à afficher le ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="c8c9a-127">Le `launchModal()` fonction est exécutée une fois que la page HTML complète a été chargée.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="c8c9a-128">À ce moment-là, toutefois, l’infrastructure ASP.NET AJAX n’était pas entièrement chargé encore.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="c8c9a-129">Par conséquent, le `launchModal()` fonction définit simplement une variable de contrôle ModalPopup doit être affiché par la suite :</span><span class="sxs-lookup"><span data-stu-id="c8c9a-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="c8c9a-130">Le `pageLoad()` fonction JavaScript est une fonction spéciale qui est exécutée une fois ASP.NET AJAX a été entièrement chargé.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="c8c9a-131">Par conséquent nous ajouter du code à cette fonction pour afficher le contrôle ModalPopup, mais uniquement si `launchModal()` a été appelé avant :</span><span class="sxs-lookup"><span data-stu-id="c8c9a-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="c8c9a-132">Le `$find()` fonction recherche un élément nommé dans la page et attend l’ID côté serveur en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="c8c9a-133">Par conséquent, `$find("mpe")` retourne la représentation sous forme de client du contrôle ModalPopup ; son `show()` méthode permet de la fenêtre contextuelle s’affichent.</span><span class="sxs-lookup"><span data-stu-id="c8c9a-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="c8c9a-134">[![La fenêtre contextuelle modale s’affiche lorsque l’utilisateur clique sur un des boutons](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c8c9a-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="c8c9a-135">La fenêtre contextuelle modale s’affiche lorsque l’utilisateur clique sur un des boutons ([cliquez pour afficher l’image en taille réelle](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c8c9a-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8c9a-136">Next</span><span class="sxs-lookup"><span data-stu-id="c8c9a-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
