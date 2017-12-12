---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: "Manipulation des propriétés DropShadow depuis le Code Client (c#) | Documents Microsoft"
author: wenz
description: "Personnalisation de l’Interface d’édition du contrôle DataList"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 59f7d4610ce610ef4357510f0e861f107278b5da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="3d2a1-103">Manipulation des propriétés DropShadow depuis le Code Client (c#)</span><span class="sxs-lookup"><span data-stu-id="3d2a1-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="3d2a1-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3d2a1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3d2a1-105">[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3d2a1-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="3d2a1-106">Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="3d2a1-107">Propriétés de cette extension peuvent également être modifiées à l’aide du code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="3d2a1-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="3d2a1-108">Overview</span></span>

<span data-ttu-id="3d2a1-109">Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="3d2a1-110">Propriétés de cette extension peuvent également être modifiées à l’aide du code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="3d2a1-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="3d2a1-111">Steps</span></span>

<span data-ttu-id="3d2a1-112">Le code commence par un panneau de configuration contenant des lignes de texte :</span><span class="sxs-lookup"><span data-stu-id="3d2a1-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="3d2a1-113">La classe CSS associée donne le panneau de configuration une couleur d’arrière-plan intéressante :</span><span class="sxs-lookup"><span data-stu-id="3d2a1-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="3d2a1-114">Le `DropShadowExtender` est ajouté pour étendre le panneau de configuration avec un effet d’ombre portée, opacité définie à 50 % :</span><span class="sxs-lookup"><span data-stu-id="3d2a1-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="3d2a1-115">Ensuite, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle à utiliser :</span><span class="sxs-lookup"><span data-stu-id="3d2a1-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="3d2a1-116">Un autre panneau contient deux liens JavaScript pour définir l’opacité de l’ombre : le lien moins diminue l’opacité de l’ombre, le lien plu l’augmente.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="3d2a1-117">La fonction JavaScript `changeOpacity()` doit tout d’abord recherchez le `DropShadowExtender` contrôle sur la page.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="3d2a1-118">ASP.NET AJAX définit le `$find()` méthode pour exactement cette tâche.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="3d2a1-119">Ensuite, la `get_Opacity()` méthode extrait l’opacité en cours, le `set_Opacity()` lui affecte la méthode.</span><span class="sxs-lookup"><span data-stu-id="3d2a1-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="3d2a1-120">Le code JavaScript puis place la valeur d’opacité actuelle dans le `<label>` élément :</span><span class="sxs-lookup"><span data-stu-id="3d2a1-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="3d2a1-121">[![L’opacité est modifiée du côté client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3d2a1-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="3d2a1-122">L’opacité est modifiée du côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3d2a1-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3d2a1-123">[Précédent](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Suivant](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3d2a1-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
