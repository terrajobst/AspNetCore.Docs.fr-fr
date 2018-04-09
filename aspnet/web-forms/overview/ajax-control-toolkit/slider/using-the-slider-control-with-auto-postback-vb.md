---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Utilisation du contrôle de curseur avec publication automatique (VB) | Documents Microsoft
author: wenz
description: Le contrôle de curseur dans la boîte à outils de contrôle AJAX fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris. Il est possible d’effectuer la comptabilisation automatique curseur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: edb8fa13716c3c0beb7cf86dd3843caaec939483
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="468df-104">Utilisation du contrôle de curseur avec publication automatique (VB)</span><span class="sxs-lookup"><span data-stu-id="468df-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="468df-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="468df-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="468df-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="468df-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="468df-107">Le contrôle de curseur dans la boîte à outils de contrôle AJAX fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="468df-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="468df-108">Il est possible d’effectuer la curseur autopostback une fois sa valeur change.</span><span class="sxs-lookup"><span data-stu-id="468df-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="468df-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="468df-109">Overview</span></span>

<span data-ttu-id="468df-110">Le contrôle de curseur dans la boîte à outils de contrôle AJAX fournit un contrôle slider graphique qui peut être contrôlé à l’aide de la souris.</span><span class="sxs-lookup"><span data-stu-id="468df-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="468df-111">Il est possible d’effectuer la curseur autopostback une fois sa valeur change.</span><span class="sxs-lookup"><span data-stu-id="468df-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="468df-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="468df-112">Steps</span></span>

<span data-ttu-id="468df-113">Pour faciliter le curseur automatiquement publication (postback) à une modification, les deux zones de texte doivent l’attribut `AutoPostBack="true"`: la zone de texte qui deviendra le curseur lui-même et la zone de texte qui contient la position du curseur.</span><span class="sxs-lookup"><span data-stu-id="468df-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="468df-114">Voici le balisage requis pour qui :</span><span class="sxs-lookup"><span data-stu-id="468df-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="468df-115">Le `SliderExtender` contrôle à partir de la boîte à outils de contrôle ASP.NET AJAX affecte les fonctionnalités de curseur à deux zones de texte :</span><span class="sxs-lookup"><span data-stu-id="468df-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="468df-116">Un élément supplémentaire de l’étiquette sera utilisé ultérieurement pour informer l’utilisateur d’une publication :</span><span class="sxs-lookup"><span data-stu-id="468df-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="468df-117">Enfin, le `ScriptManager` contrôle d’ASP.NET AJAX charge le JavaScript requis pour la boîte à outils de contrôle à utiliser :</span><span class="sxs-lookup"><span data-stu-id="468df-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="468df-118">Maintenant le curseur est publication ; sur le côté serveur, cet événement peut être intercepté et réactions :</span><span class="sxs-lookup"><span data-stu-id="468df-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="468df-119">[![En déplaçant le curseur déclenche une publication (postback)](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="468df-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="468df-120">En déplaçant le curseur déclenche une publication ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="468df-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="468df-121">[![Ensuite, la date de cette modification est écrite dans l’étiquette](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="468df-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="468df-122">Ensuite, la date de cette modification est écrite dans l’étiquette ([cliquez pour afficher l’image en taille réelle](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="468df-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="468df-123">[Précédent](databinding-the-slider-control-cs.md)
> [Suivant](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="468df-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
