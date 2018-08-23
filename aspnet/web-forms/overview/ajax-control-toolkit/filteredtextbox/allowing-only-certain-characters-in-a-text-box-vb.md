---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Autoriser seulement certains caractères dans une zone de texte (VB) | Microsoft Docs
author: wenz
description: Contrôles de validation ASP.NET peuvent garantir que seulement certains caractères sont autorisés dans l’entrée d’utilisateur. Toutefois cela toujours n’empêche pas les utilisateurs de taper non valides...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: aec5a3af98cf40e460f4164fb8950e8029002937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826383"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Autoriser seulement certains caractères dans une zone de texte (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Contrôles de validation ASP.NET peuvent garantir que seulement certains caractères sont autorisés dans l’entrée d’utilisateur. Toutefois cette toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.


## <a name="overview"></a>Vue d'ensemble

Contrôles de validation ASP.NET peuvent garantir que seulement certains caractères sont autorisés dans l’entrée d’utilisateur. Toutefois cette toujours n’empêche pas les utilisateurs de taper des caractères non valides et essayez d’envoyer le formulaire.

## <a name="steps"></a>Étapes

ASP.NET AJAX Control Toolkit contient le `FilteredTextBox` contrôle qui étend une zone de texte. Une fois activé, uniquement un certain ensemble de caractères peut-être être entré dans le champ.

Pour ce faire, nous devons abord comme d’habitude ASP.NET AJAX `ScriptManager` qui charge les bibliothèques JavaScript qui sont également utilisés par ASP.NET AJAX Control Toolkit :

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Ensuite, nous avons besoin d’une zone de texte :

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Enfin, le `FilteredTextBoxExtender` contrôle s’occupe de limiter les caractères que l’utilisateur est autorisé à taper. Tout d’abord, définissez le `TargetControlID` attribut le `ID` de la `TextBox` contrôle. Ensuite, choisissez une des `FilterType` valeurs :

- `Custom` par défaut ; Vous devez fournir une liste de caractères valides
- `LowercaseLetters` uniquement des lettres minuscules
- `Numbers` chiffres uniquement
- `UppercaseLetters` uniquement des lettres majuscules

Si le `Custom FilterType` est utilisé, le `ValidChars` propriété doit être défini et fournir une liste de caractères qui peuvent être tapés. À propos : Si vous essayez de coller du texte dans la zone de texte, tous les caractères non valides sont supprimés.

Voici le balisage pour le `FilteredTextBoxExtender` contrôle qui autorise uniquement des chiffres (quelque chose qui aurait également été possible avec `FilterType="Numbers"`) :

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Exécutez la page, puis réessayez d’entrer une lettre si JavaScript est activé, il ne fonctionnera pas ; Toutefois, les chiffres apparaissent dans la page. Toutefois Notez que la protection `FilteredTextBox` fournit n’est pas absolument : si JavaScript est activé, toutes les données peuvent être entrées dans la zone de texte, donc vous devez utiliser une validation supplémentaire signifie que, par exemple, ASP. Contrôles de validation du NET.


[![Seuls les chiffres peuvent être entrés.](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Seuls les chiffres peuvent être entrés ([cliquez pour afficher l’image en taille réelle](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](allowing-only-certain-characters-in-a-text-box-cs.md)
