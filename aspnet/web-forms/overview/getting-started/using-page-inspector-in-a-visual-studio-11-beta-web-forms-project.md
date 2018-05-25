---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: À l’aide de l’inspecteur de Page pour Visual Studio 2012 dans ASP.NET Web Forms | Documents Microsoft
author: rick-anderson
description: L’inspecteur de page pour Visual Studio 2012 est un outil de développement web avec un navigateur intégré. Sélectionner un élément dans le navigateur intégré et l’inspecteur de Page...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: ca8a3c194577766e56d0604323fef567d539316c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="b1195-104">À l’aide de l’inspecteur de Page pour Visual Studio 2012 dans ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="b1195-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="b1195-105">par Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="b1195-105">by Tim Ammann</span></span>

> <span data-ttu-id="b1195-106">L’inspecteur de page pour Visual Studio 2012 est un outil de développement web avec un navigateur intégré.</span><span class="sxs-lookup"><span data-stu-id="b1195-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="b1195-107">Sélectionner un élément dans le navigateur intégré et l’inspecteur de Page instantanément met en surbrillance l’élément source et CSS.</span><span class="sxs-lookup"><span data-stu-id="b1195-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="b1195-108">Vous pouvez rechercher n’importe quelle page dans votre application, rapidement rechercher les sources de balisage rendu et utiliser les outils de navigation à droite dans l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1195-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="b1195-109">Ce didacticiel shwos comment activer le Mode d’Inspection rapidement localiser et modifier les règles CSS et du texte dans votre projet web.</span><span class="sxs-lookup"><span data-stu-id="b1195-109">This tutorial shwos how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="b1195-110">Ce didacticiel utilise un projet d’Application Web Forms, mais vous pouvez également utiliser l’inspecteur de Page pour les projets de Site Web et [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span><span class="sxs-lookup"><span data-stu-id="b1195-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="b1195-111">Le didacticiel comprend les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="b1195-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="b1195-112">Composants requis</span><span class="sxs-lookup"><span data-stu-id="b1195-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="b1195-113">Créer une Application Web</span><span class="sxs-lookup"><span data-stu-id="b1195-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="b1195-114">Utilisation de l’inspecteur de Page pour afficher l’Application</span><span class="sxs-lookup"><span data-stu-id="b1195-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="b1195-115">Activer le Mode d’Inspection</span><span class="sxs-lookup"><span data-stu-id="b1195-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="b1195-116">Utilisation de l’inspecteur de Page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="b1195-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="b1195-117">Mode d’inspection et de la fenêtre de code HTML</span><span class="sxs-lookup"><span data-stu-id="b1195-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="b1195-118">Aperçu des modifications dans la fenêtre Styles CSS</span><span class="sxs-lookup"><span data-stu-id="b1195-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="b1195-119">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="b1195-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="b1195-120">À l’aide du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="b1195-120">Using the CSS Color Picker</span></span>](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="b1195-121">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b1195-121">Prerequisites</span></span>

- <span data-ttu-id="b1195-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="b1195-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="b1195-123">Pour obtenir la dernière version de l’inspecteur de Page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le Kit de développement logiciel Azure pour .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="b1195-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="b1195-124">L’inspecteur de page est fourni avec les outils de développement Web de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b1195-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="b1195-125">La version la plus récente est 1.3.</span><span class="sxs-lookup"><span data-stu-id="b1195-125">The latest version is 1.3.</span></span> <span data-ttu-id="b1195-126">Pour vérifier quelle version vous avez, exécutez Visual Studio, puis sélectionnez **sur Microsoft Visual Studio** à partir de la **aide** menu.</span><span class="sxs-lookup"><span data-stu-id="b1195-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="b1195-127">Créer une Application Web</span><span class="sxs-lookup"><span data-stu-id="b1195-127">Create a Web Application</span></span>

<span data-ttu-id="b1195-128">Tout d’abord, vous allez créer une application web que vous utiliserez avec l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="b1195-129">Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="b1195-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="b1195-130">Sur la gauche, développez **Visual C#**, sélectionnez **Web**, puis sélectionnez **Application ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="b1195-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nouvelle Application Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="b1195-132">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1195-132">Click **OK**.</span></span>

<span data-ttu-id="b1195-133">L’application s’ouvre dans **Source** vue.</span><span class="sxs-lookup"><span data-stu-id="b1195-133">The application opens in **Source** view.</span></span>

![Nouvelle Application Web Forms en mode Source](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="b1195-135">Maintenant que vous avez une application fonctionne avec, vous pouvez utiliser l’inspecteur de Page pour examiner et modifier.</span><span class="sxs-lookup"><span data-stu-id="b1195-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="b1195-136">Utilisation de l’inspecteur de Page pour afficher l’Application</span><span class="sxs-lookup"><span data-stu-id="b1195-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="b1195-137">Ensuite, vous allez afficher l’application avec l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="b1195-138">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **afficher dans l’inspecteur de Page**.</span><span class="sxs-lookup"><span data-stu-id="b1195-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Afficher dans l’inspecteur de Page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="b1195-140">Par défaut, au démarrage de l’inspecteur de Page pour la première fois, il est ancré dans une fenêtre étroite sur le côté gauche de l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1195-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="b1195-141">Laisser ancrée sur le côté gauche et définissez-le sur une largeur qui est à l’aise pour vous, ou dans un des zones outil ancre sur le haut, bas ou à droite :</span><span class="sxs-lookup"><span data-stu-id="b1195-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Position d’ancrage de l’inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="b1195-143">Si vous détacher de la fenêtre de l’inspecteur de Page, vous pouvez le placer en dehors de Visual Studio, ou même sur un deuxième écran si vous en avez.</span><span class="sxs-lookup"><span data-stu-id="b1195-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="b1195-144">Toutefois, dans la commande ALT + TAB entre l’inspecteur de Page et de Visual Studio lorsque la fenêtre de l’inspecteur de Page n’est pas ancrée, accédez à **outils** &gt; **Options** &gt;  **Environnement** &gt; **onglets et fenêtres**et sous **onglet bien**, désactivez la case à cocher appelée **fenêtres d’outils flottante toujours au premier plan de la fenêtre principale**:</span><span class="sxs-lookup"><span data-stu-id="b1195-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Décochez la case de windows outil flottante ALT + TAB entre Visual Studio et la fenêtre de l’inspecteur de Page non ancrée](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="b1195-146">Le volet supérieur de la fenêtre de l’inspecteur de Page affiche la page actuelle dans une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="b1195-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="b1195-147">Le volet inférieur affiche la page dans le balisage HTML sur la gauche, et certains onglets à droite qui vous permettent d’inspecter les différents aspects de la page.</span><span class="sxs-lookup"><span data-stu-id="b1195-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="b1195-148">Le volet inférieur est similaire à la [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b1195-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="b1195-149">(Toutefois, contrairement aux outils de développement, vous pouvez utiliser l’inspecteur de Page à droite dans Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="b1195-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="b1195-151">Dans ce didacticiel, vous allez utiliser le volet Explorateur de l’inspecteur de Page et le **HTML** et **Styles** onglets pour vous aider à rapidement accéder et apporter des modifications à l’application.</span><span class="sxs-lookup"><span data-stu-id="b1195-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="b1195-152">Activer le Mode d’Inspection</span><span class="sxs-lookup"><span data-stu-id="b1195-152">Enable Inspection Mode</span></span>

<span data-ttu-id="b1195-153">Ensuite, vous verrez comment le Mode d’Inspection de l’inspecteur de Page fonctionne.</span><span class="sxs-lookup"><span data-stu-id="b1195-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="b1195-154">Dans la fenêtre de l’inspecteur de Page, cliquez sur le **inspecter** bouton.</span><span class="sxs-lookup"><span data-stu-id="b1195-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="b1195-156">Pour afficher le mode d’inspection en action, déplacez votre souris sur différentes parties de la page dans la fenêtre du navigateur l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="b1195-157">Comme vous le faites, le pointeur de la souris se transforme en signe plus grand, et la mise en surbrillance de l’élément en dessous :</span><span class="sxs-lookup"><span data-stu-id="b1195-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Survol de wrapper de div.content](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="b1195-159">Lorsque vous déplacez le pointeur de la souris, notez que</span><span class="sxs-lookup"><span data-stu-id="b1195-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="b1195-160">Le contenu de **Source** afficher change pour afficher le balisage correspondant à l’élément sélectionné dans la page.</span><span class="sxs-lookup"><span data-stu-id="b1195-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="b1195-161">La balise pertinente est mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="b1195-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="b1195-162">Si la source est dans un autre fichier, ce fichier est ouvert en mode Source avec la balise pertinente mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="b1195-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="b1195-163">Le balisage affiché dans le **HTML** onglet dans l’inspecteur de Page également modifie pour correspondre à l’élément sélectionné dans la page.</span><span class="sxs-lookup"><span data-stu-id="b1195-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="b1195-164">Dans le **HTML** onglet, le balisage approprié est indiqué.</span><span class="sxs-lookup"><span data-stu-id="b1195-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="b1195-165">Le **Styles** onglet affiche les règles CSS relatives à la sélection actuelle.</span><span class="sxs-lookup"><span data-stu-id="b1195-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="b1195-166">Utilisation de l’inspecteur de Page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="b1195-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="b1195-167">Vous voyez à présent comment vous pouvez utiliser l’inspecteur de Page pour rechercher et d’apporter des modifications à la balise ou de texte dont l’emplacement ne soit pas immédiatement évident.</span><span class="sxs-lookup"><span data-stu-id="b1195-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="b1195-168">Placer l’inspecteur de Page en Mode d’Inspection, puis faites défiler vers le bas de la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="b1195-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="b1195-169">Dès que vous entrez la zone de pied de page, l’inspecteur de Page ouvre le *Site.Master* fichier de disposition dans **Source** vue sous un onglet temporaire à droite des autres onglets et met en surbrillance de la section du maître page que vous avez avez sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b1195-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="b1195-170">Cela vous indique comment l’inspecteur de Page peuvent rechercher et afficher le contenu sur une page qui proviennent en fait un fichier différent de celui que vous avez ouvert.</span><span class="sxs-lookup"><span data-stu-id="b1195-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Met en évidence de pied de page en Mode d’Inspection](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="b1195-172">Dans la fenêtre du navigateur l’inspecteur de Page, placez le pointeur de la souris au-dessus de la ligne avec les informations de copyright <a id="a"> </a>Notez.</span><span class="sxs-lookup"><span data-stu-id="b1195-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="b1195-173">Dans le *Site.Master* page, la ligne correspondante est mise en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="b1195-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Ligne de pied de page copyright mis en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="b1195-175">Ajouter du texte à la fin de la ligne dans le *Site.Master* fichier.</span><span class="sxs-lookup"><span data-stu-id="b1195-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="b1195-176">&lt;p&gt;&amp;copier ; &lt;% : DateTime.Now.Year %&gt; -mon Rocks d’Application ASP.NET !&lt; /p&gt;</span><span class="sxs-lookup"><span data-stu-id="b1195-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="b1195-177">Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre de navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Mon Rocks d’Application ASP.NET !](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="b1195-179">Vous pouvez avoir pensions que le pied de page sur la *Default.aspx* page, mais il est avéré dans la page maître et l’inspecteur de Page trouvé pour vous.</span><span class="sxs-lookup"><span data-stu-id="b1195-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="b1195-180">Mode d’inspection et de la fenêtre de code HTML</span><span class="sxs-lookup"><span data-stu-id="b1195-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="b1195-181">Ensuite, vous aurez un coup de œil rapide à la fenêtre de code HTML et comment il mappe des éléments pour vous.</span><span class="sxs-lookup"><span data-stu-id="b1195-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="b1195-182">Placez l’inspecteur de Page en Mode d’Inspection.</span><span class="sxs-lookup"><span data-stu-id="b1195-182">Put Page Inspector in Inspection Mode.</span></span>

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="b1195-184">Cliquez sur la partie supérieure de la page indiquant que « votre logo ici ».</span><span class="sxs-lookup"><span data-stu-id="b1195-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="b1195-185">Vous examinez un élément particulier plus en détail, afin de l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.</span><span class="sxs-lookup"><span data-stu-id="b1195-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="b1195-186">Maintenant déplacer le pointeur de la souris vers le **HTML** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="b1195-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="b1195-187">Lorsque vous déplacez le pointeur de la souris, l’inspecteur de Page décrit l’élément dans le **HTML** fenêtre et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="b1195-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Fenêtre HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="b1195-189">Comme auparavant, l’inspecteur de Page ouvre le *Site.Master* fichier pour vous dans un onglet temporaire. Cliquez sur l’onglet Site.Master, et le balisage correspondant est mis en surbrillance dans le &lt;en-tête&gt; section :</span><span class="sxs-lookup"><span data-stu-id="b1195-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Balise en surbrillance](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="b1195-191">Aperçu des modifications dans la fenêtre Styles CSS</span><span class="sxs-lookup"><span data-stu-id="b1195-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="b1195-192">Ensuite, vous verrez comment vous pouvez utiliser l’inspecteur de Page **Styles** fenêtre pour afficher un aperçu des modifications apportées à CSS.</span><span class="sxs-lookup"><span data-stu-id="b1195-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="b1195-193">Cliquez sur le **inspecter** bouton à placer l’inspecteur de Page en Mode d’Inspection.</span><span class="sxs-lookup"><span data-stu-id="b1195-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b1195-194">Dans la fenêtre de navigateur de l’inspecteur de Page, déplacez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b1195-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Vous pointez sur des éléments](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="b1195-196">Cliquez une fois dans la section div.content-wrapper, puis déplacez le pointeur de la souris pour la **Styles** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="b1195-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="b1195-197">Dans le sélecteur de classe wrapper .content .featured, désactivez et activez la case à cocher pour la propriété de couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="b1195-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Couleur d’arrière-plan clair](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="b1195-199">Notez comment la modification affiche un aperçu d’instantanément dans la fenêtre de navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="b1195-200">Sélectionnez la case à cocher à nouveau, puis double-cliquez sur la valeur de propriété et la remplacer par `red`.</span><span class="sxs-lookup"><span data-stu-id="b1195-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="b1195-201">La modification s’affiche immédiatement :</span><span class="sxs-lookup"><span data-stu-id="b1195-201">The change shows immediately:</span></span>

![Couleur d’arrière-plan rouge](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="b1195-203">Le **Styles** rend fenêtre faciles à tester et afficher un aperçu de CSS change avant de valider les modifications apportées au style sheet lui-même.</span><span class="sxs-lookup"><span data-stu-id="b1195-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="b1195-204">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="b1195-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="b1195-205">Cette fonctionnalité requiert la version 1.3 de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-205">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="b1195-206">La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et voir les modifications immédiatement dans le navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="b1195-207">Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.</span><span class="sxs-lookup"><span data-stu-id="b1195-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="b1195-208">Dans le navigateur de l’inspecteur de Page, déplacez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b1195-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="b1195-209">Cliquez une fois pour sélectionner cet élément.</span><span class="sxs-lookup"><span data-stu-id="b1195-209">Click once to select this element.</span></span>

<span data-ttu-id="b1195-210">Le **styles** fenêtre affiche toutes les règles CSS pour cet élément.</span><span class="sxs-lookup"><span data-stu-id="b1195-210">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="b1195-211">Faites défiler jusqu'à un sélecteur de classe rechercher .featured .content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="b1195-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="b1195-212">Cliquez sur « .featured .content-wrapper ».</span><span class="sxs-lookup"><span data-stu-id="b1195-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="b1195-213">L’inspecteur de page ouvre le fichier CSS qui définit ce style (Site.css) et met en surbrillance le style CSS correspondant.</span><span class="sxs-lookup"><span data-stu-id="b1195-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Fichier CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="b1195-215">Maintenant modifiez la valeur de `background-color` « rouge ».</span><span class="sxs-lookup"><span data-stu-id="b1195-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="b1195-216">La modification s’affiche immédiatement dans le navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="b1195-216">The change appears immediately in the Page Inspector browser.</span></span>

![Navigateur de l’inspecteur de page](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="b1195-218">À l’aide du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="b1195-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="b1195-219">Ensuite, vous allez apprendre à utiliser l’inspecteur de Page pour rechercher rapidement et de modifier le code CSS texte mis en surbrillance dans l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="b1195-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="b1195-220">Dans cet exemple, vous avez décidé que vous ne la surbrillance bleue et souhaitez Remplacez-la par une autre couleur.</span><span class="sxs-lookup"><span data-stu-id="b1195-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="b1195-221">Cliquez sur le **inspecter** bouton.</span><span class="sxs-lookup"><span data-stu-id="b1195-221">Click the **Inspect** button.</span></span>

![Inspecter l’élément](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="b1195-223">Dans la fenêtre de navigateur de l’inspecteur de Page, déplacez le pointeur de la souris sur la mise en surbrillance « vidéos, didacticiels et exemples » texte afin que le code CSS « marquer « étiquette s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b1195-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Vous pointez sur l’élément de la marque](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="b1195-225">Cliquez sur le texte pour le sélectionner.</span><span class="sxs-lookup"><span data-stu-id="b1195-225">Click the text to select it.</span></span> <span data-ttu-id="b1195-226">Le sélecteur de marque CSS correspondant s’affiche au bas de la **Styles** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="b1195-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Marquer le sélecteur dans la fenêtre Styles](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="b1195-228">Cliquez sur le sélecteur de marque.</span><span class="sxs-lookup"><span data-stu-id="b1195-228">Click the mark selector.</span></span> <span data-ttu-id="b1195-229">Cette opération ouvre le *Site.css* fichier de l’application web.</span><span class="sxs-lookup"><span data-stu-id="b1195-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="b1195-230">Cliquez sur l’onglet Site.css et le CSS correspondant pour le sélecteur est mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="b1195-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Marquer le sélecteur dans la feuille de style](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="b1195-232">Sélectionnez et supprimez la ligne avec la propriété de couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="b1195-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="b1195-233">Vous allez maintenant utiliser le nouveau sélecteur de couleurs CSS de 2012 Visual Studio pour choisir une nouvelle couleur pour le **marquer** propriété de couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="b1195-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="b1195-234">À l’aide du sélecteur de couleurs CSS de Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b1195-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="b1195-235">L’éditeur de CSS dans Visual Studio 2012 a un sélecteur de couleurs qui permet de facilement choisir et insérer des couleurs.</span><span class="sxs-lookup"><span data-stu-id="b1195-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="b1195-236">Il possède une barre de couleur simple et un sélecteur « pop-down » qui offre un contrôle plus précis.</span><span class="sxs-lookup"><span data-stu-id="b1195-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="b1195-237">Le sélecteur de couleurs inclut une palette standard de couleurs, prend en charge les noms de couleurs standard, les codes de hachage, les couleurs RVB, RGBA, HSL et HSLA et conserve une liste des couleurs que vous avez utilisé le plus récemment dans le document.</span><span class="sxs-lookup"><span data-stu-id="b1195-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="b1195-238">Sur la ligne où la propriété de couleur d’arrière-plan a été, tapez « bc » et appuyez une fois sur la flèche vers le bas.</span><span class="sxs-lookup"><span data-stu-id="b1195-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="b1195-239">Lorsque vous tapez le premier caractère de chaque mot dans une propriété séparées par des tirets comme « couleur d’arrière-plan », IntelliSense filtre la liste pour afficher uniquement les propriétés qui correspondent à :</span><span class="sxs-lookup"><span data-stu-id="b1195-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![IntelliSense filtré des valeurs](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="b1195-241">Maintenant, tapez un signe deux-points.</span><span class="sxs-lookup"><span data-stu-id="b1195-241">Now type a colon.</span></span> <span data-ttu-id="b1195-242">Lorsque vous procédez ainsi, le nom de propriété de couleur d’arrière-plan complète est inséré.</span><span class="sxs-lookup"><span data-stu-id="b1195-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="b1195-243">Type **#** ou **RVB (**, et la barre de sélecteur de couleurs s’affiche :</span><span class="sxs-lookup"><span data-stu-id="b1195-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![La barre de sélecteur de couleurs CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="b1195-245">Pour voir comment fonctionne de la barre de sélecteur de couleurs, cliquez sur les couleurs avec le pointeur de la souris, ou appuyez sur la touche flèche bas et puis utiliser les touches de direction gauche et droite pour parcourir les couleurs.</span><span class="sxs-lookup"><span data-stu-id="b1195-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="b1195-246">Lorsque vous visitez une couleur, la valeur correspondante pour la propriété de couleur d’arrière-plan est affichée en aperçu :</span><span class="sxs-lookup"><span data-stu-id="b1195-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![valeur de propriété de couleur d’arrière-plan aperçu](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="b1195-248">À ce stade, vous pouvez appuyer sur ENTRÉE pour sélectionner la valeur, puis un point-virgule ( ;) pour terminer l’entrée CSS.</span><span class="sxs-lookup"><span data-stu-id="b1195-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="b1195-249">Pour l’instant, passez à la section suivante afin que vous pouvez voir comment le sélecteur de couleur pop-bas fonctionne.</span><span class="sxs-lookup"><span data-stu-id="b1195-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="b1195-250">À l’aide du sélecteur de couleur Pop en bas</span><span class="sxs-lookup"><span data-stu-id="b1195-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="b1195-251">Lorsque la barre de couleurs ne dispose pas la couleur exacte que vous recherchez, vous pouvez utiliser le sélecteur de couleurs pop vers le bas.</span><span class="sxs-lookup"><span data-stu-id="b1195-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="b1195-252">Pour l’ouvrir, cliquez sur le chevron double à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche vers le bas une ou deux fois sur le clavier.</span><span class="sxs-lookup"><span data-stu-id="b1195-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Sélecteur de couleurs CSS Pop-bas](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="b1195-254">Cliquez sur une couleur dans la barre verticale sur la droite.</span><span class="sxs-lookup"><span data-stu-id="b1195-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="b1195-255">Cela indique un dégradé de cette couleur dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="b1195-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="b1195-256">Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée, ou cliquez sur n’importe quel point dans la fenêtre principale pour choisir avec une précision supérieure.</span><span class="sxs-lookup"><span data-stu-id="b1195-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="b1195-257">Si votre écran de l’ordinateur que vous souhaitez utiliser est une couleur (ne doit pas nécessairement être à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil Pipette dans la partie inférieure droite.</span><span class="sxs-lookup"><span data-stu-id="b1195-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="b1195-258">Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="b1195-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="b1195-259">Ainsi, modifications de couleur aux valeurs RGBA, car le format RVBA peut représenter l’opacité.</span><span class="sxs-lookup"><span data-stu-id="b1195-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="b1195-260">Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de la couleur d’arrière-plan dans le *Site.css* fichier.</span><span class="sxs-lookup"><span data-stu-id="b1195-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="b1195-261">La barre de mise à jour de l’inspecteur de Page</span><span class="sxs-lookup"><span data-stu-id="b1195-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="b1195-262">L’inspecteur de page détecte immédiatement la modification apportée à la *Site.css* fichier (ou à n’importe quel fichier dans l’application) et affiche une alerte dans une barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="b1195-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barre de mise à jour](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="b1195-264">Pour enregistrer tous vos fichiers et actualiser le navigateur de l’inspecteur de Page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="b1195-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="b1195-265">La modification de la couleur de surbrillance s’affiche dans le navigateur :</span><span class="sxs-lookup"><span data-stu-id="b1195-265">The change in the highlight color appears in the browser:</span></span>

![Couleur de surbrillance modifié](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="b1195-267">Notez que vous actualisé aisément le navigateur de l’inspecteur de Page directement à partir de l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1195-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="b1195-268">Au lieu d’un navigateur externe à l’aide de l’inspecteur de Page vous permet de rester dans l’éditeur lorsque vous développez vos applications web.</span><span class="sxs-lookup"><span data-stu-id="b1195-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="b1195-269">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="b1195-269">See Also</span></span>

<span data-ttu-id="b1195-270">[Présentation de l’inspecteur de Page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vidéo Channel 9)</span><span class="sxs-lookup"><span data-stu-id="b1195-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="b1195-271">[Messages d’erreur de l’inspecteur de page](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="b1195-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
