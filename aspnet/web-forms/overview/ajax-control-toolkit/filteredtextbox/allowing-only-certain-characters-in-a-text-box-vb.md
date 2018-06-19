---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Autoriser uniquement certains caractères dans une zone de texte (VB) | Documents Microsoft
author: wenz
description: Contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée d’utilisateur. Toutefois cela toujours n’empêche pas les utilisateurs de la saisie non valides...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b63a3582c09e08310c97d4adfc7b8273458a723
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870152"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="ef338-104">Autoriser uniquement certains caractères dans une zone de texte (VB)</span><span class="sxs-lookup"><span data-stu-id="ef338-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="ef338-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ef338-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ef338-106">[Télécharger le Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ef338-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="ef338-107">Contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ef338-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="ef338-108">Toutefois cela toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="ef338-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="ef338-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="ef338-109">Overview</span></span>

<span data-ttu-id="ef338-110">Contrôles de validation ASP.NET peuvent garantir que seuls certains caractères sont autorisés dans l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ef338-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="ef338-111">Toutefois cela toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.</span><span class="sxs-lookup"><span data-stu-id="ef338-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="ef338-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="ef338-112">Steps</span></span>

<span data-ttu-id="ef338-113">Les outils de contrôle ASP.NET AJAX contient le `FilteredTextBox` contrôle qui étend une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="ef338-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="ef338-114">Une fois activé, uniquement un certain jeu de caractères peut être entré dans le champ.</span><span class="sxs-lookup"><span data-stu-id="ef338-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="ef338-115">Pour ce faire, nous devons comme d’habitude ASP.NET AJAX `ScriptManager` qui charge les bibliothèques JavaScript qui sont également utilisés par les outils de contrôle ASP.NET AJAX :</span><span class="sxs-lookup"><span data-stu-id="ef338-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="ef338-116">Ensuite, nous avons besoin d’une zone de texte :</span><span class="sxs-lookup"><span data-stu-id="ef338-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="ef338-117">Enfin, le `FilteredTextBoxExtender` contrôle prend en charge de limiter les caractères que l’utilisateur est autorisé à taper.</span><span class="sxs-lookup"><span data-stu-id="ef338-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="ef338-118">Tout d’abord, définissez le `TargetControlID` d’attribut pour le `ID` de la `TextBox` contrôle.</span><span class="sxs-lookup"><span data-stu-id="ef338-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="ef338-119">Ensuite, choisissez une des `FilterType` valeurs :</span><span class="sxs-lookup"><span data-stu-id="ef338-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="ef338-120">`Custom` valeur par défaut ; Vous devez fournir une liste de caractères valides</span><span class="sxs-lookup"><span data-stu-id="ef338-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="ef338-121">`LowercaseLetters` lettres minuscules uniquement</span><span class="sxs-lookup"><span data-stu-id="ef338-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="ef338-122">`Numbers` uniquement des chiffres</span><span class="sxs-lookup"><span data-stu-id="ef338-122">`Numbers` digits only</span></span>
- <span data-ttu-id="ef338-123">`UppercaseLetters` uniquement des lettres majuscules</span><span class="sxs-lookup"><span data-stu-id="ef338-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="ef338-124">Si le `Custom FilterType` est utilisé, le `ValidChars` propriété doit être défini et fournir une liste de caractères qui peuvent être tapés.</span><span class="sxs-lookup"><span data-stu-id="ef338-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="ef338-125">La façon dont : Si vous essayez de coller du texte dans la zone de texte, tous les caractères non valides sont supprimés.</span><span class="sxs-lookup"><span data-stu-id="ef338-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="ef338-126">Voici le balisage de la `FilteredTextBoxExtender` contrôle qui permet uniquement de chiffres (ce qui aurait également été possible avec `FilterType="Numbers"`) :</span><span class="sxs-lookup"><span data-stu-id="ef338-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="ef338-127">Exécutez la page, puis réessayez d’entrer une lettre si JavaScript est activé, il ne fonctionnera pas. Toutefois, les chiffres s’affichent dans la page.</span><span class="sxs-lookup"><span data-stu-id="ef338-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="ef338-128">Notez, cependant que la protection `FilteredTextBox` fournit n’est pas à toute épreuve : si JavaScript est activé, toutes les données peuvent être entrées dans la zone de texte, donc vous devez utiliser une validation supplémentaire signifie, par exemple, ASP. Contrôles de validation du réseau.</span><span class="sxs-lookup"><span data-stu-id="ef338-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="ef338-129">[![Seuls les chiffres peuvent être entrés.](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ef338-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="ef338-130">Seuls les chiffres peuvent être entrés ([cliquez pour afficher l’image en taille réelle](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ef338-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ef338-131">Précédent</span><span class="sxs-lookup"><span data-stu-id="ef338-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
