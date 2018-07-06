---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Création d’un AJAX personnalisé contrôler l’extendeur de contrôle Toolkit (c#) | Microsoft Docs
author: microsoft
description: Les extendeurs personnalisés permettent de personnaliser et étendre les fonctionnalités des contrôles ASP.NET sans avoir à créer de nouvelles classes.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390951"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="1f9a6-103">Création d’un extendeur de contrôle de boîte à outils de contrôle AJAX personnalisés (c#)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="1f9a6-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1f9a6-105">Les extendeurs personnalisés permettent de personnaliser et étendre les fonctionnalités des contrôles ASP.NET sans avoir à créer de nouvelles classes.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="1f9a6-106">Dans ce didacticiel, vous allez apprendre à créer un extendeur de contrôle AJAX Control Toolkit personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="1f9a6-107">Nous créons une simple, mais utile, nouvelle extendeur qui modifie l’état d’un bouton de désactivé à activé lorsque vous tapez du texte dans une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="1f9a6-108">Après avoir lu ce didacticiel, vous serez en mesure d’étendre le Kit de ressources ASP.NET AJAX avec vos propres extendeurs de contrôle.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="1f9a6-109">Vous pouvez créer des extendeurs de contrôle personnalisé à l’aide de Visual Studio ou Visual Web Developer (Vérifiez que vous disposez de la dernière version de Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="1f9a6-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="1f9a6-110">Vue d’ensemble de l’extendeur DisabledButton</span><span class="sxs-lookup"><span data-stu-id="1f9a6-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="1f9a6-111">Notre nouvel extendeur de contrôle est nommé l’extendeur DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="1f9a6-112">Cet extendeur aura trois propriétés :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-112">This extender will have three properties:</span></span>

- <span data-ttu-id="1f9a6-113">TargetControlID - la zone de texte qui étend le contrôle.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="1f9a6-114">TargetButtonIID - le bouton qui est activée ou désactivée.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="1f9a6-115">DisabledText - le texte qui s’affiche initialement dans le bouton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="1f9a6-116">Lorsque vous commencez à taper, le bouton affiche la valeur de la propriété de texte du bouton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="1f9a6-117">Raccordement de l’extendeur DisabledButton à un contrôle TextBox et Button.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="1f9a6-118">Avant de saisir n’importe quel texte, le bouton est désactivé et la zone de texte et un bouton ressemblent à ceci :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="1f9a6-119">([Cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="1f9a6-120">Une fois que vous commencez à taper de texte, le bouton est activé et la zone de texte et un bouton ressemblent à ceci :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="1f9a6-121">([Cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="1f9a6-122">Pour créer notre extendeur de contrôle, nous devons créer les trois fichiers suivants :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="1f9a6-123">DisabledButtonExtender.cs - ce fichier est la classe de contrôle côté serveur qui gère la création de votre extendeur et vous permettent de définir les propriétés au moment du design.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="1f9a6-124">Il définit également les propriétés qui peuvent être définies sur votre extendeur.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="1f9a6-125">Ces propriétés sont accessibles par le biais de code et au moment du design et correspondent aux propriétés définies dans le fichier DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="1f9a6-126">DisabledButtonBehavior.js--Ce fichier est où vous allez ajouter l’ensemble de votre logique de script client.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="1f9a6-127">DisabledButtonDesigner.cs - cette classe permet à des fonctionnalités au moment du design.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="1f9a6-128">Vous avez besoin de cette classe si vous souhaitez que l’extendeur du contrôle pour fonctionner correctement avec le Concepteur de développeur Visual Studio/Visual Web.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="1f9a6-129">Par conséquent, un extendeur de contrôle se compose d’un contrôle côté serveur, un comportement côté client et une classe de concepteur côté serveur.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="1f9a6-130">Vous allez apprendre à créer les trois de ces fichiers dans les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="1f9a6-131">Créer le projet et le site Web d’extendeur personnalisé</span><span class="sxs-lookup"><span data-stu-id="1f9a6-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="1f9a6-132">La première étape consiste à créer un projet bibliothèque de classes et le site Web dans Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="1f9a6-133">Nous ll créer l’extendeur personnalisé dans le projet de bibliothèque de classes et tester l’extendeur personnalisé dans le site Web.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="1f9a6-134">Laisser s démarrer avec le site Web.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-134">Let�s start with the website.</span></span> <span data-ttu-id="1f9a6-135">Suivez ces étapes pour créer le site Web :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="1f9a6-136">Sélectionnez l’option de menu **fichier, nouveau Site Web**.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="1f9a6-137">Sélectionnez le **Site Web ASP.NET** modèle.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="1f9a6-138">Nommez le nouveau site Web *Website1*.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="1f9a6-139">Cliquez sur le **OK** bouton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-139">Click the **OK** button.</span></span>

<span data-ttu-id="1f9a6-140">Ensuite, nous devons créer le projet de bibliothèque de classes qui contient le code pour l’extendeur du contrôle :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="1f9a6-141">Sélectionnez l’option de menu **fichier, ajouter, nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="1f9a6-142">Sélectionnez le **bibliothèque de classes** modèle.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="1f9a6-143">Nommer la nouvelle bibliothèque de classes avec le nom **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="1f9a6-144">Cliquez sur le **OK** bouton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-144">Click the **OK** button.</span></span>

<span data-ttu-id="1f9a6-145">Après avoir effectué ces étapes, votre fenêtre de l’Explorateur de solutions doit ressembler à la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="1f9a6-146">[![Solution avec un projet de bibliothèque de site Web et de classe](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="1f9a6-147">**Figure 01**: Solution avec le projet de bibliothèque de site Web et de la classe ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="1f9a6-148">Ensuite, vous devez ajouter toutes les références d’assembly nécessaires au projet de bibliothèque de classes :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="1f9a6-149">Cliquez sur le projet CustomExtenders et sélectionnez l’option de menu **ajouter une référence**.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="1f9a6-150">Sélectionnez l’onglet .NET.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="1f9a6-151">Ajoutez des références aux assemblys suivants :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="1f9a6-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="1f9a6-152">System.Web.dll</span></span>
    2. <span data-ttu-id="1f9a6-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="1f9a6-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="1f9a6-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="1f9a6-154">System.Design.dll</span></span>
    4. <span data-ttu-id="1f9a6-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="1f9a6-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="1f9a6-156">Sélectionnez l’onglet Parcourir.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="1f9a6-157">Ajoutez une référence à l’assembly AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="1f9a6-158">Cet assembly se trouve dans le dossier où vous avez téléchargé les outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="1f9a6-159">Après avoir effectué ces étapes, votre dossier de références de projet de bibliothèque de classe doit ressembler à la Figure 2.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="1f9a6-160">[![Dossier des références avec les références requises](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="1f9a6-161">**Figure 02**: dossier références avec les références requises ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="1f9a6-162">Création de l’extendeur de contrôle personnalisé</span><span class="sxs-lookup"><span data-stu-id="1f9a6-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="1f9a6-163">Maintenant que nous avons notre bibliothèque de classes, nous pouvons commencer à créer notre contrôle d’extendeur.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="1f9a6-164">Permettent de démarrer avec le composant élémentaire d’une classe de contrôle d’extendeur personnalisé (voir Listing 1) s.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="1f9a6-165">**Liste 1 - MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="1f9a6-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="1f9a6-166">Il existe plusieurs choses que vous remarquez sur la classe d’extendeur de contrôle dans le Listing 1.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="1f9a6-167">Tout d’abord, notez que la classe hérite de la classe de base ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="1f9a6-168">Tous les contrôles d’extendeur AJAX Control Toolkit dérivent de cette classe de base.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="1f9a6-169">Par exemple, la classe de base inclut la propriété TargetID qui est une propriété obligatoire de chaque extendeur de contrôle.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="1f9a6-170">Ensuite, notez que la classe inclut les deux attributs suivants liés à un script client :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="1f9a6-171">WebResource - provoque un fichier à inclure en tant que ressource incorporée dans un assembly.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="1f9a6-172">ClientScriptResource - provoque une ressource de script à récupérer à partir d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="1f9a6-173">L’attribut WebResource est utilisé pour incorporer le fichier MyControlBehavior.js JavaScript dans l’assembly lors de la compilation de l’extendeur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="1f9a6-174">L’attribut ClientScriptResource est utilisé pour récupérer le script MyControlBehavior.js à partir de l’assembly lors de l’extendeur personnalisé est utilisé dans une page web.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="1f9a6-175">Dans l’ordre des attributs WebResource et ClientScriptResource fonctionne, vous devez compiler le fichier JavaScript en tant que ressource incorporée.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="1f9a6-176">Sélectionnez le fichier dans la fenêtre Explorateur de solutions, ouvrez la feuille de propriétés et affectez la valeur *ressource incorporée* à la **Action de génération** propriété.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="1f9a6-177">Notez que l’extendeur du contrôle inclut également un attribut TargetControlType ne.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="1f9a6-178">Cet attribut est utilisé pour spécifier le type de contrôle qui est étendu par l’extendeur du contrôle.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="1f9a6-179">Dans le cas de liste 1, l’extendeur du contrôle est utilisé pour étendre une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="1f9a6-180">Enfin, notez que l’extendeur personnalisé inclut une propriété nommée MyProperty.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="1f9a6-181">La propriété est marquée avec l’attribut ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="1f9a6-182">Les méthodes GetPropertyValue() et SetPropertyValue() sont utilisées pour passer la valeur de propriété à partir de l’extendeur de contrôle côté serveur pour le comportement côté client.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="1f9a6-183">Permettent de s continuez et implémentez le code pour notre extendeur DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="1f9a6-184">Vous trouverez le code pour cet extendeur dans le Listing 2.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="1f9a6-185">**Listing 2 - DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="1f9a6-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="1f9a6-186">L’extendeur DisabledButton dans le Listing 2 a deux propriétés nommées TargetButtonID et DisabledText.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="1f9a6-187">Le IDReferenceProperty appliquée à la propriété TargetButtonID vous empêche d’autre que l’ID d’un contrôle bouton affectation à cette propriété.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="1f9a6-188">Les attributs WebResource et ClientScriptResource associent un comportement côté client situé dans un fichier nommé DisabledButtonBehavior.js avec cet extendeur.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="1f9a6-189">Nous abordons ce fichier JavaScript dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="1f9a6-190">Création d’un comportement d’extendeur personnalisé</span><span class="sxs-lookup"><span data-stu-id="1f9a6-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="1f9a6-191">Le composant côté client d’un extendeur de contrôle est appelé un comportement.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="1f9a6-192">La logique réelle de la désactivation et l’activation du bouton est contenue dans le comportement de DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="1f9a6-193">Le code JavaScript pour le comportement est inclus dans le Listing 3.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="1f9a6-194">**Liste 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="1f9a6-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="1f9a6-195">Le fichier JavaScript dans le Listing 3 contient une classe côté client nommée DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="1f9a6-196">Cette classe, comme sa représentation côté serveur, inclut deux propriétés nommées TargetButtonID et DisabledText auquel vous pouvez accéder à l’aide de get\_TargetButtonID/set\_TargetButtonID et obtenez\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="1f9a6-197">La méthode initialize() associe un gestionnaire d’événements de touche relâchée à l’élément cible pour le comportement.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="1f9a6-198">Chaque fois que vous tapez une lettre dans la zone de texte associé à ce comportement, le Gestionnaire de keyup s’exécute.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="1f9a6-199">Le Gestionnaire de keyup Active ou désactive le bouton selon que la zone de texte associée au comportement contient tout texte.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="1f9a6-200">N’oubliez pas que vous devez compiler le fichier JavaScript dans la liste de 3 comme ressource incorporée.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="1f9a6-201">Sélectionnez le fichier dans la fenêtre Explorateur de solutions, ouvrez la feuille de propriétés et affectez la valeur *ressource incorporée* à la **Action de génération** propriété (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="1f9a6-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="1f9a6-202">Cette option est disponible dans Visual Studio et Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="1f9a6-203">[![Ajout d’un fichier JavaScript en tant que ressource incorporée](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="1f9a6-204">**Figure 03**: ajout d’un fichier JavaScript comme une ressource incorporée ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="1f9a6-205">Création du Concepteur d’extendeur personnalisé</span><span class="sxs-lookup"><span data-stu-id="1f9a6-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="1f9a6-206">Il existe une classe dernière que nous devons créer pour terminer notre extendeur.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="1f9a6-207">Nous devons créer la classe de concepteur dans la liste 4.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="1f9a6-208">Cette classe est nécessaire pour rendre l’extendeur se comportent correctement avec le Concepteur de développeur Visual Studio/Visual Web.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="1f9a6-209">**Liste 4 - DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="1f9a6-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="1f9a6-210">Vous associez le concepteur dans la liste 4 de l’extendeur DisabledButton avec l’attribut de concepteur. Vous devez appliquer l’attribut de concepteur pour la classe DisabledButtonExtender comme suit :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="1f9a6-211">À l’aide de l’extendeur personnalisé</span><span class="sxs-lookup"><span data-stu-id="1f9a6-211">Using the Custom Extender</span></span>

<span data-ttu-id="1f9a6-212">Maintenant que nous avons terminé la création de l’extendeur du contrôle DisabledButton, il est temps de les utiliser dans notre site Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="1f9a6-213">Tout d’abord, nous devons ajouter l’extendeur personnalisé à la boîte à outils.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="1f9a6-214">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-214">Follow these steps:</span></span>

1. <span data-ttu-id="1f9a6-215">Ouvrez une page ASP.NET en double-cliquant sur la page dans la fenêtre Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="1f9a6-216">Avec le bouton droit de la boîte à outils et sélectionnez l’option de menu **choisir des éléments de**.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="1f9a6-217">Dans la boîte de dialogue Choisir des éléments de boîte à outils, accédez à l’assembly CustomExtenders.dll.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="1f9a6-218">Cliquez sur le **OK** bouton pour fermer la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="1f9a6-219">Après avoir effectué ces étapes, l’extendeur du contrôle DisabledButton doit apparaître dans la boîte à outils (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="1f9a6-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="1f9a6-220">[![DisabledButton dans la boîte à outils](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="1f9a6-221">**Figure 04**: DisabledButton dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="1f9a6-222">Ensuite, nous devons créer une page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="1f9a6-223">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-223">Follow these steps:</span></span>

1. <span data-ttu-id="1f9a6-224">Créer une nouvelle page ASP.NET nommée ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="1f9a6-225">Faites glisser un ScriptManager sur la page.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="1f9a6-226">Faites glisser un contrôle de zone de texte sur la page.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="1f9a6-227">Faites glisser un contrôle de bouton sur la page.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="1f9a6-228">Dans la fenêtre Propriétés, modifiez la propriété ID de bouton à la valeur <em>btnSave</em> et la propriété de texte sur la valeur *enregistrer\**.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="1f9a6-229">Nous avons créé une page avec un contrôle ASP.NET TextBox et Button standard.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="1f9a6-230">Ensuite, nous devons étendre le contrôle de zone de texte avec l’extendeur DisabledButton :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="1f9a6-231">Sélectionnez le **ajouter un extendeur** option pour ouvrir la boîte de dialogue Assistant extendeur de tâche (voir Figure 5).</span><span class="sxs-lookup"><span data-stu-id="1f9a6-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="1f9a6-232">Notez que la boîte de dialogue inclut notre extendeur DisabledButton personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="1f9a6-233">Sélectionnez l’extendeur DisabledButton et cliquez sur le **OK** bouton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="1f9a6-234">[![La boîte de dialogue Assistant extendeur](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="1f9a6-235">**Figure 05**: boîte de dialogue de l’Assistant d’extendeur ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="1f9a6-236">Enfin, nous pouvons définir les propriétés de l’extendeur DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="1f9a6-237">Vous pouvez modifier les propriétés de l’extendeur DisabledButton en modifiant les propriétés du contrôle de zone de texte :</span><span class="sxs-lookup"><span data-stu-id="1f9a6-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="1f9a6-238">Sélectionnez la zone de texte dans le concepteur.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="1f9a6-239">Dans la fenêtre Propriétés, développez le nœud d’extendeurs (voir Figure 6).</span><span class="sxs-lookup"><span data-stu-id="1f9a6-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="1f9a6-240">Affectez la valeur *enregistrer* à la propriété DisabledText et la valeur *btnSave* à la propriété TargetButtonID.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="1f9a6-241">[![Définition des propriétés d’extendeur](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="1f9a6-242">**Figure 06**: définition des propriétés d’extendeur ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="1f9a6-243">Lorsque vous exécutez la page (en appuyant sur F5), le contrôle Button est initialement désactivé.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="1f9a6-244">Dès que vous commencez la saisie de texte dans la zone de texte, le bouton de contrôle est activé (voir Figure 7).</span><span class="sxs-lookup"><span data-stu-id="1f9a6-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="1f9a6-245">[![L’extendeur DisabledButton en action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="1f9a6-246">**Figure 07**: The DisabledButton l’extendeur en action ([cliquez pour afficher l’image en taille réelle](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="1f9a6-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="1f9a6-247">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="1f9a6-247">Summary</span></span>

<span data-ttu-id="1f9a6-248">L’objectif de ce didacticiel est d’expliquer comment vous pouvez étendre les outils de contrôle AJAX avec les contrôles d’extendeur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="1f9a6-249">Dans ce didacticiel, nous avons créé un extendeur de contrôle DisabledButton simple.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="1f9a6-250">Nous avons implémenté Cet extendeur en créant une classe DisabledButtonExtender, un comportement DisabledButtonBehavior JavaScript et une classe DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="1f9a6-251">Vous suivez un ensemble similaire d’étapes chaque fois que vous créez un extendeur de contrôle personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1f9a6-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1f9a6-252">[Précédent](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Suivant](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1f9a6-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
