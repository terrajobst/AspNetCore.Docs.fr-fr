---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulation des propriétés de DropShadow depuis du Code Client (VB) | Microsoft Docs
author: wenz
description: Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée. Propriétés de cet extendeur peuvent également être modifiées à l’aide du client JavaScrip...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b2be6964e3f46a007cda7ddba4dda14d03449d34
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378921"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="20836-104">Manipulation des propriétés de DropShadow depuis du Code Client (VB)</span><span class="sxs-lookup"><span data-stu-id="20836-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="20836-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="20836-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="20836-106">[Télécharger le Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="20836-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="20836-107">Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="20836-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="20836-108">Propriétés de cet extendeur peuvent également être modifiées à l’aide de code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="20836-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="20836-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="20836-109">Overview</span></span>

<span data-ttu-id="20836-110">Le contrôle DropShadow dans AJAX Control Toolkit étend un panneau avec une ombre portée.</span><span class="sxs-lookup"><span data-stu-id="20836-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="20836-111">Propriétés de cet extendeur peuvent également être modifiées à l’aide de code JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="20836-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="20836-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="20836-112">Steps</span></span>

<span data-ttu-id="20836-113">Le code commence par un panneau contenant des lignes de texte :</span><span class="sxs-lookup"><span data-stu-id="20836-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="20836-114">La classe CSS associée donne le volet d’une couleur d’arrière-plan intéressante :</span><span class="sxs-lookup"><span data-stu-id="20836-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="20836-115">Le `DropShadowExtender` est ajouté pour étendre le panneau avec un opacité définie sur 50 %, effet d’ombre portée :</span><span class="sxs-lookup"><span data-stu-id="20836-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="20836-116">Ensuite, ASP.NET AJAX `ScriptManager` contrôle permet de la boîte à outils de contrôle fonctionne :</span><span class="sxs-lookup"><span data-stu-id="20836-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="20836-117">Un autre panneau contient deux liens de JavaScript pour définir l’opacité de l’ombre : le lien moins diminue l’opacité de l’ombre, le lien plus (+) pour l’augmenter.</span><span class="sxs-lookup"><span data-stu-id="20836-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="20836-118">La fonction JavaScript `changeOpacity()` doit ensuite d’abord trouver le `DropShadowExtender` contrôle sur la page.</span><span class="sxs-lookup"><span data-stu-id="20836-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="20836-119">ASP.NET AJAX définit le `$find()` méthode pour exactement cette tâche.</span><span class="sxs-lookup"><span data-stu-id="20836-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="20836-120">Ensuite, le `get_Opacity()` méthode récupère l’opacité en cours, le `set_Opacity()` méthode il définit.</span><span class="sxs-lookup"><span data-stu-id="20836-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="20836-121">Le code JavaScript puis place la valeur d’opacité actuelle dans le `<label>` élément :</span><span class="sxs-lookup"><span data-stu-id="20836-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="20836-122">[![L’opacité est modifiée du côté client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="20836-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="20836-123">L’opacité est modifiée du côté client ([cliquez pour afficher l’image en taille réelle](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="20836-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="20836-124">Précédent</span><span class="sxs-lookup"><span data-stu-id="20836-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
