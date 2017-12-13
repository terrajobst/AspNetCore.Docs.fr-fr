---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: "Autoriser uniquement certains caractères dans une zone de texte (VB) | Documents Microsoft"
author: wenz
description: "Contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée d’utilisateur. Toutefois cela toujours n’empêche pas les utilisateurs de la saisie non valides..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: b41ec1dfda5d85c625026e1f1e1ecd7e190ee3ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="7a999-104">Autoriser uniquement certains caractères dans une zone de texte (VB)</span><span class="sxs-lookup"><span data-stu-id="7a999-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="7a999-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7a999-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7a999-106">[Télécharger le Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7a999-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="7a999-107">Contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7a999-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="7a999-108">Toutefois cela toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="7a999-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="7a999-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="7a999-109">Overview</span></span>

<span data-ttu-id="7a999-110">Contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7a999-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="7a999-111">Toutefois cela toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="7a999-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="7a999-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="7a999-112">Steps</span></span>

<span data-ttu-id="7a999-113">Les outils de contrôle ASP.NET AJAX contient le `FilteredTextBox` contrôle qui étend une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="7a999-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="7a999-114">Une fois activé, uniquement un certain jeu de caractères peut être entré dans le champ.</span><span class="sxs-lookup"><span data-stu-id="7a999-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="7a999-115">Pour ce faire, nous devons comme d’habitude ASP.NET AJAX `ScriptManager` qui charge les bibliothèques JavaScript qui sont également utilisés par les outils de contrôle ASP.NET AJAX :</span><span class="sxs-lookup"><span data-stu-id="7a999-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="7a999-116">Ensuite, nous avons besoin d’une zone de texte :</span><span class="sxs-lookup"><span data-stu-id="7a999-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="7a999-117">Enfin, le `FilteredTextBoxExtender` contrôle prend en charge de limiter les caractères que l’utilisateur est autorisé à taper.</span><span class="sxs-lookup"><span data-stu-id="7a999-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="7a999-118">Tout d’abord, définissez le `TargetControlID` d’attribut pour le `ID` de la `TextBox` contrôle.</span><span class="sxs-lookup"><span data-stu-id="7a999-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="7a999-119">Ensuite, choisissez une des `FilterType` valeurs :</span><span class="sxs-lookup"><span data-stu-id="7a999-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="7a999-120">`Custom`valeur par défaut ; Vous devez fournir une liste de caractères valides</span><span class="sxs-lookup"><span data-stu-id="7a999-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="7a999-121">`LowercaseLetters`lettres minuscules uniquement</span><span class="sxs-lookup"><span data-stu-id="7a999-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="7a999-122">`Numbers`uniquement des chiffres</span><span class="sxs-lookup"><span data-stu-id="7a999-122">`Numbers` digits only</span></span>
- <span data-ttu-id="7a999-123">`UppercaseLetters`uniquement des lettres majuscules</span><span class="sxs-lookup"><span data-stu-id="7a999-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="7a999-124">Si le `Custom FilterType` est utilisé, le `ValidChars` propriété doit être défini et fournir une liste de caractères qui peuvent être tapés.</span><span class="sxs-lookup"><span data-stu-id="7a999-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="7a999-125">La façon dont : Si vous essayez de coller du texte dans la zone de texte, tous les caractères non valides sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="7a999-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="7a999-126">Voici le balisage de la `FilteredTextBoxExtender` contrôle qui permet uniquement de chiffres (ce qui aurait également été possible avec `FilterType="Numbers"`) :</span><span class="sxs-lookup"><span data-stu-id="7a999-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="7a999-127">Exécutez la page, puis réessayez d’entrer une lettre si JavaScript est activé, il ne fonctionnera pas. Toutefois, les chiffres s’affichent dans la page.</span><span class="sxs-lookup"><span data-stu-id="7a999-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="7a999-128">Notez, cependant que la protection `FilteredTextBox` fournit n’est pas à toute épreuve : si JavaScript est activé, toutes les données peuvent être entrées dans la zone de texte, donc vous devez utiliser une validation supplémentaire signifie, par exemple, ASP. Contrôles de validation du réseau.</span><span class="sxs-lookup"><span data-stu-id="7a999-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="7a999-129">[![Seuls les chiffres peuvent être entrés.](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7a999-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="7a999-130">Seuls les chiffres peuvent être entrés ([cliquez pour afficher l’image en taille réelle](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7a999-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7a999-131">Précédent</span><span class="sxs-lookup"><span data-stu-id="7a999-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
