---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: L’utilisation de publications (postback) avec ReorderList fournissant (c#) | Documents Microsoft
author: wenz
description: Le contrôle ReorderList fournissant dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer. Chaque fois que la liste est réorganisée, un bon de commande en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed01c30c0721c8f1cd8ccb3fea0735ea8fa4f0a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="0c51f-104">L’utilisation de publications (postback) avec ReorderList fournissant (c#)</span><span class="sxs-lookup"><span data-stu-id="0c51f-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="0c51f-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0c51f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0c51f-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0c51f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="0c51f-107">Le contrôle ReorderList fournissant dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer.</span><span class="sxs-lookup"><span data-stu-id="0c51f-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0c51f-108">Chaque fois que la liste est réorganisée, une publication (postback) informe le serveur de la modification.</span><span class="sxs-lookup"><span data-stu-id="0c51f-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="0c51f-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="0c51f-109">Overview</span></span>

<span data-ttu-id="0c51f-110">Le `ReorderList` contrôle dans la boîte à outils de contrôle AJAX fournit une liste qui peut être réorganisée par l’utilisateur via la fonction glisser- déposer.</span><span class="sxs-lookup"><span data-stu-id="0c51f-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0c51f-111">Chaque fois que la liste est réorganisée, une publication (postback) informe le serveur de la modification.</span><span class="sxs-lookup"><span data-stu-id="0c51f-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="0c51f-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="0c51f-112">Steps</span></span>

<span data-ttu-id="0c51f-113">Il existe plusieurs sources de données possibles pour le `ReorderList` contrôle.</span><span class="sxs-lookup"><span data-stu-id="0c51f-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="0c51f-114">Un consiste à utiliser un `XmlDataSource` contrôle :</span><span class="sxs-lookup"><span data-stu-id="0c51f-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="0c51f-115">Afin de lier ce code XML pour un `ReorderList` doivent avoir la valeur contrôle et activer les publications, les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="0c51f-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="0c51f-116">`DataSourceID`: L’ID de la source de données</span><span class="sxs-lookup"><span data-stu-id="0c51f-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="0c51f-117">`SortOrderField`: La propriété selon laquelle trier par</span><span class="sxs-lookup"><span data-stu-id="0c51f-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="0c51f-118">`AllowReorder`: S’il faut autoriser l’utilisateur à réorganiser les éléments de liste</span><span class="sxs-lookup"><span data-stu-id="0c51f-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="0c51f-119">`PostBackOnReorder`: Si vous souhaitez créer une publication (postback) chaque fois que la liste est réorganisée.</span><span class="sxs-lookup"><span data-stu-id="0c51f-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="0c51f-120">Voici le balisage approprié pour le contrôle :</span><span class="sxs-lookup"><span data-stu-id="0c51f-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="0c51f-121">Dans le `ReorderList` contrôle, les données spécifiques à partir de la source de données peut être lié à l’aide de la `Eval()` méthode :</span><span class="sxs-lookup"><span data-stu-id="0c51f-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="0c51f-122">À une position arbitraire sur la page, une étiquette contiendra les informations lors de la réorganisation dernière s’est produite :</span><span class="sxs-lookup"><span data-stu-id="0c51f-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="0c51f-123">Cette étiquette est remplie avec le texte dans le code côté serveur, gérer la publication :</span><span class="sxs-lookup"><span data-stu-id="0c51f-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="0c51f-124">Enfin, pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé sur la page :</span><span class="sxs-lookup"><span data-stu-id="0c51f-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="0c51f-125">[![Chaque réorganisation déclenche une publication (postback)](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0c51f-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="0c51f-126">Chaque réorganisation déclenche une publication ([cliquez pour afficher l’image en taille réelle](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0c51f-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0c51f-127">Next</span><span class="sxs-lookup"><span data-stu-id="0c51f-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
