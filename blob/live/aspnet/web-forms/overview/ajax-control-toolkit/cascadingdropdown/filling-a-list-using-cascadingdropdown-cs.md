---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: "Remplissage d’une liste à l’aide de CascadingDropDown (c#) | Documents Microsoft"
author: wenz
description: "Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: e5e0f11a815632aff9e17dc0f783f7eba2753995
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="8dbda-103">Remplissage d’une liste à l’aide de CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="8dbda-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="8dbda-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8dbda-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8dbda-105">[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8dbda-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="8dbda-106">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8dbda-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8dbda-107">(Par exemple, une seule liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Le premier test pour résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="8dbda-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="8dbda-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="8dbda-108">Overview</span></span>

<span data-ttu-id="8dbda-109">Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8dbda-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8dbda-110">(Par exemple, une seule liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Le premier test pour résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.</span><span class="sxs-lookup"><span data-stu-id="8dbda-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="8dbda-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="8dbda-111">Steps</span></span>

<span data-ttu-id="8dbda-112">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="8dbda-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="8dbda-113">Ensuite, un contrôle DropDownList est nécessaire :</span><span class="sxs-lookup"><span data-stu-id="8dbda-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="8dbda-114">Pour cette liste, un extendeur CascadingDropDown est ajouté.</span><span class="sxs-lookup"><span data-stu-id="8dbda-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="8dbda-115">Il envoie une demande asynchrone à un service web qui retourne ensuite une liste d’entrées à afficher dans la liste.</span><span class="sxs-lookup"><span data-stu-id="8dbda-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="8dbda-116">Pour ce faire, les attributs CascadingDropDown suivants doivent être définis :</span><span class="sxs-lookup"><span data-stu-id="8dbda-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="8dbda-117">`ServicePath`: URL du service web fournir les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="8dbda-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="8dbda-118">`ServiceMethod`: Méthode web fournir les entrées de liste</span><span class="sxs-lookup"><span data-stu-id="8dbda-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="8dbda-119">`TargetControlID`: ID de la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="8dbda-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="8dbda-120">`Category`: Les informations de catégorie qui sont soumises à la méthode web lorsqu’elle est appelée</span><span class="sxs-lookup"><span data-stu-id="8dbda-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="8dbda-121">`PromptText`: Texte à afficher lorsque le chargement asynchrone des données de liste à partir du serveur</span><span class="sxs-lookup"><span data-stu-id="8dbda-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="8dbda-122">Voici le balisage de la `CascadingDropDown` élément.</span><span class="sxs-lookup"><span data-stu-id="8dbda-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="8dbda-123">La seule différence entre c# et VB est le nom du service web associé :</span><span class="sxs-lookup"><span data-stu-id="8dbda-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="8dbda-124">Le code JavaScript en provenance de le `CascadingDropDown` extendeur appelle une méthode de service web avec la signature suivante :</span><span class="sxs-lookup"><span data-stu-id="8dbda-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="8dbda-125">Par conséquent, l’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (définie par les outils de contrôle ASP.NET AJAX).</span><span class="sxs-lookup"><span data-stu-id="8dbda-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="8dbda-126">Dans le `CascadingDropDownNameValue` constructeur, le premier texte de l’entrée liste, puis sa valeur doivent être fournies, tout comme `<option value="VALUE">NAME</option>` dans HTML.</span><span class="sxs-lookup"><span data-stu-id="8dbda-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="8dbda-127">Voici quelques exemples de données :</span><span class="sxs-lookup"><span data-stu-id="8dbda-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="8dbda-128">Chargement de la page dans le navigateur déclenche la liste à remplir avec les trois fournisseurs.</span><span class="sxs-lookup"><span data-stu-id="8dbda-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="8dbda-129">[![La liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8dbda-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="8dbda-130">La liste est remplie automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8dbda-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8dbda-131">Next</span><span class="sxs-lookup"><span data-stu-id="8dbda-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
