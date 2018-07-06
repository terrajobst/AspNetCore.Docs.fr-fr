---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Prédéfinition des entrées de liste avec CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 7797aa91452bfbed2695fd26e3b9d2a5783dd216
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825087"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="e847a-103">Prédéfinition des entrées de liste avec CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="e847a-103">Presetting List Entries with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="e847a-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e847a-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e847a-105">[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e847a-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="e847a-106">Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList.</span><span class="sxs-lookup"><span data-stu-id="e847a-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e847a-107">Avec un peu de code, il est possible qu’un élément de liste est présélectionné une fois que les données ont été chargées dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="e847a-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="e847a-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e847a-108">Overview</span></span>

<span data-ttu-id="e847a-109">Le contrôle CascadingDropDown dans AJAX Control Toolkit étend un contrôle DropDownList afin que les modifications dans un DropDownList charges associés à des valeurs dans un autre objet DropDownList.</span><span class="sxs-lookup"><span data-stu-id="e847a-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="e847a-110">(Par exemple, une liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Avec un peu de code, il est possible qu’un élément de liste est présélectionné une fois que les données ont été chargées dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="e847a-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="e847a-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="e847a-111">Steps</span></span>

<span data-ttu-id="e847a-112">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="e847a-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="e847a-113">Ensuite, un contrôle DropDownList est nécessaire :</span><span class="sxs-lookup"><span data-stu-id="e847a-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="e847a-114">Pour cette liste, un extendeur CascadingDropDown est ajouté, qui fournit des informations de URL et la méthode de service web :</span><span class="sxs-lookup"><span data-stu-id="e847a-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="e847a-115">L’extendeur CascadingDropDown puis appelle de façon asynchrone un service web avec la signature de méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="e847a-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="e847a-116">La méthode retourne un tableau de type valeur de CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="e847a-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="e847a-117">Le constructeur du type attend tout d’abord légende de l’entrée liste, puis la valeur (HTML `value` attribut).</span><span class="sxs-lookup"><span data-stu-id="e847a-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="e847a-118">Si le troisième argument est défini sur true, la liste élément est automatiquement sélectionné dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e847a-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="e847a-119">Chargement de la page dans le navigateur remplira la liste déroulante avec trois fournisseurs, l’autre qui est présélectionnée.</span><span class="sxs-lookup"><span data-stu-id="e847a-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="e847a-120">[![La liste est remplie et présélectionnée automatiquement](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e847a-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="e847a-121">La liste est remplie et présélectionnée automatiquement ([cliquez pour afficher l’image en taille réelle](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e847a-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e847a-122">[Précédent](using-cascadingdropdown-with-a-database-cs.md)
> [Suivant](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e847a-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
