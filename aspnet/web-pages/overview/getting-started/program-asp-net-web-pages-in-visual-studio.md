---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: "Programmation ASP.NET Web Pages (Razor) à l’aide de Visual Studio | Documents Microsoft"
author: tfitzmac
description: "Cette annexe décrit comment vous pouvez utiliser Visual Studio 2010 ou Visual Web Developer 2010 Express pour le programme ASP.NET Web Pages avec la syntaxe Razor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5cfeda206eda8fb3fd769d34fb40bae2c3b65093
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="282d7-103">Programmation de Pages Web ASP.NET (Razor) à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="282d7-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="282d7-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="282d7-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="282d7-105">Cet article explique comment vous pouvez utiliser Visual Studio ou Visual Web Developer Express au programme des sites Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="282d7-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="282d7-106">Vous allez apprendre</span><span class="sxs-lookup"><span data-stu-id="282d7-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="282d7-107">Vous devez installer (le cas échéant) pour travailler avec des Pages Web ASP.NET dans votre version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282d7-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="282d7-108">Comment ajouter la prise en charge pour les Pages Web ASP.NET pour Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="282d7-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="282d7-109">L’utilisation des fonctionnalités dans Visual Studio pour travailler avec des pages ASP.NET Razor, notamment IntelliSense et le débogueur.</span><span class="sxs-lookup"><span data-stu-id="282d7-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="282d7-110">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="282d7-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="282d7-111">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="282d7-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="282d7-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="282d7-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="282d7-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="282d7-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="282d7-114">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="282d7-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="282d7-115">Vous pouvez programmer des pages Web ASP.NET avec syntaxe Razor à l’aide de WebMatrix ou nombreux autres éditeurs de code.</span><span class="sxs-lookup"><span data-stu-id="282d7-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="282d7-116">Vous pouvez également utiliser Microsoft Visual Studio est un environnement complet de développement intégré (IDE) qui fournit un ensemble puissant d’outils pour la création de nombreux types d’applications (pas seulement des sites Web).</span><span class="sxs-lookup"><span data-stu-id="282d7-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="282d7-117">Pour utiliser des pages ASP.NET Razor, vous pouvez utiliser l’une des éditions complètes de Visual Studio ou gratuit [Visual Studio Express pour Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span><span class="sxs-lookup"><span data-stu-id="282d7-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="282d7-118">Deux fonctionnalités particulièrement utiles que Visual Studio fournit pour la programmation avec des pages web ASP.NET Razor sont :</span><span class="sxs-lookup"><span data-stu-id="282d7-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="282d7-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="282d7-119">*IntelliSense*.</span></span> <span data-ttu-id="282d7-120">La fonctionnalité IntelliSense intégrée à Visual Studio est plus complet que IntelliSense dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="282d7-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="282d7-121">*Débogueur*.</span><span class="sxs-lookup"><span data-stu-id="282d7-121">*Debugger*.</span></span> <span data-ttu-id="282d7-122">Le débogueur vous permet de résoudre les problèmes de votre code en arrêtant un programme pendant qu’il est en cours d’exécution, examen des variables et parcourir le code ligne par ligne.</span><span class="sxs-lookup"><span data-stu-id="282d7-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="282d7-123">À l’aide de Visual Studio avec différentes Versions des Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="282d7-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="282d7-124">Visual Studio 2012 et Visual Studio 2013 incluent la prise en charge pour les Pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="282d7-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="282d7-125">(Les packages qui sont nécessaires pour prendre en charge des Pages Web ASP.NET sont installés lorsque vous installez Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="282d7-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="282d7-126">Visual Studio 2010 n’inclut pas prise en charge par défaut pour ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="282d7-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="282d7-127">Pour utiliser les Pages Web ASP.NET avec Visual Studio 2010, vous devez installer le package d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="282d7-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="282d7-128">Pour obtenir ASP.NET Web Pages 2, vous installez ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="282d7-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="282d7-129">Le tableau suivant récapitule la prise en charge pour les Pages Web ASP.NET dans les différentes versions de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282d7-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="282d7-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="282d7-130">Visual Studio 2010</span></span> | <span data-ttu-id="282d7-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="282d7-131">Visual Studio 2012</span></span> | <span data-ttu-id="282d7-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="282d7-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="282d7-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="282d7-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="282d7-134">Installez ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="282d7-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="282d7-135">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="282d7-135">(Included)</span></span> | <span data-ttu-id="282d7-136">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="282d7-136">(Included)</span></span> |
| <span data-ttu-id="282d7-137">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="282d7-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="282d7-138">Mise à jour d’ASP.NET Web Pages 3 NuGet via</span><span class="sxs-lookup"><span data-stu-id="282d7-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="282d7-139">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="282d7-139">(Included)</span></span> |

<span data-ttu-id="282d7-140">Pour travailler avec Visual Studio 2010, consultez [prise en charge pour les Pages Web ASP.NET dans Visual Studio 2010 installation](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="282d7-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="282d7-141">Lancement de Visual Studio à partir de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="282d7-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="282d7-142">Si vous avez démarré un projet dans WebMatrix et que vous souhaitez basculer vers Visual Studio, WebMatrix fournit un bouton pour facilement ouvrir le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282d7-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="282d7-143">Vous devez disposer de Visual Studio installée sur votre ordinateur pour que ce bouton soit activé.</span><span class="sxs-lookup"><span data-stu-id="282d7-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="282d7-144">L’illustration suivante montre le bouton dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="282d7-144">The following image shows the button in WebMatrix.</span></span>

![Lancez Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="282d7-146">Lorsque vous cliquez sur le bouton, le projet est ouvert dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282d7-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="282d7-147">Vous pouvez basculer dans les deux sens entre WebMatrix et Visual Studio sans problème.</span><span class="sxs-lookup"><span data-stu-id="282d7-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="282d7-148">Vous êtes averti si tous les fichiers ont été modifiés dans l’autre environnement et doivent être rechargés pour obtenir les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="282d7-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="282d7-149">Création du site Web ASP.NET Razor dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="282d7-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="282d7-150">Pour créer un site Web ASP.NET Razor dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="282d7-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="282d7-151">Démarrez Visual Studio ou Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="282d7-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="282d7-152">Dans le **fichier** menu, cliquez sur **nouveau Site Web**.</span><span class="sxs-lookup"><span data-stu-id="282d7-152">In the **File** menu, click **New Web Site**.</span></span>

    ![Créez un site web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="282d7-154">Dans le **nouveau Site Web** boîte de dialogue, sélectionnez la langue à utiliser (Visual c# ou Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="282d7-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="282d7-155">Sélectionnez le **Site Web ASP.NET (Razor)** modèle.</span><span class="sxs-lookup"><span data-stu-id="282d7-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![site de Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="282d7-157">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="282d7-157">Click **OK**.</span></span>

<span data-ttu-id="282d7-158">Votre nouveau projet existe et est rempli avec certaines des pages web par défaut pour vous aider à commencer.</span><span class="sxs-lookup"><span data-stu-id="282d7-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="282d7-159">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="282d7-159">Using IntelliSense</span></span>

<span data-ttu-id="282d7-160">Maintenant que vous avez créé un site, vous pouvez voir le fonctionne d’IntelliSense dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282d7-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="282d7-161">Dans le site Web que vous venez de créer, ouvrir le *Default.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="282d7-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="282d7-162">Après le `<h3>` balises dans la page, tapez `@ServerInfo.` (y compris le point).</span><span class="sxs-lookup"><span data-stu-id="282d7-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="282d7-163">Notez comment IntelliSense affiche les méthodes disponibles pour le `ServerInfo` helper dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="282d7-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="282d7-165">Sélectionnez le `GetHtml` méthode dans la liste, puis appuyez sur ENTRÉE.</span><span class="sxs-lookup"><span data-stu-id="282d7-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="282d7-166">IntelliSense complète automatiquement la méthode.</span><span class="sxs-lookup"><span data-stu-id="282d7-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="282d7-167">(Comme avec n’importe quelle méthode en c#, vous devez ajouter `()` caractères après la méthode.)</span><span class="sxs-lookup"><span data-stu-id="282d7-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
 <span data-ttu-id="282d7-168">Le code complet pour le `GetHtml` méthode ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="282d7-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="282d7-169">Appuyez sur Ctrl + F5 pour exécuter la page.</span><span class="sxs-lookup"><span data-stu-id="282d7-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="282d7-170">Voici à quoi ressemble la page lorsque affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="282d7-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![page par défaut dans le navigateur](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="282d7-172">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="282d7-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="282d7-173">L’utilisation du débogueur</span><span class="sxs-lookup"><span data-stu-id="282d7-173">Using the Debugger</span></span>

1. <span data-ttu-id="282d7-174">En haut de la *Default.cshtml* page, après la ligne qui commence par `Page.Title`, ajoutez la ligne de code suivante :</span><span class="sxs-lookup"><span data-stu-id="282d7-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="282d7-175">Dans la marge grise de l’éditeur à gauche du code, puis cliquez sur en regard de cette nouvelle ligne pour ajouter un *point d’arrêt*.</span><span class="sxs-lookup"><span data-stu-id="282d7-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="282d7-176">Un point d’arrêt est un marqueur qui indique au débogueur d’arrêter l’exécution du programme à ce stade afin de voir ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="282d7-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![point d’arrêt défini](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="282d7-178">Supprimez l’appel à la `ServerInfo.GetHtml` (méthode) et ajoutez un appel à la `@myTime` variable à la place.</span><span class="sxs-lookup"><span data-stu-id="282d7-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="282d7-179">Cet appel affiche la valeur de temps actuelle qui est retournée par la nouvelle ligne de code.</span><span class="sxs-lookup"><span data-stu-id="282d7-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="282d7-180">Appuyez sur F5 pour exécuter la page dans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="282d7-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="282d7-181">La page s’arrête sur le point d’arrêt que vous définissez.</span><span class="sxs-lookup"><span data-stu-id="282d7-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="282d7-182">L’illustration suivante montre la page d’aperçu dans l’éditeur avec le point d’arrêt (en jaune).</span><span class="sxs-lookup"><span data-stu-id="282d7-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![point d’arrêt de débogage](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="282d7-184">Dans la barre d’outils de débogage, cliquez sur le **pas à pas détaillé** bouton (ou appuyez sur F11) pour exécuter la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="282d7-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="282d7-185">Chaque fois que vous cliquez sur ce bouton, vous permet de passer l’exécution à la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="282d7-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Pas à pas détaillé de bouton](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="282d7-187">Examinez la valeur de la `myTime` variable en maintenant le pointeur de la souris dessus ou en examinant les valeurs affichées dans le **variables locales** et **pile des appels** windows.</span><span class="sxs-lookup"><span data-stu-id="282d7-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="282d7-188">Visual Studio affiche la valeur de la variable.</span><span class="sxs-lookup"><span data-stu-id="282d7-188">Visual Studio display the value of the variable.</span></span>

    ![valeur d’afficher le temps](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="282d7-190">Lorsque vous avez terminé d’examiner la variable et le code, appuyez sur F5 pour continuer l’exécution de la page sans vous arrêter à chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="282d7-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="282d7-191">Lorsque vous avez terminé d’exécuter pas à pas tout le code, le navigateur affiche la page.</span><span class="sxs-lookup"><span data-stu-id="282d7-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="282d7-192">Pour en savoir plus sur le débogueur et le débogage de code dans Visual Studio, consultez [procédure pas à pas : débogage des Pages Web dans Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="282d7-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="282d7-193">À l’aide de Razor dans les projets ASP.NET MVC avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="282d7-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="282d7-194">La syntaxe Razor est aussi largement utilisée dans les projets ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="282d7-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="282d7-195">MVC est un moyen puissant, basé sur des modèles pour créer des sites Web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="282d7-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="282d7-196">Si votre site ASP.NET Web Pages devient difficile à gérer, vous souhaiterez sa conversion en une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="282d7-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="282d7-197">Pour obtenir un exemple de création d’une application MVC, consultez [mise en route avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="282d7-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="282d7-198">L’installation de la prise en charge pour les Pages Web ASP.NET dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="282d7-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="282d7-199">Cette section montre comment installer Visual Web Developer Express 2010 et les outils de Pages Web ASP.NET pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="282d7-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="282d7-200">Si vous n’avez pas encore de Web Platform Installer, téléchargez-le à partir de l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="282d7-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [<span data-ttu-id="282d7-201">https://www.Microsoft.com/Web/downloads/Platform.aspx</span><span class="sxs-lookup"><span data-stu-id="282d7-201">https://www.microsoft.com/web/downloads/platform.aspx</span></span>](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="282d7-202">Exécutez le programme d’installation de la plateforme Web.</span><span class="sxs-lookup"><span data-stu-id="282d7-202">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="282d7-203">Cliquez sur le **produits** onglet.</span><span class="sxs-lookup"><span data-stu-id="282d7-203">Click the **Products** tab.</span></span>

    ![Onglet produits de WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="282d7-205">Recherchez **ASP.NET MVC 4** (pour ASP.NET Web Pages 2) puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="282d7-205">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="282d7-206">Ces produits incluent des outils de Visual Studio pour créer des sites Web ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="282d7-206">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Options d’installation WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="282d7-208">Cliquez sur **installer** pour terminer l’installation.</span><span class="sxs-lookup"><span data-stu-id="282d7-208">Click **Install** to complete the installation.</span></span>
