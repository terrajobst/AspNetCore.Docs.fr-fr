---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: À l’aide de contrôles de boîte à outils de contrôle AJAX et extensions de contrôle (c#) | Documents Microsoft
author: microsoft
description: Découvrez comment ajouter des contrôles de boîte à outils de contrôle AJAX et aux extendeurs à vos pages ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873493"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a><span data-ttu-id="1058e-103">À l’aide de contrôles de boîte à outils de contrôle AJAX et extensions de contrôle (c#)</span><span class="sxs-lookup"><span data-stu-id="1058e-103">Using AJAX Control Toolkit Controls and Control Extenders (C#)</span></span>
====================
<span data-ttu-id="1058e-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1058e-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1058e-105">Découvrez comment ajouter des contrôles de boîte à outils de contrôle AJAX et aux extendeurs à vos pages ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1058e-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="1058e-106">La boîte à outils de contrôle AJAX contient un ensemble de contrôles et les extendeurs de contrôle.</span><span class="sxs-lookup"><span data-stu-id="1058e-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="1058e-107">Dans ce bref didacticiel, vous allez apprendre à ajouter des contrôles et les extensions de contrôle à une page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1058e-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="1058e-108">Pour obtenir des instructions sur l’installation de la boîte à outils de contrôle AJAX et l’ajout de la boîte à outils de contrôle AJAX à la boîte à outils Visual Studio/Visual Web Developer, consultez le didacticiel [prise en main la boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-cs.md).</span><span class="sxs-lookup"><span data-stu-id="1058e-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="1058e-109">À l’aide de contrôles de boîte à outils de contrôle AJAX</span><span class="sxs-lookup"><span data-stu-id="1058e-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="1058e-110">Un contrôle de boîte à outils de contrôle AJAX fonctionne comme un contrôle ASP.NET normal.</span><span class="sxs-lookup"><span data-stu-id="1058e-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="1058e-111">Vous pouvez faire glisser le contrôle à partir de la boîte à outils vers une page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1058e-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="1058e-112">Vous pouvez ajouter le contrôle à la page en mode Création ou de vue de Source.</span><span class="sxs-lookup"><span data-stu-id="1058e-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="1058e-113">Il existe un besoin particulier lorsque vous utilisez les contrôles à partir de la boîte à outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="1058e-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="1058e-114">La page doit contenir un contrôle ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="1058e-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="1058e-115">Le contrôle ScriptManager est responsable de l’inclure tous les JavaScript nécessaire requis par les contrôles de boîte à outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="1058e-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="1058e-116">Par exemple, l’onglet Outils de contrôle AJAX inclut un contrôle nommé le contrôle de l’éditeur.</span><span class="sxs-lookup"><span data-stu-id="1058e-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="1058e-117">Ce contrôle affiche un éditeur HTML complet.</span><span class="sxs-lookup"><span data-stu-id="1058e-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="1058e-118">Suivez ces étapes pour ajouter le contrôle d’édition à une page :</span><span class="sxs-lookup"><span data-stu-id="1058e-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="1058e-119">Créer une page ASP.NET nommée ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="1058e-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="1058e-120">Sélectionnez le contrôle ScriptManager sous l’onglet Extensions AJAX dans la boîte à outils et faites glisser le contrôle sur la page.</span><span class="sxs-lookup"><span data-stu-id="1058e-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="1058e-121">Sélectionnez le contrôle d’édition sous l’onglet Outils de contrôle AJAX dans la boîte à outils et faites glisser le contrôle sur la page (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="1058e-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="1058e-122">Le concepteur doit ressembler à la Figure 2.</span><span class="sxs-lookup"><span data-stu-id="1058e-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="1058e-123">Exécuter le site web en sélectionnant l’option de menu **déboguer, démarrer le débogage** ou en appuyant sur la touche F5.</span><span class="sxs-lookup"><span data-stu-id="1058e-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="1058e-124">Vous devez voir la page dans la Figure 3.</span><span class="sxs-lookup"><span data-stu-id="1058e-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="1058e-125">[![Sélection du contrôle de l’éditeur HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1058e-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)</span></span>

<span data-ttu-id="1058e-126">**Figure 01**: sélection du contrôle de l’éditeur HTML ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="1058e-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))</span></span>


<span data-ttu-id="1058e-127">[![Concepteur de Visual Studio avec le contrôle ScriptManager et modifier](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1058e-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)</span></span>

<span data-ttu-id="1058e-128">**Figure 02**: concepteur avec le contrôle ScriptManager et l’édition de Visual Studio ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="1058e-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))</span></span>


<span data-ttu-id="1058e-129">[![La page DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1058e-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)</span></span>

<span data-ttu-id="1058e-130">**Figure 03**: DisplayEditor.aspx la page ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1058e-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="1058e-131">À l’aide des extensions de contrôle de boîte à outils de contrôle AJAX</span><span class="sxs-lookup"><span data-stu-id="1058e-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="1058e-132">La boîte à outils de contrôle AJAX contient également des extensions de contrôle.</span><span class="sxs-lookup"><span data-stu-id="1058e-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="1058e-133">Comme son nom l’indique, un extendeur de contrôle étend les fonctionnalités d’un contrôle existant.</span><span class="sxs-lookup"><span data-stu-id="1058e-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="1058e-134">Par exemple, l’extendeur de contrôle ConfirmButton étend le contrôle ASP.NET standard.</span><span class="sxs-lookup"><span data-stu-id="1058e-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="1058e-135">L’extendeur modifie le comportement de s de contrôle de bouton afin que le bouton affiche une boîte de dialogue de confirmation lorsque vous cliquez dessus.</span><span class="sxs-lookup"><span data-stu-id="1058e-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="1058e-136">Un extendeur de contrôle, comme un contrôle de boîte à outils de contrôle AJAX, nécessite un contrôle ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="1058e-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="1058e-137">Avant de commencer à l’aide des extensions de contrôle dans la page, vous devez ajouter un contrôle ScriptManager à une page.</span><span class="sxs-lookup"><span data-stu-id="1058e-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="1058e-138">Suivez ces étapes pour utiliser l’extendeur de contrôle ConfirmButton :</span><span class="sxs-lookup"><span data-stu-id="1058e-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="1058e-139">Créer une page ASP.NET nommée ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="1058e-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="1058e-140">Ajouter un contrôle ScriptManager à la page en faisant glisser le contrôle sur la page sous l’onglet Extensions AJAX.</span><span class="sxs-lookup"><span data-stu-id="1058e-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="1058e-141">Ajouter un contrôle bouton standard à la page en faisant glisser le bouton sous l’onglet Standard dans la boîte à outils jusqu'à l’aire du concepteur.</span><span class="sxs-lookup"><span data-stu-id="1058e-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="1058e-142">Cliquez sur le **ajouter un extendeur** option de tâches (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="1058e-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="1058e-143">Dans la boîte de dialogue Choisir un extendeur, sélectionnez ConfirmButtonExtender (voir Figure 5) et cliquez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="1058e-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="1058e-144">Sélectionnez le contrôle de bouton dans le concepteur et développez les extendeurs, Button1\_nœud ConfirmButtonExtender dans la fenêtre Propriétés (voir Figure 6).</span><span class="sxs-lookup"><span data-stu-id="1058e-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="1058e-145">Affectez la valeur *vraiment ?* à la propriété ConfirmText la valeur.</span><span class="sxs-lookup"><span data-stu-id="1058e-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="1058e-146">Exécuter la page en sélectionnant l’option de menu **déboguer, démarrer le débogage** ou appuyez sur la touche F5.</span><span class="sxs-lookup"><span data-stu-id="1058e-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="1058e-147">[![L’option de la tâche Ajouter un extendeur](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1058e-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)</span></span>

<span data-ttu-id="1058e-148">**Figure 04**: l’option Ajouter un extendeur la tâche ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="1058e-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))</span></span>


<span data-ttu-id="1058e-149">[![En sélectionnant l’extendeur de contrôle ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="1058e-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)</span></span>

<span data-ttu-id="1058e-150">**Figure 05**: en sélectionnant l’extendeur de contrôle ConfirmButton ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="1058e-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))</span></span>


<span data-ttu-id="1058e-151">[![Définition d’une propriété ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="1058e-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)</span></span>

<span data-ttu-id="1058e-152">**Figure 06**: définition d’une propriété ConfirmButton ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="1058e-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))</span></span>


<span data-ttu-id="1058e-153">Lorsque la page s’ouvre, vous devriez voir un bouton.</span><span class="sxs-lookup"><span data-stu-id="1058e-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="1058e-154">Lorsque vous cliquez sur le bouton, vous obtenez la boîte de dialogue de confirmation dans la Figure 7.</span><span class="sxs-lookup"><span data-stu-id="1058e-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="1058e-155">[![Affichage de la boîte de dialogue de confirmation](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="1058e-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)</span></span>

<span data-ttu-id="1058e-156">**Figure 07**: affichage de la boîte de dialogue de confirmation ([cliquez pour afficher l’image en taille réelle](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="1058e-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))</span></span>


<span data-ttu-id="1058e-157">Notez que vous normalement ne faites pas glisser un extendeur de contrôle sur une page.</span><span class="sxs-lookup"><span data-stu-id="1058e-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="1058e-158">Au lieu de cela, vous utilisez la **ajouter un extendeur** option pour ajouter un extendeur à un contrôle que vous avez déjà ajouté à une page de tâches.</span><span class="sxs-lookup"><span data-stu-id="1058e-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="1058e-159">Notez, en outre, que vous définissez contrôle propriétés extendeur en ouvrant la feuille de propriétés pour le contrôle en cours d’extension.</span><span class="sxs-lookup"><span data-stu-id="1058e-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="1058e-160">Un seul contrôle ASP.NET peut être étendu par plusieurs systèmes de contrôle.</span><span class="sxs-lookup"><span data-stu-id="1058e-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="1058e-161">La feuille de propriétés pour le contrôle en cours d’extension répertorie tous les extendeurs de contrôle associés au contrôle.</span><span class="sxs-lookup"><span data-stu-id="1058e-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1058e-162">[Précédent](get-started-with-the-ajax-control-toolkit-cs.md)
> [Suivant](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1058e-162">[Previous](get-started-with-the-ajax-control-toolkit-cs.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)</span></span>
