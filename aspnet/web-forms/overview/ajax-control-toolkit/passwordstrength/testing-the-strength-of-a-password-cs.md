---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Test de la force de mot de passe (c#) | Microsoft Docs
author: wenz
description: Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différés ont tendance à choisir des mots de passe simples qui sont faciles à arrêter. Le contrôle PasswordStrength dans ASP. N....
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: b71744df46f8b8d20efafa333a10370397c37d2f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826593"
---
<a name="testing-the-strength-of-a-password-c"></a>Test de la force de mot de passe (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différés ont tendance à choisir des mots de passe simples qui sont faciles à arrêter. Le contrôle PasswordStrength dans ASP.NET AJAX Control Toolkit peut vérifier la qualité est d’un mot de passe.


## <a name="overview"></a>Vue d'ensemble

Presque n’importe où, les mots de passe sont requis afin que les utilisateurs différés ont tendance à choisir des mots de passe simples qui sont faciles à arrêter. Le `PasswordStrength` contrôle dans ASP.NET AJAX Control Toolkit peut vérifier la qualité est d’un mot de passe.

## <a name="steps"></a>Étapes

Le `PasswordStrength` contrôle étend une zone de texte et vérifie si le mot de passe qu’il contient est suffisant. Il offre une multitude d’options via des attributs ; Voici quelques-uns d'entre eux :

- `MinimumNumericCharacters` nombre minimal de caractères numériques requis dans le mot de passe
- `MinimumSymbolCharacters` nombre minimal de caractères de symbole (pas des lettres et chiffres) requis dans le mot de passe
- `PreferredPasswordLength` longueur minimale du mot de passe
- `RequiresUpperAndLowerCaseCharacters` Indique si le mot de passe doit utiliser des caractères majuscules et minuscules

Le `StrengthIndicatorType` fournit les informations comment présenter la force du mot de passe, sous forme de texte (valeur `"Text"`) ou comme une sorte de barre de progression (valeur `"BarIndicator"`). Dans le `DisplayPosition` attribut, vous configurez dans lequel les informations s’affichent. Voici un exemple complet, y compris ASP.NET AJAX `ScriptManager` contrôle, le `PasswordStrength` contrôle et, bien sûr, une zone de texte où l’utilisateur peut entrer un mot de passe. Fins de démonstration, le champ de formulaire de ce dernier est un champ de texte normal et non un champ de mot de passe afin que vous puissiez voir pendant le développement ce que vous tapez.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Exécuter la page, puis tapez absent : uniquement une fois que vous avez entré des lettres minuscules, lettres majuscules, chiffres et symboles, le mot de passe est jugé comme unbreakable.


[![Le mot de passe est maintenant () bien](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Maintenant le mot de passe (assez) convient ([cliquez pour afficher l’image en taille réelle](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](testing-the-strength-of-a-password-vb.md)
