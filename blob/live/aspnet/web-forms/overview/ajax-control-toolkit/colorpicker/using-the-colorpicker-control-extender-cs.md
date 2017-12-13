---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: "À l’aide de l’extendeur du contrôle ColorPicker (c#) | Documents Microsoft"
author: microsoft
description: "ColorPicker est un extendeur ASP.NET AJAX qui fournit une fonctionnalité de choix de couleurs de côté client avec l’interface utilisateur dans un contrôle de menu contextuel. Elle peut être jointe à n’importe quel ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3cde9552e8aecd5e7e651a825902fb79ae108c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="64732-104">À l’aide de l’extendeur du contrôle ColorPicker (c#)</span><span class="sxs-lookup"><span data-stu-id="64732-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="64732-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="64732-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="64732-106">ColorPicker est un extendeur ASP.NET AJAX qui fournit une fonctionnalité de choix de couleurs de côté client avec l’interface utilisateur dans un contrôle de menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="64732-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="64732-107">Elle peut être jointe à n’importe quel contrôle ASP.NET de zone de texte.</span><span class="sxs-lookup"><span data-stu-id="64732-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="64732-108">Il s’agit.</span><span class="sxs-lookup"><span data-stu-id="64732-108">It.</span></span>


<span data-ttu-id="64732-109">L’objectif de ce didacticiel est d’expliquer comment vous pouvez utiliser l’extendeur de contrôle ColorPicker de boîte à outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="64732-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="64732-110">L’extendeur de contrôle ColorPicker affiche une boîte de dialogue contextuelle qui vous permet de sélectionner une couleur.</span><span class="sxs-lookup"><span data-stu-id="64732-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="64732-111">Le composant ColorPicker est utile lorsque vous souhaitez fournir une interface utilisateur intuitive pour un utilisateur de choisir une couleur.</span><span class="sxs-lookup"><span data-stu-id="64732-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="64732-112">Étendre un contrôle de zone de texte avec l’extendeur du contrôle ColorPicker</span><span class="sxs-lookup"><span data-stu-id="64732-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="64732-113">Par exemple, imaginez que vous souhaitez créer un site Web qui permet des visiteurs créer des cartes de visite personnalisées.</span><span class="sxs-lookup"><span data-stu-id="64732-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="64732-114">Les visiteurs peuvent entrer le texte pour une carte de visite et sélectionner la couleur.</span><span class="sxs-lookup"><span data-stu-id="64732-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="64732-115">La page ASP.NET dans la liste 1 contienne deux contrôles de zone de texte nommée txtCardText et txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="64732-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="64732-116">Quand vous envoyez le formulaire, les valeurs sélectionnées sont affichés (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="64732-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="64732-117">[![Formulaire simple pour la création d’une carte de visite](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="64732-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="64732-118">**Figure 01**: une forme Simple pour la création d’une carte de visite ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="64732-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="64732-119">**La liste 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="64732-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="64732-120">L’écran de liste 1 fonctionne, mais elle ne fournit pas une expérience utilisateur satisfaisante.</span><span class="sxs-lookup"><span data-stu-id="64732-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="64732-121">L’utilisateur a vers le type d’une couleur dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="64732-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="64732-122">Si l’utilisateur souhaite obtenir une couleur spécialisée - par exemple, uniquement la nuance de droite de pea vert - ensuite, l’utilisateur doit déterminer le code de couleur HTML sans l’aide.</span><span class="sxs-lookup"><span data-stu-id="64732-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="64732-123">Vous pouvez utiliser l’extendeur de contrôle ColorPicker pour créer une meilleure expérience utilisateur.</span><span class="sxs-lookup"><span data-stu-id="64732-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="64732-124">Le composant ColorPicker affiche une boîte de dialogue couleur lorsque vous déplacez le focus vers un contrôle de zone de texte (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="64732-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="64732-125">[![L’extendeur du contrôle ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="64732-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="64732-126">**Figure 02**: l’extendeur de contrôle ColorPicker ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="64732-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="64732-127">Vous devez effectuer deux étapes pour utiliser l’extendeur de contrôle ColorPicker avec le formulaire dans la liste 1 :</span><span class="sxs-lookup"><span data-stu-id="64732-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="64732-128">Ajouter un contrôle ScriptManager à la page</span><span class="sxs-lookup"><span data-stu-id="64732-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="64732-129">Ajoutez l’extendeur de contrôle ColorPicker à la page</span><span class="sxs-lookup"><span data-stu-id="64732-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="64732-130">Avant de pouvoir utiliser le composant ColorPicker, vous devez ajouter un ScriptManager à votre page.</span><span class="sxs-lookup"><span data-stu-id="64732-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="64732-131">Pour ajouter le ScriptManager est intéressant juste en dessous du ouverture côté serveur &lt;formulaire&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="64732-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="64732-132">Vous pouvez faire glisser ScriptManager sur la page à partir de la boîte à outils (ScriptManager se trouve sous l’onglet Extensions AJAX).</span><span class="sxs-lookup"><span data-stu-id="64732-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="64732-133">Vous pouvez également taper la balise suivante dans la vue de Source sous la balise de formulaire côté serveur d’ouverture :</span><span class="sxs-lookup"><span data-stu-id="64732-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="64732-134">&lt;ASP : ScriptManager ID = « ScriptManager1 » runat = « server » /&gt;</span><span class="sxs-lookup"><span data-stu-id="64732-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="64732-135">Le moyen le plus simple pour ajouter l’extendeur de contrôle ColorPicker à la page est en mode Design.</span><span class="sxs-lookup"><span data-stu-id="64732-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="64732-136">Si vous pointez votre souris sur la zone de texte txtCardColor, une option de la tâche s’affiche le permet de vous permet d’ajouter un extendeur (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="64732-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="64732-137">Si vous sélectionnez cette option, l’Assistant extendeur s’affiche (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="64732-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="64732-138">[![Ajout d’un extendeur](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="64732-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="64732-139">**Figure 03**: ajout d’un extendeur ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="64732-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="64732-140">[![Sélection d’un extendeur de contrôle avec l’Assistant extendeur](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="64732-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="64732-141">**Figure 04**: sélection d’un extendeur de contrôle avec l’Assistant extendeur ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="64732-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="64732-142">Vous pouvez choisir l’extendeur ColorPicker pour étendre la zone de texte txtCardColor avec l’extendeur ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="64732-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="64732-143">Cliquez sur OK pour fermer la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="64732-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="64732-144">Après avoir apporté ces modifications, la source de la page ressemble à la liste 2.</span><span class="sxs-lookup"><span data-stu-id="64732-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="64732-145">Listing 2 - CreateCard.aspx (avec ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="64732-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="64732-146">Notez que la page contienne désormais un contrôle ColorPickerExtender qui apparaît directement sous le contrôle de zone de texte txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="64732-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="64732-147">Le contrôle ColorPickerExtender étend le contrôle txtCardColor afin qu’il affiche une boîte de dialogue de sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="64732-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="64732-148">Utilisation d’un bouton pour lancer la boîte de dialogue de sélecteur de couleurs</span><span class="sxs-lookup"><span data-stu-id="64732-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="64732-149">L’extendeur ColorPicker prend en charge les propriétés suivantes :</span><span class="sxs-lookup"><span data-stu-id="64732-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="64732-150">PopupButtonId - l’ID d’un bouton dans la page qui provoque la boîte de dialogue du sélecteur de couleur apparaissent.</span><span class="sxs-lookup"><span data-stu-id="64732-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="64732-151">PopupPosition - la position, par rapport au contrôle cible, de la boîte de dialogue de sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="64732-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="64732-152">Les valeurs possibles sont absolue, centre, en bas à gauche, en bas à droite, en haut à gauche, en haut à droite, droite et gauche (la valeur par défaut est en bas à gauche).</span><span class="sxs-lookup"><span data-stu-id="64732-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="64732-153">SampleControlId - l’ID d’un contrôle qui affiche la couleur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="64732-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="64732-154">SelectedColor - couleur initiale sélectionnée par le composant ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="64732-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="64732-155">Vous pouvez utiliser ces propriétés pour personnaliser l’affichage de la boîte de dialogue de sélecteur de couleurs et l’affichage de la couleur sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="64732-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="64732-156">La page de liste 3 illustre comment vous pouvez utiliser plusieurs de ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="64732-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="64732-157">**La liste 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="64732-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="64732-158">La page de liste 3 inclut un choisir la couleur du bouton (voir Figure 5).</span><span class="sxs-lookup"><span data-stu-id="64732-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="64732-159">Lorsque vous cliquez sur ce bouton, la boîte de dialogue de sélecteur de couleur s’affiche au-dessus de la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="64732-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="64732-160">Si vous sélectionnez une couleur dans la boîte de dialogue la couleur sélectionnée s’affiche en tant que la couleur d’arrière-plan de la lblSample contrôle Label.</span><span class="sxs-lookup"><span data-stu-id="64732-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="64732-161">La propriété ColorPicker PopupButtonID est utilisée pour associer le bouton de choisir la couleur à l’extendeur ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="64732-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="64732-162">Lorsque vous fournissez une valeur pour la propriété PopupButtonID, la boîte de dialogue de sélecteur de couleurs ne s’affiche plus lorsque le contrôle cible a le focus.</span><span class="sxs-lookup"><span data-stu-id="64732-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="64732-163">Vous devez cliquer sur le bouton pour afficher la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="64732-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="64732-164">La propriété SampleControlID est utilisée pour associer un contrôle qui affiche la couleur sélectionnée avec le composant ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="64732-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="64732-165">Le composant ColorPicker modifie la couleur d’arrière-plan de ce contrôle la couleur actuellement sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="64732-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="64732-166">[![Affichage de la boîte de dialogue du sélecteur de couleurs avec un bouton](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="64732-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="64732-167">**Figure 05**: affichage de la boîte de dialogue du sélecteur de couleurs avec un bouton ([cliquez pour afficher l’image en taille réelle](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="64732-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="64732-168">Résumé</span><span class="sxs-lookup"><span data-stu-id="64732-168">Summary</span></span>

<span data-ttu-id="64732-169">Dans ce didacticiel, vous avez appris à utiliser l’extendeur de contrôle ColorPicker pour afficher une boîte de dialogue Sélecteur de couleurs contextuelle.</span><span class="sxs-lookup"><span data-stu-id="64732-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="64732-170">Tout d’abord, nous avons examiné comment vous pouvez afficher la boîte de dialogue lorsque le focus est déplacé vers un contrôle de zone de texte.</span><span class="sxs-lookup"><span data-stu-id="64732-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="64732-171">Ensuite, vous avez appris à créer un bouton qui affiche la boîte de dialogue de sélecteur de couleur quand le bouton est activé.</span><span class="sxs-lookup"><span data-stu-id="64732-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="64732-172">Next</span><span class="sxs-lookup"><span data-stu-id="64732-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
