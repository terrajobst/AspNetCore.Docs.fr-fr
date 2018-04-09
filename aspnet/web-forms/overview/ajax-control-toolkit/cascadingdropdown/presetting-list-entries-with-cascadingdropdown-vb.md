---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Prédéfinissez les entrées de la liste avec CascadingDropDown (VB) | Documents Microsoft
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f74e6ac80b756240870d9406a03db11c610093aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="26ff1-103">Prédéfinissez les entrées de la liste avec CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="26ff1-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="26ff1-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="26ff1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="26ff1-105">[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="26ff1-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="26ff1-106">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="26ff1-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="26ff1-107">Avec un peu de code, il est possible qu’un élément de liste est présélectionné une fois que les données ont été chargées dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="26ff1-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="26ff1-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="26ff1-108">Overview</span></span>

<span data-ttu-id="26ff1-109">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="26ff1-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="26ff1-110">(Par exemple, une seule liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Avec un peu de code, il est possible qu’un élément de liste est présélectionné une fois que les données ont été chargées dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="26ff1-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="26ff1-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="26ff1-111">Steps</span></span>

<span data-ttu-id="26ff1-112">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="26ff1-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="26ff1-113">Ensuite, un contrôle DropDownList est nécessaire :</span><span class="sxs-lookup"><span data-stu-id="26ff1-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="26ff1-114">Pour cette liste, un extendeur CascadingDropDown est ajouté, qui fournit des informations de URL et la méthode de service web :</span><span class="sxs-lookup"><span data-stu-id="26ff1-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="26ff1-115">L’extendeur CascadingDropDown puis asynchrone appelle un service web avec la signature de méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="26ff1-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="26ff1-116">La méthode retourne un tableau de type CascadingDropDown valeur.</span><span class="sxs-lookup"><span data-stu-id="26ff1-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="26ff1-117">Le constructeur du type attend la première légende de l’entrée liste, puis la valeur (HTML `value` attribut).</span><span class="sxs-lookup"><span data-stu-id="26ff1-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="26ff1-118">Si le troisième argument est défini sur true, la liste élément est sélectionné automatiquement dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="26ff1-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="26ff1-119">Chargement de la page dans le navigateur remplit la liste déroulante avec trois fournisseurs, l’autre est présélectionnée.</span><span class="sxs-lookup"><span data-stu-id="26ff1-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="26ff1-120">[![La liste est remplie et présélectionnée automatiquement](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="26ff1-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="26ff1-121">La liste est remplie et présélectionnée automatiquement ([cliquez pour afficher l’image en taille réelle](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="26ff1-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="26ff1-122">[Précédent](using-cascadingdropdown-with-a-database-vb.md)
> [Suivant](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="26ff1-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
