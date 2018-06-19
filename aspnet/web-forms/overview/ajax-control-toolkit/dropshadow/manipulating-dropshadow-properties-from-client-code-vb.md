---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulation des propriétés DropShadow à partir du Code Client (VB) | Documents Microsoft
author: wenz
description: Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée. Propriétés de cette extension peuvent également être modifiées à l’aide du client JavaScrip...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870906"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="bd7f0-104">Manipulation des propriétés DropShadow à partir du Code Client (VB)</span><span class="sxs-lookup"><span data-stu-id="bd7f0-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="bd7f0-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bd7f0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bd7f0-106">[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bd7f0-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="bd7f0-107">Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="bd7f0-108">Propriétés de cette extension peuvent également être modifiées à l’aide du code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="bd7f0-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="bd7f0-109">Overview</span></span>

<span data-ttu-id="bd7f0-110">Le contrôle dans la boîte à outils de contrôle AJAX DropShadow étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="bd7f0-111">Propriétés de cette extension peuvent également être modifiées à l’aide du code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="bd7f0-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="bd7f0-112">Steps</span></span>

<span data-ttu-id="bd7f0-113">Le code commence par un panneau de configuration contenant des lignes de texte :</span><span class="sxs-lookup"><span data-stu-id="bd7f0-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="bd7f0-114">La classe CSS associée donne le panneau de configuration une couleur d’arrière-plan intéressante :</span><span class="sxs-lookup"><span data-stu-id="bd7f0-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="bd7f0-115">Le `DropShadowExtender` est ajouté pour étendre le panneau de configuration avec un effet d’ombre portée, opacité définie à 50 % :</span><span class="sxs-lookup"><span data-stu-id="bd7f0-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="bd7f0-116">Ensuite, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle à utiliser :</span><span class="sxs-lookup"><span data-stu-id="bd7f0-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="bd7f0-117">Un autre panneau contient deux liens JavaScript pour définir l’opacité de l’ombre : le lien moins diminue l’opacité de l’ombre, le lien plu l’augmente.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="bd7f0-118">La fonction JavaScript `changeOpacity()` doit tout d’abord recherchez le `DropShadowExtender` contrôle sur la page.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="bd7f0-119">ASP.NET AJAX définit le `$find()` méthode pour exactement cette tâche.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="bd7f0-120">Ensuite, la `get_Opacity()` méthode extrait l’opacité en cours, le `set_Opacity()` lui affecte la méthode.</span><span class="sxs-lookup"><span data-stu-id="bd7f0-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="bd7f0-121">Le code JavaScript puis place la valeur d’opacité actuelle dans le `<label>` élément :</span><span class="sxs-lookup"><span data-stu-id="bd7f0-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="bd7f0-122">[![L’opacité est modifiée du côté client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bd7f0-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="bd7f0-123">L’opacité est modifiée du côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bd7f0-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bd7f0-124">Précédent</span><span class="sxs-lookup"><span data-stu-id="bd7f0-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
