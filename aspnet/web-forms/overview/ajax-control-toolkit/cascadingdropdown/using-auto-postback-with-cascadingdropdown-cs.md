---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: À l’aide de la publication automatique CascadingDropDown (c#) | Documents Microsoft
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 04e0914dd1057f9ce490f68ae3fa9c56766beafb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872466"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a><span data-ttu-id="5bb90-103">À l’aide de la publication automatique CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="5bb90-103">Using Auto-Postback with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="5bb90-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5bb90-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5bb90-105">[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5bb90-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)</span></span>

> <span data-ttu-id="5bb90-106">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="5bb90-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="5bb90-107">Toutefois lorsque le contrôle CascadingDropDown, ASP. Fonctionnalité de AutoPostBack du contrôle de NET DropDownList ne fonctionne pas, étant donné que le chargement asynchrone de données dans la liste génère une publication (inutile) lui-même.</span><span class="sxs-lookup"><span data-stu-id="5bb90-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="5bb90-108">Avec du code JavaScript, vous pouvez éviter cet effet.</span><span class="sxs-lookup"><span data-stu-id="5bb90-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="5bb90-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="5bb90-109">Overview</span></span>

<span data-ttu-id="5bb90-110">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="5bb90-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="5bb90-111">(Par exemple, une seule liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Toutefois lorsque le contrôle CascadingDropDown, ASP. Fonctionnalité de AutoPostBack du contrôle de NET DropDownList ne fonctionne pas, étant donné que le chargement asynchrone de données dans la liste génère une publication (inutile) lui-même.</span><span class="sxs-lookup"><span data-stu-id="5bb90-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="5bb90-112">Avec du code JavaScript, vous pouvez éviter cet effet.</span><span class="sxs-lookup"><span data-stu-id="5bb90-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="5bb90-113">Étapes</span><span class="sxs-lookup"><span data-stu-id="5bb90-113">Steps</span></span>

<span data-ttu-id="5bb90-114">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le &lt; `form` &gt; élément) :</span><span class="sxs-lookup"><span data-stu-id="5bb90-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="5bb90-115">Ensuite, un contrôle DropDownList est nécessaire :</span><span class="sxs-lookup"><span data-stu-id="5bb90-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="5bb90-116">Pour cette liste, un extendeur CascadingDropDown est ajouté, qui fournit des informations de URL et la méthode de service web :</span><span class="sxs-lookup"><span data-stu-id="5bb90-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="5bb90-117">L’extendeur CascadingDropDown puis asynchrone appelle un service web avec la signature de méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="5bb90-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="5bb90-118">La méthode retourne un tableau de type CascadingDropDown valeur.</span><span class="sxs-lookup"><span data-stu-id="5bb90-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="5bb90-119">Le constructeur du type attend la première légende de l’entrée liste, puis la valeur (HTML `value` attribut).</span><span class="sxs-lookup"><span data-stu-id="5bb90-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="5bb90-120">Chargement de la page dans le navigateur remplit la liste déroulante avec trois fournisseurs, l’autre est présélectionnée.</span><span class="sxs-lookup"><span data-stu-id="5bb90-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="5bb90-121">En outre, ASP.NET définit la `__doPostBack()` méthode JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5bb90-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="5bb90-122">Une fois que la page a été chargée, cet appel JavaScript est ajouté à la liste déroulante, mais uniquement si des éléments qu’il contient.</span><span class="sxs-lookup"><span data-stu-id="5bb90-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="5bb90-123">S’il n’y a aucun éléments dans la liste, la boîte à outils de contrôle est actuellement leur chargement, afin que le code JavaScript utilise un délai d’attente et tente à nouveau une demi-seconde.</span><span class="sxs-lookup"><span data-stu-id="5bb90-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

<span data-ttu-id="5bb90-124">De cette manière, une publication (postback) est exécutée uniquement lorsqu’il existe réellement des éléments dans la liste et l’utilisateur sélectionne une entrée.</span><span class="sxs-lookup"><span data-stu-id="5bb90-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="5bb90-125">[![Sélection d’un élément de liste entraîne une publication](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5bb90-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="5bb90-126">Sélection d’un élément de liste entraîne une publication ([cliquez pour afficher l’image en taille réelle](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5bb90-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5bb90-127">[Précédent](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Suivant](filling-a-list-using-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5bb90-127">[Previous](presetting-list-entries-with-cascadingdropdown-cs.md)
[Next](filling-a-list-using-cascadingdropdown-vb.md)</span></span>
