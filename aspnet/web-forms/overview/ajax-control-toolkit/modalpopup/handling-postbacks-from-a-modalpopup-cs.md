---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Gestion des publications (postback) à partir d’un ModalPopup (c#) | Microsoft Docs
author: wenz
description: Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client. Une attention particulière doit entreprendre lorsqu’un pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 9bcb1ad058b800f3f957cafff07a5a54b44178a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829304"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="c1506-104">Gestion des publications (postback) à partir d’un ModalPopup (c#)</span><span class="sxs-lookup"><span data-stu-id="c1506-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="c1506-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c1506-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c1506-106">[Télécharger le Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c1506-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="c1506-107">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="c1506-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c1506-108">Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="c1506-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="c1506-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c1506-109">Overview</span></span>

<span data-ttu-id="c1506-110">Le contrôle ModalPopup dans AJAX Control Toolkit offre un moyen simple de créer une contextuelle modale à l’aide de moyens de côté client.</span><span class="sxs-lookup"><span data-stu-id="c1506-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c1506-111">Une attention particulière doit prendre lorsqu’une publication (postback) est créé à partir de la fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="c1506-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="c1506-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="c1506-112">Steps</span></span>

<span data-ttu-id="c1506-113">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="c1506-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="c1506-114">Ensuite, ajoutez un panneau qui sert de la fenêtre contextuelle modale.</span><span class="sxs-lookup"><span data-stu-id="c1506-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="c1506-115">Là, l’utilisateur peut entrer un nom et une adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="c1506-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="c1506-116">Un bouton est utilisé pour fermer la fenêtre contextuelle et enregistrer les informations.</span><span class="sxs-lookup"><span data-stu-id="c1506-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="c1506-117">Notez que le `OnClick` attribut a la valeur afin qu’une publication (postback) se produit lorsqu’un clic sur ce bouton :</span><span class="sxs-lookup"><span data-stu-id="c1506-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="c1506-118">La page elle-même se compose de deux étiquettes exactement les mêmes informations : nom et adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="c1506-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="c1506-119">Un bouton est utilisé pour déclencher la fenêtre contextuelle modale :</span><span class="sxs-lookup"><span data-stu-id="c1506-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="c1506-120">Pour que la fenêtre contextuelle s’affiche, ajoutez le `ModalPopupExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="c1506-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="c1506-121">Définir le `PopupControlID` ID du panneau par d’attribut et `TargetControlID` à l’ID du bouton :</span><span class="sxs-lookup"><span data-stu-id="c1506-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="c1506-122">Maintenant chaque fois que le `Save` dans la fenêtre contextuelle modale avec le bouton est cliqué, côté serveur `SaveData()` méthode est exécutée.</span><span class="sxs-lookup"><span data-stu-id="c1506-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="c1506-123">Là, vous pouvez enregistrer les données entrées dans un magasin de données.</span><span class="sxs-lookup"><span data-stu-id="c1506-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="c1506-124">Par souci de simplicité, les nouvelles données sont tout simplement sorties dans l’étiquette :</span><span class="sxs-lookup"><span data-stu-id="c1506-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="c1506-125">En outre, les contrôles de zone de texte dans la fenêtre contextuelle modale doivent être remplis avec le nom et le courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="c1506-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="c1506-126">Toutefois cela est nécessaire uniquement lorsque aucune publication (postback) se produit.</span><span class="sxs-lookup"><span data-stu-id="c1506-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="c1506-127">En l’absence d’une publication (postback), la fonctionnalité de viewstate ASP.NET remplira automatiquement les zones de texte avec les valeurs appropriées.</span><span class="sxs-lookup"><span data-stu-id="c1506-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="c1506-128">[![La fenêtre contextuelle modale entraîne une publication](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c1506-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="c1506-129">La fenêtre contextuelle modale entraîne une publication ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c1506-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c1506-130">[Précédent](using-modalpopup-with-a-repeater-control-cs.md)
> [Suivant](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c1506-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
