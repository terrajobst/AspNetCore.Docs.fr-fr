---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: À l’aide de l’inspecteur de Page dans ASP.NET MVC | Documents Microsoft
author: rick-anderson
description: L’inspecteur de page dans Visual Studio 2012 est un outil de développement web avec un navigateur intégré. Sélectionner un élément dans le navigateur intégré et l’inspecteur de Page i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="f8ece-104">Utilisation de l'Inspecteur de page dans ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f8ece-104">Using Page Inspector in ASP.NET MVC</span></span>
====================
<span data-ttu-id="f8ece-105">par Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="f8ece-105">by Tim Ammann</span></span>

> <span data-ttu-id="f8ece-106">L’inspecteur de page dans Visual Studio 2012 est un outil de développement web avec un navigateur intégré.</span><span class="sxs-lookup"><span data-stu-id="f8ece-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="f8ece-107">Sélectionner un élément dans le navigateur intégré et l’inspecteur de Page instantanément met en surbrillance l’élément source et CSS.</span><span class="sxs-lookup"><span data-stu-id="f8ece-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="f8ece-108">Vous pouvez rechercher n’importe quelle vue MVC, rapidement rechercher les sources de balisage rendu et utiliser les outils de navigation à droite dans l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8ece-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="f8ece-109">Regardez la vidéo</span><span class="sxs-lookup"><span data-stu-id="f8ece-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="f8ece-110">Ce didacticiel montre comment activer le Mode d’Inspection, puis rapidement localiser et modifier le balisage et CSS dans votre projet web.</span><span class="sxs-lookup"><span data-stu-id="f8ece-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="f8ece-111">Ce didacticiel utilise un projet MVC, mais vous pouvez également utiliser l’inspecteur de Page pour [Web Forms](https://go.microsoft.com/?linkid=9802001) et d’autres applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f8ece-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="f8ece-112">Le didacticiel comprend les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="f8ece-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="f8ece-113">Composants requis</span><span class="sxs-lookup"><span data-stu-id="f8ece-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="f8ece-114">Créer une Application Web</span><span class="sxs-lookup"><span data-stu-id="f8ece-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="f8ece-115">Utilisation de l’inspecteur de Page pour accéder à une vue</span><span class="sxs-lookup"><span data-stu-id="f8ece-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="f8ece-116">Activer le Mode d’Inspection</span><span class="sxs-lookup"><span data-stu-id="f8ece-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="f8ece-117">Utilisation de l’inspecteur de Page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="f8ece-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="f8ece-118">Mode d’inspection et de la fenêtre de code HTML</span><span class="sxs-lookup"><span data-stu-id="f8ece-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="f8ece-119">Aperçu des modifications dans la fenêtre Styles CSS</span><span class="sxs-lookup"><span data-stu-id="f8ece-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="f8ece-120">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="f8ece-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="f8ece-121">À l’aide du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="f8ece-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="f8ece-122">Mappage entre les éléments de Page dynamique et JavaScript</span><span class="sxs-lookup"><span data-stu-id="f8ece-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f8ece-123">Prérequis</span><span class="sxs-lookup"><span data-stu-id="f8ece-123">Prerequisites</span></span>

- <span data-ttu-id="f8ece-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="f8ece-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="f8ece-125">Pour obtenir la dernière version de l’inspecteur de Page, utilisez [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) pour installer le Kit de développement logiciel Windows Azure pour .NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="f8ece-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>


<span data-ttu-id="f8ece-126">L’inspecteur de page est fourni avec les outils de développement Web de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f8ece-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="f8ece-127">La version la plus récente est 1.3.</span><span class="sxs-lookup"><span data-stu-id="f8ece-127">The latest version is 1.3.</span></span> <span data-ttu-id="f8ece-128">Pour vérifier quelle version vous avez, exécutez Visual Studio, puis sélectionnez **sur Microsoft Visual Studio** à partir de la **aide** menu.</span><span class="sxs-lookup"><span data-stu-id="f8ece-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="f8ece-129">Créer une Application Web</span><span class="sxs-lookup"><span data-stu-id="f8ece-129">Create a Web Application</span></span>

<span data-ttu-id="f8ece-130">Commencez par créer une application web que vous utiliserez avec l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="f8ece-131">Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f8ece-132">Sur la gauche, développez **Visual C#**, sélectionnez **Web**, puis sélectionnez **Application de Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nouvelle Application ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="f8ece-134">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-134">Click **OK**.</span></span>

<span data-ttu-id="f8ece-135">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **Application Internet**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="f8ece-136">Laissez **Razor** en tant que le moteur d’affichage par défaut.</span><span class="sxs-lookup"><span data-stu-id="f8ece-136">Leave **Razor** as the default view engine.</span></span>

![Nouveau projet ASP.NET MVC - Application Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="f8ece-138">L’application s’ouvre dans **Source** vue.</span><span class="sxs-lookup"><span data-stu-id="f8ece-138">The application opens in **Source** view.</span></span>

![Nouvelle Application ASP.NET MVC en mode Source](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="f8ece-140">Maintenant que vous avez une application fonctionne avec, vous pouvez utiliser l’inspecteur de Page pour examiner et modifier.</span><span class="sxs-lookup"><span data-stu-id="f8ece-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="f8ece-141">Utilisation de l’inspecteur de Page pour accéder à une vue</span><span class="sxs-lookup"><span data-stu-id="f8ece-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="f8ece-142">Dans Visual Studio 2012, vous pouvez cliquer sur n’importe quelle vue dans votre projet, sélectionnez **afficher dans l’inspecteur de Page**, et l’inspecteur de Page de trouver l’itinéraire et afficher la page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="f8ece-143">Dans **l’Explorateur de solutions**, développez le **vues** dossier, puis le **accueil** dossier.</span><span class="sxs-lookup"><span data-stu-id="f8ece-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="f8ece-144">Cliquez avec le bouton droit sur le fichier Index.cshtml et choisissez **afficher dans l’inspecteur de Page**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Afficher les Index.cshtml dans l’inspecteur de Page](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="f8ece-146">Par défaut, l’inspecteur de Page est ancré dans une fenêtre sur le côté gauche de l’environnement Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8ece-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="f8ece-147">Si vous préférez, vous pouvez ancrer ailleurs ou détacher la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="f8ece-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="f8ece-148">Consultez [Comment : réorganiser et ancrer des fenêtres](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8ece-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="f8ece-149">Le volet supérieur de la fenêtre de l’inspecteur de Page affiche la page actuelle dans une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="f8ece-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="f8ece-150">Le volet inférieur affiche la page dans le balisage HTML, ainsi que certains onglets qui vous permettent d’inspecter les différents aspects de la page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="f8ece-151">Le volet inférieur est similaire à la [outils de développement F12](https://msdn.microsoft.com/ie/aa740478) dans Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f8ece-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Application ASP.NET MVC dans l’inspecteur de Page](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="f8ece-153">Dans ce didacticiel, vous allez utiliser le **HTML** et **Styles** onglets pour naviguer rapidement et apporter des modifications à l’application.</span><span class="sxs-lookup"><span data-stu-id="f8ece-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="f8ece-154">Mode EnableInspection</span><span class="sxs-lookup"><span data-stu-id="f8ece-154">EnableInspection Mode</span></span>

<span data-ttu-id="f8ece-155">Pour placer l’inspecteur de Page en Mode d’Inspection, cliquez sur le **inspecter** bouton.</span><span class="sxs-lookup"><span data-stu-id="f8ece-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="f8ece-156">En Mode d’Inspection, lorsque vous placez le pointeur de la souris sur n’importe quelle partie de la page rendue, le code ou le balisage source correspondant est mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="f8ece-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Basculer en Mode d’Inspection](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="f8ece-158">Maintenant, déplacez votre souris sur différentes parties de la page dans l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="f8ece-159">Comme vous le faites, le pointeur de la souris se transforme en signe plus grand, et la mise en surbrillance de l’élément en dessous :</span><span class="sxs-lookup"><span data-stu-id="f8ece-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Survol de wrapper de div.content](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="f8ece-161">Lorsque vous déplacez le pointeur de la souris, Visual Studio met en surbrillance la syntaxe Razor correspondante dans le fichier source.</span><span class="sxs-lookup"><span data-stu-id="f8ece-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="f8ece-162">Si l’élément HTML provient d’un autre fichier source, Visual Studio ouvre automatiquement le fichier.</span><span class="sxs-lookup"><span data-stu-id="f8ece-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="f8ece-163">Dans l’inspecteur de Page, le **HTML** onglet montre le code HTML qui a été généré à partir de la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="f8ece-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="f8ece-164">Lorsque vous déplacez le pointeur de la souris, les éléments HTML sont mis en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="f8ece-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="f8ece-165">Le **Styles** onglet affiche les règles CSS pour l’élément.</span><span class="sxs-lookup"><span data-stu-id="f8ece-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="f8ece-166">Utilisation de l’inspecteur de Page pour apporter des modifications au balisage</span><span class="sxs-lookup"><span data-stu-id="f8ece-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="f8ece-167">L’inspecteur de page vous permet de rechercher les marques dont l’emplacement ne soit pas évident.</span><span class="sxs-lookup"><span data-stu-id="f8ece-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="f8ece-168">Vous pouvez modifier le balisage et voir les modifications résultantes.</span><span class="sxs-lookup"><span data-stu-id="f8ece-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="f8ece-169">Pour cela, cliquez sur **inspecter** puis faites défiler vers le bas de la page dans la fenêtre de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="f8ece-170">Lorsque vous déplacez le pointeur de la souris dans la zone de pied de page, l’inspecteur de Page ouvre le \_Layout.cshtml fichier et met en surbrillance de la section de la page de disposition que vous avez sélectionné.</span><span class="sxs-lookup"><span data-stu-id="f8ece-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="f8ece-171">Comme vous pouvez le voir, le pied de page est défini dans le fichier de disposition et pas la vue proprement dite.</span><span class="sxs-lookup"><span data-stu-id="f8ece-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Pied de page](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="f8ece-173">Maintenant déplacer le pointeur de la souris au-dessus de la ligne avec les informations de copyright <a id="a"> </a>Notez.</span><span class="sxs-lookup"><span data-stu-id="f8ece-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="f8ece-174">Dans le \_Layout.cshtml page, la ligne correspondante est mise en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="f8ece-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Ligne de pied de page copyright mis en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="f8ece-176">Ajouter du texte à la fin de la ligne dans le \_Layout.cshtml fichier.</span><span class="sxs-lookup"><span data-stu-id="f8ece-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="f8ece-177">&lt;p&gt;&amp;copier ; @DateTime.Now.Year -Mon Application ASP.NET MVC donne ! &lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="f8ece-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="f8ece-178">Maintenant, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour pour afficher les résultats dans la fenêtre de navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Mon Rocks d’Application ASP.NET !](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="f8ece-180">Vous pouvez penser que le pied de page définies dans Index.cshtml, mais il s’est avéré être dans le \_Layout.cshtml et l’inspecteur de Page trouvé pour vous.</span><span class="sxs-lookup"><span data-stu-id="f8ece-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="f8ece-181">Mode d’inspection et de la fenêtre de code HTML</span><span class="sxs-lookup"><span data-stu-id="f8ece-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="f8ece-182">Ensuite, vous aurez un coup de œil rapide à la fenêtre de code HTML et comment il mappe des éléments pour vous.</span><span class="sxs-lookup"><span data-stu-id="f8ece-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="f8ece-183">Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.</span><span class="sxs-lookup"><span data-stu-id="f8ece-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f8ece-184">Cliquez sur la partie supérieure de la page indiquant que « Votre logohere ».</span><span class="sxs-lookup"><span data-stu-id="f8ece-184">Click the top part of the page that says "Your logohere".</span></span> <span data-ttu-id="f8ece-185">Vous examinez un élément particulier plus en détail, afin de l’affichage dans la fenêtre du navigateur ne change plus lorsque vous déplacez le pointeur de la souris.</span><span class="sxs-lookup"><span data-stu-id="f8ece-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="f8ece-186">Maintenant déplacer le pointeur de la souris vers le **HTML** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="f8ece-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="f8ece-187">Lorsque vous déplacez le pointeur de la souris, l’inspecteur de Page décrit l’élément dans le **HTML** fenêtre et met en surbrillance l’élément correspondant dans la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="f8ece-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Fenêtre HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="f8ece-189">Comme auparavant, l’inspecteur de Page ouvre le \_fichier Layout.cshtml pour vous dans un onglet temporaire. Cliquez sur le \_Layout.cshtml les onglet temporaire et les balises correspondantes sont mises en surbrillance dans le &lt;en-tête&gt; section pour vous :</span><span class="sxs-lookup"><span data-stu-id="f8ece-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Balise en surbrillance](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="f8ece-191">Aperçu des modifications dans la fenêtre Styles CSS</span><span class="sxs-lookup"><span data-stu-id="f8ece-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="f8ece-192">Ensuite, vous allez utiliser l’inspecteur de Page **Styles** fenêtre pour afficher un aperçu des modifications apportées à CSS.</span><span class="sxs-lookup"><span data-stu-id="f8ece-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="f8ece-193">Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.</span><span class="sxs-lookup"><span data-stu-id="f8ece-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f8ece-194">Dans la fenêtre de navigateur de l’inspecteur de Page, déplacez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f8ece-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Survol de wrapper de div.content](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="f8ece-196">Cliquez une fois dans la section div.content-wrapper, puis déplacez le pointeur de la souris pour la **Styles** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="f8ece-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="f8ece-197">Le **styles** fenêtre affiche toutes les règles CSS pour cet élément.</span><span class="sxs-lookup"><span data-stu-id="f8ece-197">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f8ece-198">Faites défiler jusqu'à un sélecteur de classe rechercher .featured .content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="f8ece-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f8ece-199">Maintenant, désactivez la case à cocher pour la propriété de couleur d’arrière-plan.</span><span class="sxs-lookup"><span data-stu-id="f8ece-199">Now clear the checkbox for the background-color property.</span></span>

![Couleur d’arrière-plan clair](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="f8ece-201">Notez comment la modification affiche un aperçu d’instantanément dans la fenêtre de navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="f8ece-202">Sélectionnez la case à cocher à nouveau, puis double-cliquez sur la valeur de propriété et modifiez-la en rouge.</span><span class="sxs-lookup"><span data-stu-id="f8ece-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="f8ece-203">La modification s’affiche immédiatement :</span><span class="sxs-lookup"><span data-stu-id="f8ece-203">The change shows immediately:</span></span>

![Couleur d’arrière-plan rouge](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="f8ece-205">Le **Styles** rend fenêtre faciles à tester et afficher un aperçu de CSS change avant de valider les modifications apportées au style sheet lui-même.</span><span class="sxs-lookup"><span data-stu-id="f8ece-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="f8ece-206">Synchronisation automatique CSS</span><span class="sxs-lookup"><span data-stu-id="f8ece-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="f8ece-207">Cette fonctionnalité requiert la version 1.3 de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-207">This feature requires version 1.3 of Page Inspector.</span></span>


<span data-ttu-id="f8ece-208">La fonctionnalité de synchronisation automatique CSS vous permet de modifier directement un fichier CSS et voir les modifications immédiatement dans le navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="f8ece-209">Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.</span><span class="sxs-lookup"><span data-stu-id="f8ece-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="f8ece-210">Dans le navigateur de l’inspecteur de Page, déplacez le pointeur de la souris sur la section « Page d’accueil » jusqu'à ce que le **div.content-wrapper** étiquette s’affiche.</span><span class="sxs-lookup"><span data-stu-id="f8ece-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="f8ece-211">Cliquez une fois pour sélectionner cet élément.</span><span class="sxs-lookup"><span data-stu-id="f8ece-211">Click once to select this element.</span></span>

<span data-ttu-id="f8ece-212">Le **styles** fenêtre affiche toutes les règles CSS pour cet élément.</span><span class="sxs-lookup"><span data-stu-id="f8ece-212">The **Syles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="f8ece-213">Faites défiler jusqu'à un sélecteur de classe rechercher .featured .content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="f8ece-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="f8ece-214">Cliquez sur « .featured .content-wrapper ».</span><span class="sxs-lookup"><span data-stu-id="f8ece-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="f8ece-215">L’inspecteur de page ouvre le fichier CSS qui définit ce style (Site.css) et met en surbrillance le style CSS correspondant.</span><span class="sxs-lookup"><span data-stu-id="f8ece-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="f8ece-216">Maintenant modifiez la valeur de `background-color` « rouge ».</span><span class="sxs-lookup"><span data-stu-id="f8ece-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="f8ece-217">La modification s’affiche immédiatement dans le navigateur de l’inspecteur de Page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="f8ece-218">À l’aide du sélecteur de couleurs CSS</span><span class="sxs-lookup"><span data-stu-id="f8ece-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="f8ece-219">L’éditeur de CSS dans Visual Studio 2012 a un sélecteur de couleurs qui permet de facilement choisir et insérer des couleurs.</span><span class="sxs-lookup"><span data-stu-id="f8ece-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="f8ece-220">Le sélecteur de couleurs inclut une palette standard de couleurs, prend en charge les noms de couleurs standard, les codes de hachage, les couleurs RVB, RGBA, HSL et HSLA et conserve une liste des couleurs que vous avez utilisé le plus récemment dans le document.</span><span class="sxs-lookup"><span data-stu-id="f8ece-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="f8ece-221">Dans la section précédente, vous avez modifié la valeur de la `background-color` propriété.</span><span class="sxs-lookup"><span data-stu-id="f8ece-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="f8ece-222">Pour appeler le sélecteur de couleurs, placez le point d’insertion après le nom de la propriété et le type **#** ou **RVB (**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![La barre de sélecteur de couleurs CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="f8ece-224">Cliquez sur une couleur pour le sélectionner, ou appuyez sur la touche flèche bas et puis utiliser les touches de direction gauche et droite pour parcourir les couleurs.</span><span class="sxs-lookup"><span data-stu-id="f8ece-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="f8ece-225">Lorsque vous visitez une couleur, la valeur hexadécimale correspondante est affiché en aperçu :</span><span class="sxs-lookup"><span data-stu-id="f8ece-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![valeur de propriété de couleur d’arrière-plan aperçu](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="f8ece-227">Si la barre de couleurs n’a pas la couleur exacte de que votre choix, vous pouvez utiliser le sélecteur de couleur pop-bas.</span><span class="sxs-lookup"><span data-stu-id="f8ece-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="f8ece-228">Pour l’ouvrir, cliquez sur le chevron double à l’extrémité droite de la barre de couleurs, ou appuyez sur la flèche vers le bas une ou deux fois sur le clavier.</span><span class="sxs-lookup"><span data-stu-id="f8ece-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Sélecteur de couleurs CSS Pop-bas](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="f8ece-230">Cliquez sur une couleur dans la barre verticale sur la droite.</span><span class="sxs-lookup"><span data-stu-id="f8ece-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="f8ece-231">Cela indique un dégradé de cette couleur dans la fenêtre principale.</span><span class="sxs-lookup"><span data-stu-id="f8ece-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="f8ece-232">Choisissez une couleur directement à partir de la barre verticale en appuyant sur entrée, ou cliquez sur n’importe quel point dans la fenêtre principale pour choisir avec une précision supérieure.</span><span class="sxs-lookup"><span data-stu-id="f8ece-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="f8ece-233">Si votre écran de l’ordinateur que vous souhaitez utiliser est une couleur (ne doit pas nécessairement être à l’intérieur de l’interface utilisateur de Visual Studio), vous pouvez capturer sa valeur à l’aide de l’outil Pipette dans la partie inférieure droite.</span><span class="sxs-lookup"><span data-stu-id="f8ece-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="f8ece-234">Vous pouvez également modifier l’opacité d’une couleur en déplaçant le curseur en bas du sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="f8ece-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="f8ece-235">Cette opération modifications de valeurs RGBA, couleurs, car le format RVBA peut représenter l’opacité.</span><span class="sxs-lookup"><span data-stu-id="f8ece-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="f8ece-236">Une fois que vous avez choisi une couleur, appuyez sur entrée, puis tapez un point-virgule pour terminer l’entrée de la couleur d’arrière-plan dans le *Site.css* fichier.</span><span class="sxs-lookup"><span data-stu-id="f8ece-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="f8ece-237">La barre de mise à jour de l’inspecteur de Page</span><span class="sxs-lookup"><span data-stu-id="f8ece-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="f8ece-238">L’inspecteur de page détecte immédiatement la modification apportée à la *Site.css* de fichiers et affiche une alerte dans une barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f8ece-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barre de mise à jour](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="f8ece-240">Pour enregistrer tous vos fichiers et actualiser le navigateur de l’inspecteur de Page, appuyez sur Ctrl + Alt + Entrée ou cliquez sur la barre de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f8ece-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="f8ece-241">La modification de la couleur de surbrillance s’affiche dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="f8ece-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="f8ece-242">Mappage entre les éléments de Page dynamique et JavaScript</span><span class="sxs-lookup"><span data-stu-id="f8ece-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="f8ece-243">Dans les applications web modernes, éléments de la page sont souvent générées dynamiquement avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8ece-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="f8ece-244">Cela ne signifie aucune balise statique (HTML ou Razor) qui correspond à ces éléments de la page.</span><span class="sxs-lookup"><span data-stu-id="f8ece-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="f8ece-245">Avec la version 1.3, l’inspecteur de Page peuvent maintenant mapper les éléments qui ont été ajoutés dynamiquement à la page sur le code JavaScript correspondant.</span><span class="sxs-lookup"><span data-stu-id="f8ece-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="f8ece-246">Pour illustrer cette fonctionnalité, nous allons utiliser la [modèle d’Application de Page unique (SPA)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="f8ece-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f8ece-247">Le modèle de SPA nécessite le [ASP.NET et Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="f8ece-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>


<span data-ttu-id="f8ece-248">Dans Visual Studio, choisissez **fichier** &gt; **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="f8ece-249">Sur la gauche, développez **Visual C#**, sélectionnez **Web**, puis sélectionnez **Application de Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="f8ece-250">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-250">Click **OK**.</span></span>

<span data-ttu-id="f8ece-251">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez **Application à Page unique**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="f8ece-252">Dans l’Explorateur de solutions, développez le **vues** dossier, puis le **accueil** dossier.</span><span class="sxs-lookup"><span data-stu-id="f8ece-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="f8ece-253">Cliquez avec le bouton droit sur le fichier Index.cshtml et choisissez **afficher dans l’inspecteur de Page**.</span><span class="sxs-lookup"><span data-stu-id="f8ece-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="f8ece-254">La première qui est affichée dans le navigateur de l’inspecteur de Page est une page de connexion.</span><span class="sxs-lookup"><span data-stu-id="f8ece-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="f8ece-255">Cliquez sur « S’abonner » et un nom d’utilisateur et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="f8ece-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="f8ece-256">Une fois que vous vous inscrivez, l’application vous connecte et crée une liste de tâches avec des exemples d’éléments.</span><span class="sxs-lookup"><span data-stu-id="f8ece-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="f8ece-257">Cliquez sur **inspecter** à placer l’inspecteur de Page en Mode d’Inspection.</span><span class="sxs-lookup"><span data-stu-id="f8ece-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="f8ece-258">Dans le navigateur de l’inspecteur de Page, cliquez sur un des éléments d’action.</span><span class="sxs-lookup"><span data-stu-id="f8ece-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="f8ece-259">Notez que, au lieu de la mise en surbrillance en bleu, l’élément est surligné en orange, avec « JS » en regard du nom de l’élément.</span><span class="sxs-lookup"><span data-stu-id="f8ece-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="f8ece-260">Cela indique que l’élément a été créé dynamiquement dans le script.</span><span class="sxs-lookup"><span data-stu-id="f8ece-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="f8ece-261">En outre, un trait de soulignement orange s’affiche sur le **pile des appels** onglet. Cela indique que le **pile des appels** volet a plus d’informations sur l’élément.</span><span class="sxs-lookup"><span data-stu-id="f8ece-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="f8ece-262">Cliquez sur le **pile des appels** onglet. Le **pile des appels** volet affiche la pile des appels pour l’appel de JavaScript qui a créé l’élément.</span><span class="sxs-lookup"><span data-stu-id="f8ece-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="f8ece-263">Appels aux bibliothèques externes tels que jQuery sont réduites, afin que vous pouvez facilement voir les appels à votre script de l’application.</span><span class="sxs-lookup"><span data-stu-id="f8ece-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="f8ece-264">Pour afficher la pile complète, y compris les appels aux bibliothèques externes, vous pouvez développer les nœuds nommés « Bibliothèques externes » :</span><span class="sxs-lookup"><span data-stu-id="f8ece-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="f8ece-265">Si vous cliquez sur un élément dans la pile des appels, Visual Studio ouvre le fichier de code et met en surbrillance le script correspondant.</span><span class="sxs-lookup"><span data-stu-id="f8ece-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="f8ece-266">Voir aussi</span><span class="sxs-lookup"><span data-stu-id="f8ece-266">See Also</span></span>

<span data-ttu-id="f8ece-267">[Présentation d’ASP.NET MVC 4 avec Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (site Web ASP.net)</span><span class="sxs-lookup"><span data-stu-id="f8ece-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="f8ece-268">[Présentation de l’inspecteur de Page](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vidéo Channel 9)</span><span class="sxs-lookup"><span data-stu-id="f8ece-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="f8ece-269">[Messages d’erreur de l’inspecteur de page](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="f8ece-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
