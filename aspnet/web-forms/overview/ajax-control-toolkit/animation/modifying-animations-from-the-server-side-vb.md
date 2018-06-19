---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modification des Animations à partir du serveur (VB) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b9ce85fc5040b2318233b3c553c2cf53dd03555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869268"
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="70fc3-104">Modification des Animations à partir du serveur (VB)</span><span class="sxs-lookup"><span data-stu-id="70fc3-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="70fc3-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="70fc3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="70fc3-106">[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="70fc3-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="70fc3-107">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="70fc3-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="70fc3-108">Les animations peuvent également être modifiées sur le côté serveur</span><span class="sxs-lookup"><span data-stu-id="70fc3-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="70fc3-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="70fc3-109">Overview</span></span>

<span data-ttu-id="70fc3-110">Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle.</span><span class="sxs-lookup"><span data-stu-id="70fc3-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="70fc3-111">Les animations peuvent également être modifiées sur le côté serveur</span><span class="sxs-lookup"><span data-stu-id="70fc3-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="70fc3-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="70fc3-112">Steps</span></span>

<span data-ttu-id="70fc3-113">Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="70fc3-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="70fc3-114">L’animation sera appliquée à un volet de texte qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="70fc3-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="70fc3-115">Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="70fc3-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="70fc3-116">Le reste du code s’exécute sur le côté serveur et n’utilise pas de balisage ; au lieu de cela, il utilise le code pour créer le `AnimationExtender` contrôle :</span><span class="sxs-lookup"><span data-stu-id="70fc3-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="70fc3-117">Toutefois, les outils de contrôle actuellement ne fournit pas un accès à l’API pour créer des animations.</span><span class="sxs-lookup"><span data-stu-id="70fc3-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="70fc3-118">Il est toutefois possible de définir la `AnimationExtender`propriété Animations en une chaîne qui contient le balisage XML utilisé lors de l’attribution de façon déclarative les animations.</span><span class="sxs-lookup"><span data-stu-id="70fc3-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="70fc3-119">Pour créer le code XML qui ne doit pas contenir le `<Animations>` élément que vous pouvez utiliser les XML du .NET Framework prend en charge ou, comme dans le code suivant, juste fournir la chaîne :</span><span class="sxs-lookup"><span data-stu-id="70fc3-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="70fc3-120">Enfin, ajoutez le `AnimationExtender` dans le contrôle à la page active, le `<form runat="server">` élément, en veillant à ce que l’animation est incluse et s’exécute :</span><span class="sxs-lookup"><span data-stu-id="70fc3-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="70fc3-121">[![L’animation est créée à l’aide de code en C# /Visual Basic côté serveur](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="70fc3-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="70fc3-122">L’animation est créée à l’aide de code en C# /Visual Basic côté serveur ([cliquez pour afficher l’image en taille réelle](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="70fc3-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="70fc3-123">[Précédent](triggering-an-animation-in-another-control-vb.md)
> [Suivant](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="70fc3-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
