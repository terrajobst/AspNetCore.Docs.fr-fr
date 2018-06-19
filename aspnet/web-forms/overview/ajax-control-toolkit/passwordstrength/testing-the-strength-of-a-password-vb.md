---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Test de la force de mot de passe (VB) | Documents Microsoft
author: wenz
description: Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différées ont tendance à choisir des mots de passe simples, faciles à arrêter. Le contrôle PasswordStrength dans ASP. N....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879450"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="27dc5-104">Test de la force de mot de passe (VB)</span><span class="sxs-lookup"><span data-stu-id="27dc5-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="27dc5-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="27dc5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="27dc5-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="27dc5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="27dc5-107">Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différées ont tendance à choisir des mots de passe simples, faciles à arrêter.</span><span class="sxs-lookup"><span data-stu-id="27dc5-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="27dc5-108">Le contrôle PasswordStrength dans les outils de contrôle ASP.NET AJAX peut vérifier la qualité est d’un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="27dc5-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="27dc5-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="27dc5-109">Overview</span></span>

<span data-ttu-id="27dc5-110">Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différées ont tendance à choisir des mots de passe simples, faciles à arrêter.</span><span class="sxs-lookup"><span data-stu-id="27dc5-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="27dc5-111">Le `PasswordStrength` contrôle dans la boîte à outils de contrôle ASP.NET AJAX peut vérifier la qualité est d’un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="27dc5-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="27dc5-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="27dc5-112">Steps</span></span>

<span data-ttu-id="27dc5-113">Le `PasswordStrength` contrôle étend une zone de texte et vérifie si le mot de passe est suffisant.</span><span class="sxs-lookup"><span data-stu-id="27dc5-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="27dc5-114">Il offre de nombreuses options via des attributs ; Voici quelques-unes d'entre elles :</span><span class="sxs-lookup"><span data-stu-id="27dc5-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="27dc5-115">`MinimumNumericCharacters` nombre minimal de caractères numériques requis dans le mot de passe</span><span class="sxs-lookup"><span data-stu-id="27dc5-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="27dc5-116">`MinimumSymbolCharacters` nombre minimal de caractères de symbole (pas des lettres et chiffres) requis dans le mot de passe</span><span class="sxs-lookup"><span data-stu-id="27dc5-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="27dc5-117">`PreferredPasswordLength` longueur minimale du mot de passe</span><span class="sxs-lookup"><span data-stu-id="27dc5-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="27dc5-118">`RequiresUpperAndLowerCaseCharacters` Indique si le mot de passe doit utiliser des caractères majuscules et minuscules</span><span class="sxs-lookup"><span data-stu-id="27dc5-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="27dc5-119">Le `StrengthIndicatorType` fournit les informations comment présenter la force du mot de passe, sous forme de texte (valeur `"Text"`) ou comme une sorte de barre de progression (valeur `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="27dc5-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="27dc5-120">Dans la `DisplayPosition` attribut, vous configurez dans lequel les informations s’affichent.</span><span class="sxs-lookup"><span data-stu-id="27dc5-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="27dc5-121">Voici un exemple complet, y compris ASP.NET AJAX `ScriptManager` (contrôle), le `PasswordStrength` contrôle et, bien sûr, une zone de texte dans lequel l’utilisateur peut entrer un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="27dc5-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="27dc5-122">Pour des raisons de démonstration, le champ de formulaire de ce dernier est un champ de texte standard et pas un champ de mot de passe afin que vous puissiez voir pendant le développement ce que vous tapez.</span><span class="sxs-lookup"><span data-stu-id="27dc5-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="27dc5-123">Exécuter la page et le type de suite : uniquement une fois que vous avez entré des lettres minuscules, lettres majuscules, chiffres et symboles, le mot de passe est considéré comme insécable.</span><span class="sxs-lookup"><span data-stu-id="27dc5-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="27dc5-124">[![À présent, le mot de passe () bien](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="27dc5-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="27dc5-125">À présent, le mot de passe () bien ([cliquez pour afficher l’image en taille réelle](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="27dc5-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="27dc5-126">Précédent</span><span class="sxs-lookup"><span data-stu-id="27dc5-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
