---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulation des propriétés de DropShadow depuis du Code Client (c#) | Microsoft Docs
author: wenz
description: Personnalisation de l’Interface de contrôle DataList modification
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e3166b9da97a0f4097566b62ba52b6d672eab78f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377283"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="eb889-103">Manipulation des propriétés de DropShadow depuis du Code Client (c#)</span><span class="sxs-lookup"><span data-stu-id="eb889-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="eb889-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eb889-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eb889-105">[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="eb889-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="eb889-106">Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="eb889-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="eb889-107">Propriétés de cet extendeur peuvent également être modifiées à l’aide de code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="eb889-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="eb889-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="eb889-108">Overview</span></span>

<span data-ttu-id="eb889-109">Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="eb889-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="eb889-110">Propriétés de cet extendeur peuvent également être modifiées à l’aide de code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="eb889-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="eb889-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="eb889-111">Steps</span></span>

<span data-ttu-id="eb889-112">Le code commence par un panneau contenant des lignes de texte :</span><span class="sxs-lookup"><span data-stu-id="eb889-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="eb889-113">La classe CSS associée donne le volet d’une couleur d’arrière-plan intéressante :</span><span class="sxs-lookup"><span data-stu-id="eb889-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="eb889-114">Le `DropShadowExtender` est ajouté pour étendre le panneau avec un opacité définie sur 50 %, effet d’ombre portée :</span><span class="sxs-lookup"><span data-stu-id="eb889-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="eb889-115">Ensuite, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle fonctionne :</span><span class="sxs-lookup"><span data-stu-id="eb889-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="eb889-116">Un autre panneau contient deux liens de JavaScript pour définir l’opacité de l’ombre : le lien moins diminue l’opacité de l’ombre, le lien plus (+) pour l’augmenter.</span><span class="sxs-lookup"><span data-stu-id="eb889-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="eb889-117">La fonction JavaScript `changeOpacity()` doit ensuite d’abord trouver le `DropShadowExtender` contrôle sur la page.</span><span class="sxs-lookup"><span data-stu-id="eb889-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="eb889-118">ASP.NET AJAX définit le `$find()` méthode pour exactement cette tâche.</span><span class="sxs-lookup"><span data-stu-id="eb889-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="eb889-119">Ensuite, le `get_Opacity()` méthode récupère l’opacité en cours, le `set_Opacity()` méthode il définit.</span><span class="sxs-lookup"><span data-stu-id="eb889-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="eb889-120">Le code JavaScript puis place la valeur d’opacité actuelle dans le `<label>` élément :</span><span class="sxs-lookup"><span data-stu-id="eb889-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="eb889-121">[![L’opacité est modifiée du côté client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eb889-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="eb889-122">L’opacité est modifiée du côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="eb889-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eb889-123">[Précédent](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Suivant](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="eb889-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
