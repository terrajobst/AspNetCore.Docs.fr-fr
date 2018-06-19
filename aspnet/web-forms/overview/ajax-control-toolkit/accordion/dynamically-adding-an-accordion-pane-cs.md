---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Ajout dynamique d’un volet Accordéon (c#) | Documents Microsoft
author: wenz
description: Le contrôle Accordéon dans la boîte à outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur afficher l’un d'entre eux à la fois. Panneaux est généralement déclaré w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ad2fc6ea3d527215c0226f3f594d781163d538b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879463"
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="a3355-104">Ajout dynamique d’un volet Accordéon (c#)</span><span class="sxs-lookup"><span data-stu-id="a3355-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="a3355-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a3355-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a3355-106">[Télécharger le Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a3355-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="a3355-107">Le contrôle Accordéon dans la boîte à outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur afficher l’un d'entre eux à la fois.</span><span class="sxs-lookup"><span data-stu-id="a3355-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="a3355-108">Panneaux est généralement déclaré dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.</span><span class="sxs-lookup"><span data-stu-id="a3355-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="a3355-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a3355-109">Overview</span></span>

<span data-ttu-id="a3355-110">Le contrôle Accordéon dans la boîte à outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur afficher l’un d'entre eux à la fois.</span><span class="sxs-lookup"><span data-stu-id="a3355-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="a3355-111">Panneaux est généralement déclaré dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.</span><span class="sxs-lookup"><span data-stu-id="a3355-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="a3355-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="a3355-112">Steps</span></span>

<span data-ttu-id="a3355-113">Le contrôle Accordéon expose toutes les propriétés importantes pour le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="a3355-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="a3355-114">Entre autres choses, le `Panes` propriété accorde l’accès à la collection de volets qui composent l’accordéon.</span><span class="sxs-lookup"><span data-stu-id="a3355-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="a3355-115">Chaque volet est de type `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="a3355-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="a3355-116">Par conséquent, il est facile pour créer un volet de ce type :</span><span class="sxs-lookup"><span data-stu-id="a3355-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="a3355-117">Le `HeaderContainer` propriété du `AccordionPane` fournit l’accès aux contrôles ASP.NET dans la section d’en-tête du volet ; le `ContentContainer` propriété de `AccordionPane` fait de même pour la section de contenu du volet.</span><span class="sxs-lookup"><span data-stu-id="a3355-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="a3355-118">Cela permet au code ASP.NET ajouter du contenu vers les volets :</span><span class="sxs-lookup"><span data-stu-id="a3355-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="a3355-119">Enfin, la pane(s) doit être ajouté à la `Panes` collection de l’accordéon :</span><span class="sxs-lookup"><span data-stu-id="a3355-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="a3355-120">Voici un code côté serveur complet qui ajoute deux volets à un contrôle Accordéon :</span><span class="sxs-lookup"><span data-stu-id="a3355-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="a3355-121">Le seul élément manquant est Accordéon lui-même, ce qui dépend de la présence de ASP.NET `ScriptManager` contrôle :</span><span class="sxs-lookup"><span data-stu-id="a3355-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="a3355-122">Pour terminer l’exemple, les deux classes CSS référencés dans le contrôle Accordéon fournissent des informations de style pour le navigateur :</span><span class="sxs-lookup"><span data-stu-id="a3355-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="a3355-123">[![Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3355-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="a3355-124">Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a3355-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3355-125">[Précédent](databinding-to-an-accordion-cs.md)
> [Suivant](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a3355-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
