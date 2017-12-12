---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: "À l’aide de TextBoxWatermark avec des contrôles de Validation (VB) | Documents Microsoft"
author: wenz
description: "Le contrôle TextBoxWatermark dans la boîte à outils de contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique dans la zone, il vous..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 09236481b6e51cc22a4034aa22e7c491ce27a510
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="02a47-104">À l’aide de TextBoxWatermark avec des contrôles de Validation (VB)</span><span class="sxs-lookup"><span data-stu-id="02a47-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="02a47-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="02a47-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="02a47-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="02a47-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="02a47-107">Le contrôle TextBoxWatermark dans la boîte à outils de contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone.</span><span class="sxs-lookup"><span data-stu-id="02a47-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="02a47-108">Lorsqu’un utilisateur clique dans la zone, elle est vidée.</span><span class="sxs-lookup"><span data-stu-id="02a47-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="02a47-109">Si l’utilisateur quitte la zone de sans saisie de texte, le texte prérempli s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="02a47-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="02a47-110">Il peut entrer en conflit avec les contrôles de Validation sur la même page, mais ces problèmes peuvent être surmontés.</span><span class="sxs-lookup"><span data-stu-id="02a47-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="02a47-111">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="02a47-111">Overview</span></span>

<span data-ttu-id="02a47-112">Le `TextBoxWatermark` contrôle dans la boîte à outils de contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone.</span><span class="sxs-lookup"><span data-stu-id="02a47-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="02a47-113">Lorsqu’un utilisateur clique dans la zone, elle est vidée.</span><span class="sxs-lookup"><span data-stu-id="02a47-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="02a47-114">Si l’utilisateur quitte la zone de sans saisie de texte, le texte prérempli s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="02a47-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="02a47-115">Il peut entrer en conflit avec les contrôles de Validation sur la même page, mais ces problèmes peuvent être surmontés.</span><span class="sxs-lookup"><span data-stu-id="02a47-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="02a47-116">Étapes</span><span class="sxs-lookup"><span data-stu-id="02a47-116">Steps</span></span>

<span data-ttu-id="02a47-117">Le programme d’installation de base de l’exemple est le suivant : un `TextBox` contrôle apparaît à l’aide un `TextBoxWatermarkExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="02a47-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="02a47-118">Un bouton déclenche une publication (postback) et sera utilisé ultérieurement pour déclencher les contrôles de validation sur la page.</span><span class="sxs-lookup"><span data-stu-id="02a47-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="02a47-119">En outre, un `ScriptManager` contrôle est requis pour initialiser ASP.NET AJAX :</span><span class="sxs-lookup"><span data-stu-id="02a47-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="02a47-120">Ajoutez maintenant un `RequiredFieldValidator` contrôle qui vérifie si le champ n’est texte quand le formulaire est envoyé.</span><span class="sxs-lookup"><span data-stu-id="02a47-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="02a47-121">Le `InitialValue` propriété du validateur doit être définie sur la même valeur que celle qui est utilisée dans le `TextBoxWatermarkExtender` contrôle : lorsque le formulaire est envoyé, la valeur d’une zone de texte sans modification est la valeur de filigrane dans celui-ci :</span><span class="sxs-lookup"><span data-stu-id="02a47-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="02a47-122">Toutefois, il existe un problème avec cette approche : si le client désactive JavaScript, le champ de texte n’est pas donc prérempli avec le texte de filigrane, le `RequiredFieldValidator` ne déclenche pas d’un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="02a47-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="02a47-123">Par conséquent, un deuxième `RequiredFieldValidator` contrôle est requise qui vérifie une zone de texte vide (l’omission de la `InitialValue` attribut).</span><span class="sxs-lookup"><span data-stu-id="02a47-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="02a47-124">Étant donné que les deux validateurs utilisent `Display` = `"Dynamic"`, l’utilisateur final ne peut pas distinguer l’apparence visuelle parmi les deux validateurs a été déclenché ; au lieu de cela, il semblerait qu’un seul d'entre eux.</span><span class="sxs-lookup"><span data-stu-id="02a47-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="02a47-125">Enfin, ajoutez du code côté serveur pour le texte dans le champ de sortie si aucun validateur n’émis un message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="02a47-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="02a47-126">[![Le programme de validation indique que la taille que le champ n’est pas de texte](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="02a47-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="02a47-127">Le programme de validation indique que la taille que le champ n’est pas de texte ([cliquez pour afficher l’image en taille réelle](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="02a47-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="02a47-128">Précédent</span><span class="sxs-lookup"><span data-stu-id="02a47-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
