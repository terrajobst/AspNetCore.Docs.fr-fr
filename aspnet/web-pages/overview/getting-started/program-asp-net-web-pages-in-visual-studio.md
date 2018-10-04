---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programmation ASP.NET Web Pages (Razor) à l’aide de Visual Studio | Microsoft Docs
author: tfitzmac
description: Cette annexe explique comment vous pouvez utiliser Visual Studio 2010 ou Visual Web Developer 2010 Express programmation ASP.NET Web Pages avec la syntaxe Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: f3c1a74b23a0d9535256caa660408701062fe21c
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795445"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="58cee-103">Programmation de Pages Web ASP.NET (Razor) à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58cee-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="58cee-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="58cee-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="58cee-105">Cet article explique comment vous pouvez utiliser Visual Studio ou Visual Web Developer Express pour le programme des sites Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="58cee-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="58cee-106">Ce que vous allez apprendre</span><span class="sxs-lookup"><span data-stu-id="58cee-106">What you'll learn</span></span>
>
> - <span data-ttu-id="58cee-107">Vous devez installer (voire rien) pour travailler avec des Pages Web ASP.NET dans votre version de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58cee-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="58cee-108">Comment ajouter la prise en charge pour les Pages Web ASP.NET pour Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="58cee-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="58cee-109">Comment utiliser les fonctionnalités dans Visual Studio pour travailler avec les pages ASP.NET Razor, y compris IntelliSense et le débogueur.</span><span class="sxs-lookup"><span data-stu-id="58cee-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="58cee-110">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="58cee-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="58cee-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="58cee-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="58cee-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="58cee-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="58cee-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="58cee-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="58cee-114">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 et WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="58cee-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="58cee-115">Vous pouvez programmer ASP.NET Web pages avec syntaxe Razor à l’aide de WebMatrix ou nombreux autres éditeurs de code.</span><span class="sxs-lookup"><span data-stu-id="58cee-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="58cee-116">Vous pouvez également utiliser Microsoft Visual Studio est un environnement complet de développement intégré (IDE) qui fournit un ensemble puissant d’outils pour la création de nombreux types d’applications (pas seulement les sites Web).</span><span class="sxs-lookup"><span data-stu-id="58cee-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="58cee-117">Pour travailler avec les pages ASP.NET Razor, vous pouvez utiliser [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="58cee-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="58cee-118">Deux fonctionnalités particulièrement utiles que Visual Studio fournit pour la programmation avec les pages web ASP.NET Razor sont :</span><span class="sxs-lookup"><span data-stu-id="58cee-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="58cee-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="58cee-119">*IntelliSense*.</span></span> <span data-ttu-id="58cee-120">La fonctionnalité IntelliSense intégrée à Visual Studio est plus complet que IntelliSense dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="58cee-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="58cee-121">*Débogueur*.</span><span class="sxs-lookup"><span data-stu-id="58cee-121">*Debugger*.</span></span> <span data-ttu-id="58cee-122">Le débogueur vous permet de résoudre les problèmes de votre code en arrêtant un programme pendant qu’il est en cours d’exécution, examen des variables et parcourir le code ligne par ligne.</span><span class="sxs-lookup"><span data-stu-id="58cee-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="58cee-123">À l’aide de Visual Studio avec différentes Versions d’ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="58cee-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="58cee-124">Pour développer les applications web ASP.NET dans Visual Studio 2017, installez le **ASP.NET et développement web** charge de travail.</span><span class="sxs-lookup"><span data-stu-id="58cee-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="58cee-125">Visual Studio 2012 et Visual Studio 2013 incluent la prise en charge pour les Pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="58cee-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="58cee-126">(Les packages qui sont nécessaires pour prendre en charge des Pages Web ASP.NET sont installés lorsque vous installez Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="58cee-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="58cee-127">Visual Studio 2010 n’inclut pas prise en charge par défaut pour ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="58cee-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="58cee-128">Pour utiliser les Pages Web ASP.NET avec Visual Studio 2010, vous devez installer le package d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="58cee-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="58cee-129">Pour obtenir des Pages Web ASP.NET 2, vous installez ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="58cee-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="58cee-130">Le tableau suivant récapitule la prise en charge pour les Pages Web ASP.NET dans les différentes versions de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58cee-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="58cee-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="58cee-131">Visual Studio 2010</span></span> | <span data-ttu-id="58cee-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="58cee-132">Visual Studio 2012</span></span> | <span data-ttu-id="58cee-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="58cee-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="58cee-134">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="58cee-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="58cee-135">Installer ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="58cee-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="58cee-136">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="58cee-136">(Included)</span></span> | <span data-ttu-id="58cee-137">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="58cee-137">(Included)</span></span> |
| <span data-ttu-id="58cee-138">**Pages Web ASP.NET 3**</span><span class="sxs-lookup"><span data-stu-id="58cee-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="58cee-139">Mise à jour à ASP.NET Web Pages NuGet via 3</span><span class="sxs-lookup"><span data-stu-id="58cee-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="58cee-140">(Inclus)</span><span class="sxs-lookup"><span data-stu-id="58cee-140">(Included)</span></span> |

<span data-ttu-id="58cee-141">Pour travailler avec Visual Studio 2010, consultez [prise en charge pour les Pages Web ASP.NET dans Visual Studio 2010 installation](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="58cee-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="58cee-142">Lancement de Visual Studio à partir de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="58cee-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="58cee-143">Si vous avez démarré un projet dans WebMatrix et que vous souhaitez basculer vers Visual Studio, WebMatrix fournit un bouton pour ouvrir facilement le projet dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58cee-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="58cee-144">Vous devez disposer de Visual Studio installée sur votre ordinateur pour que ce bouton soit activé.</span><span class="sxs-lookup"><span data-stu-id="58cee-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="58cee-145">L’illustration suivante montre le bouton dans WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="58cee-145">The following image shows the button in WebMatrix.</span></span>

![Lancez Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="58cee-147">Lorsque vous cliquez sur le bouton, le projet est ouvert dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58cee-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="58cee-148">Vous pouvez basculer dans les deux sens entre WebMatrix et Visual Studio sans problème.</span><span class="sxs-lookup"><span data-stu-id="58cee-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="58cee-149">Vous êtes averti si tous les fichiers ont été modifiés dans l’autre environnement et doivent être rechargés pour obtenir les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="58cee-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="58cee-150">Création du site Web ASP.NET Razor dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58cee-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="58cee-151">Pour créer un site Web ASP.NET Razor dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="58cee-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="58cee-152">Ouvrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58cee-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="58cee-153">Dans le **fichier** menu, cliquez sur **nouveau Site Web**.</span><span class="sxs-lookup"><span data-stu-id="58cee-153">In the **File** menu, click **New Web Site**.</span></span>

    ![créer un nouveau site web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="58cee-155">Dans le **nouveau Site Web** boîte de dialogue, sélectionnez la langue à utiliser (Visual c# ou Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="58cee-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="58cee-156">Sélectionnez le **Site de Web ASP.NET (Razor)** modèle.</span><span class="sxs-lookup"><span data-stu-id="58cee-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![site de Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="58cee-158">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="58cee-158">Click **OK**.</span></span>

<span data-ttu-id="58cee-159">Votre nouveau projet existe et est rempli avec certaines des pages web par défaut pour vous aider à commencer.</span><span class="sxs-lookup"><span data-stu-id="58cee-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="58cee-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="58cee-160">Using IntelliSense</span></span>

<span data-ttu-id="58cee-161">Maintenant que vous avez créé un site, vous pouvez voir le fonctionnement d’IntelliSense dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58cee-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="58cee-162">Dans le site Web que vous venez de créer, ouvrir le *Default.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="58cee-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="58cee-163">Après le `<h3>` balises dans la page, tapez `@ServerInfo.` (y compris le point).</span><span class="sxs-lookup"><span data-stu-id="58cee-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="58cee-164">Notez comment IntelliSense affiche les méthodes disponibles pour le `ServerInfo` helper dans une liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="58cee-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="58cee-166">Sélectionnez le `GetHtml` méthode à partir de la liste et appuyez sur ENTRÉE.</span><span class="sxs-lookup"><span data-stu-id="58cee-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="58cee-167">IntelliSense remplit automatiquement la méthode.</span><span class="sxs-lookup"><span data-stu-id="58cee-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="58cee-168">(Comme avec n’importe quelle méthode en c#, vous devez ajouter `()` caractères après la méthode.) Le code complet de le `GetHtml` méthode ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="58cee-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="58cee-169">Appuyez sur Ctrl + F5 pour exécuter la page.</span><span class="sxs-lookup"><span data-stu-id="58cee-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="58cee-170">Voici à quoi ressemble la page lorsque affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="58cee-170">This is what the page looks like when displayed in a browser:</span></span>

    ![page par défaut dans le navigateur](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="58cee-172">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="58cee-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="58cee-173">L’utilisation du débogueur</span><span class="sxs-lookup"><span data-stu-id="58cee-173">Using the Debugger</span></span>

1. <span data-ttu-id="58cee-174">En haut de la *Default.cshtml* page, après la ligne qui commence par `Page.Title`, ajoutez la ligne de code suivante :</span><span class="sxs-lookup"><span data-stu-id="58cee-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="58cee-175">Dans la marge grise de l’éditeur vers la gauche du code, cliquez sur en regard de cette nouvelle ligne afin d’ajouter un *point d’arrêt*.</span><span class="sxs-lookup"><span data-stu-id="58cee-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="58cee-176">Un point d’arrêt est un marqueur qui indique au débogueur pour interrompre l’exécution du programme à ce stade afin que vous puissiez voir ce qui se passe.</span><span class="sxs-lookup"><span data-stu-id="58cee-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![point d’arrêt défini](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="58cee-178">Supprimez l’appel à la `ServerInfo.GetHtml` (méthode) et ajoutez un appel à la `@myTime` variable à la place.</span><span class="sxs-lookup"><span data-stu-id="58cee-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="58cee-179">Cet appel affiche la valeur de temps actuelle qui est retournée par la nouvelle ligne de code.</span><span class="sxs-lookup"><span data-stu-id="58cee-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="58cee-180">Appuyez sur F5 pour exécuter la page dans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="58cee-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="58cee-181">La page s’arrête sur le point d’arrêt que vous définissez.</span><span class="sxs-lookup"><span data-stu-id="58cee-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="58cee-182">L’illustration suivante montre ce que la page se présente comme dans l’éditeur avec le point d’arrêt (en jaune).</span><span class="sxs-lookup"><span data-stu-id="58cee-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![point d’arrêt de débogage](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="58cee-184">Dans la barre d’outils de débogage, cliquez sur le **pas à pas détaillé** bouton (ou appuyez sur F11) pour exécuter la ligne de code suivante.</span><span class="sxs-lookup"><span data-stu-id="58cee-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="58cee-185">Chaque fois que vous cliquez sur ce bouton, vous passez l’exécution à la ligne suivante de code.</span><span class="sxs-lookup"><span data-stu-id="58cee-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Pas à pas détaillé de bouton](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="58cee-187">Examinez la valeur de la `myTime` variable en maintenant votre pointeur de la souris dessus ou en examinant les valeurs affichées dans le **variables locales** et **pile des appels** windows.</span><span class="sxs-lookup"><span data-stu-id="58cee-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="58cee-188">Visual Studio affiche la valeur de la variable.</span><span class="sxs-lookup"><span data-stu-id="58cee-188">Visual Studio display the value of the variable.</span></span>

    ![valeur d’afficher l’heure](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="58cee-190">Lorsque vous avez terminé l’examen de la variable et le parcours de code, appuyez sur F5 pour continuer l’exécution de la page sans avoir à arrêter au niveau de chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="58cee-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="58cee-191">Lorsque vous avez terminé de parcourir tout le code, le navigateur affiche la page.</span><span class="sxs-lookup"><span data-stu-id="58cee-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="58cee-192">Pour en savoir plus sur le débogueur et le débogage de code dans Visual Studio, consultez [procédure pas à pas : débogage des Pages Web dans Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="58cee-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="58cee-193">À l’aide de Razor dans les projets ASP.NET MVC avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58cee-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="58cee-194">La syntaxe Razor est également largement utilisée dans les projets ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="58cee-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="58cee-195">MVC est un moyen puissant, basé sur des modèles pour créer des sites Web dynamiques.</span><span class="sxs-lookup"><span data-stu-id="58cee-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="58cee-196">Si votre site ASP.NET Web Pages devient difficile à gérer, vous souhaiterez envisager sa conversion en une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="58cee-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="58cee-197">Pour obtenir un exemple de création d’une application MVC, consultez [mise en route avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="58cee-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="58cee-198">L’installation de la prise en charge des Pages Web ASP.NET dans Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="58cee-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="58cee-199">Cette section montre comment installer Visual Web Developer Express 2010 et les outils de Pages Web ASP.NET pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58cee-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="58cee-200">Si vous ne disposez pas de Web Platform Installer, téléchargez-le à partir de l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="58cee-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="58cee-201">Exécutez le programme Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="58cee-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="58cee-202">Cliquez sur le **produits** onglet.</span><span class="sxs-lookup"><span data-stu-id="58cee-202">Click the **Products** tab.</span></span>

    ![Onglet produits de WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="58cee-204">Recherchez **ASP.NET MVC 4** (pour ASP.NET Web Pages 2) puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="58cee-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="58cee-205">Ces produits incluent Visual Studio tools pour créer des sites Web ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="58cee-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Options d’installation WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="58cee-207">Cliquez sur **installer** pour terminer l’installation.</span><span class="sxs-lookup"><span data-stu-id="58cee-207">Click **Install** to complete the installation.</span></span>
