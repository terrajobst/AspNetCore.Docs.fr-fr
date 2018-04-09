---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Modification d’une Animation à l’aide de Code du côté Client (VB) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. L’animation peut également...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="c1159-104">Modification d’une Animation à l’aide de Code du côté Client (VB)</span><span class="sxs-lookup"><span data-stu-id="c1159-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="c1159-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c1159-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c1159-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c1159-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="c1159-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="c1159-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c1159-108">L’animation peut également être modifiée à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="c1159-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="c1159-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c1159-109">Overview</span></span>

<span data-ttu-id="c1159-110">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="c1159-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c1159-111">L’animation peut également être modifiée à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="c1159-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c1159-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="c1159-112">Steps</span></span>

<span data-ttu-id="c1159-113">Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="c1159-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="c1159-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="c1159-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="c1159-115">Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="c1159-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="c1159-116">L’animation proprement dite est lancée par un bouton HTML :</span><span class="sxs-lookup"><span data-stu-id="c1159-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="c1159-117">Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant une `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="c1159-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="c1159-118">Notez qu’il existe aucune `<Animations>` nœud dans le `AnimationExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="c1159-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="c1159-119">Code JavaScript personnalisé est utilisé pour fournir les animations à utiliser avec le contrôle.</span><span class="sxs-lookup"><span data-stu-id="c1159-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="c1159-120">Comme avec l’API du serveur de `AnimationExtender`, il n’existe aucun moyen facile d’affecter une animation à l’extendeur encore.</span><span class="sxs-lookup"><span data-stu-id="c1159-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="c1159-121">Toutefois l’extendeur expose plusieurs méthodes pour lire et écrire des animations enregistré avec les différents événements (`OnClick`, `OnLoad`, et ainsi de suite).</span><span class="sxs-lookup"><span data-stu-id="c1159-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="c1159-122">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="c1159-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="c1159-123">Le format de la valeur de retour de la `get_*()` fonctions et le format de l’argument pour le `set_*()` fonctions est une chaîne JSON, en fournissant une représentation d’objet de ce que serait le balisage XML.</span><span class="sxs-lookup"><span data-stu-id="c1159-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="c1159-124">Actuellement, il n’existe aucun moyen de passer un objet dans, mais il est possible de lire un objet à partir d’une animation donnée (`get_OnXXXBehavior()` méthodes).</span><span class="sxs-lookup"><span data-stu-id="c1159-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="c1159-125">Voici une chaîne JSON (sans les guillemets de délimitation et de mise en forme correcte) représentant une animation déclenchée par le bouton, mais l’animation du Panneau de configuration en redimensionnant et du fondu en même temps :</span><span class="sxs-lookup"><span data-stu-id="c1159-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="c1159-126">Le code JavaScript suivant affecte cette descripting JSON pour le `OnClick` animation de l’extendeur actuel et l’exécute :</span><span class="sxs-lookup"><span data-stu-id="c1159-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="c1159-127">[![L’animation s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c1159-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="c1159-128">L’animation s’exécute immédiatement, sans un clic de souris (et avec très peu de balisage) ([cliquez pour afficher l’image en taille réelle](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c1159-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c1159-129">[Précédent](executing-animations-using-client-side-code-vb.md)
> [Suivant](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c1159-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
