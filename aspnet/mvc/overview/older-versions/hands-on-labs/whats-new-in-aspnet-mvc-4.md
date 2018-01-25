---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: "Quelles sont les nouveautés dans ASP.NET MVC 4 | Documents Microsoft"
author: rick-anderson
description: "ASP.NET MVC 4 est une infrastructure pour générer des applications web évolutive basée sur des normes, à l’aide de modèles de conception bien établis et la puissance d’ASP.NET et..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8b1bdae048afc78399ccc7b0eac7125d9b983c13
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="7ed65-103">Quelles sont les nouveautés dans ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7ed65-103">What's New in ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="7ed65-104">par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7ed65-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7ed65-105">Télécharger Camps Web Kit de formation</span><span class="sxs-lookup"><span data-stu-id="7ed65-105">Download Web Camps Training Kit</span></span>](http://www.microsoft.com/download/29843)

> <span data-ttu-id="7ed65-106">ASP.NET MVC 4 est une infrastructure pour générer des applications web évolutive basée sur des normes, à l’aide de modèles de conception bien établis et la puissance d’ASP.NET et le .NET framework.</span><span class="sxs-lookup"><span data-stu-id="7ed65-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="7ed65-107">Cette nouvelle, quatrième version du framework se concentre sur la simplification du développement d’applications web mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>
> 
> <span data-ttu-id="7ed65-108">Pour commencer, lorsque vous créez un nouveau projet ASP.NET MVC 4 est maintenant un modèle de projet d’application mobile que vous pouvez utiliser pour générer une application autonome en particulier pour les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="7ed65-109">En outre, ASP.NET MVC 4 s’intègre avec jQuery Mobile via un package jQuery.Mobile.MVC NuGet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="7ed65-110">jQuery Mobile est une structure de type HTML5 pour développer des applications web qui sont compatibles avec toutes les plateformes d’appareil mobile, y compris Windows Phone, iPhone, Android et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="7ed65-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="7ed65-111">Toutefois, si vous avez besoin de spécialisation, ASP.NET MVC 4 vous permet également de servir des vues différentes pour différents appareils et fournissent des optimisations spécifiques au périphérique.</span><span class="sxs-lookup"><span data-stu-id="7ed65-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>
> 
> <span data-ttu-id="7ed65-112">Dans cet atelier pratique, vous allez démarrer avec ASP.NET MVC 4 &quot;Application Internet&quot; modèle de projet pour créer une application de la galerie de photos.</span><span class="sxs-lookup"><span data-stu-id="7ed65-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="7ed65-113">Vous allez améliorer progressivement de l’application à l’aide de jQuery Mobile et les nouveautés de ASP.NET MVC 4 pour le rendre compatible avec les différents appareils mobiles et les navigateurs web de bureau.</span><span class="sxs-lookup"><span data-stu-id="7ed65-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="7ed65-114">Vous allez également découvrir associez-y de code pour la génération de code et comment ASP.NET MVC 4 rend plus facile d’écrire des méthodes d’action asynchrones en prenant en charge de la tâche&lt;ActionResult&gt; types de retour.</span><span class="sxs-lookup"><span data-stu-id="7ed65-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>
> 
> <span data-ttu-id="7ed65-115">Tous les exemples de code et des extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="7ed65-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7ed65-116">Objectifs</span><span class="sxs-lookup"><span data-stu-id="7ed65-116">Objectives</span></span>

<span data-ttu-id="7ed65-117">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="7ed65-117">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="7ed65-118">Tirer parti des améliorations apportées à la ASP.NET MVC projet modèles, y compris le nouveau modèle de projet d’application mobile</span><span class="sxs-lookup"><span data-stu-id="7ed65-118">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="7ed65-119">Utilisez l’attribut de la fenêtre d’affichage de HTML5 et requêtes de média CSS pour améliorer l’affichage sur les périphériques mobiles</span><span class="sxs-lookup"><span data-stu-id="7ed65-119">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="7ed65-120">Utilisez jQuery Mobile pour les améliorations progressives et pour la création de l’interface utilisateur web tactiles</span><span class="sxs-lookup"><span data-stu-id="7ed65-120">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="7ed65-121">Créer des affichages mobiles spécifiques</span><span class="sxs-lookup"><span data-stu-id="7ed65-121">Create mobile-specific views</span></span>
- <span data-ttu-id="7ed65-122">Utilisez le composant de sélecteur de vue pour basculer entre les vues mobiles et de bureau dans l’application</span><span class="sxs-lookup"><span data-stu-id="7ed65-122">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="7ed65-123">Créer des contrôleurs asynchrones à l’aide de la prise en charge de la tâche</span><span class="sxs-lookup"><span data-stu-id="7ed65-123">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7ed65-124">Prérequis</span><span class="sxs-lookup"><span data-stu-id="7ed65-124">Prerequisites</span></span>

<span data-ttu-id="7ed65-125">Vous devez disposer des éléments suivants pour effectuer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="7ed65-125">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7ed65-126">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lecture [annexe B](#AppendixB) pour obtenir des instructions sur l’installation).</span><span class="sxs-lookup"><span data-stu-id="7ed65-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="7ed65-127">[ASP.NET MVC 4](../../../mvc4.md) (inclus dans l’installation de Microsoft Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="7ed65-127">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="7ed65-128">Émulateur de Windows Phone (inclus dans le [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="7ed65-128">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="7ed65-129">Facultatif : [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) avec **Electric Plum iPhone simulateur** extension (uniquement pour l’exercice 3 est utilisé pour parcourir l’application web avec un simulateur iPhone)</span><span class="sxs-lookup"><span data-stu-id="7ed65-129">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7ed65-130">Installation</span><span class="sxs-lookup"><span data-stu-id="7ed65-130">Setup</span></span>

<span data-ttu-id="7ed65-131">Dans le document de laboratoire, vous serez invité à insérer des blocs de code.</span><span class="sxs-lookup"><span data-stu-id="7ed65-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="7ed65-132">Pour des raisons pratiques, la majeure partie de ce code est fourni en tant que Visual Studio des extraits de Code, que vous pouvez utiliser à partir de Visual Studio pour éviter d’avoir à ajouter manuellement.</span><span class="sxs-lookup"><span data-stu-id="7ed65-132">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="7ed65-133">Pour installer les extraits de code :</span><span class="sxs-lookup"><span data-stu-id="7ed65-133">To install the code snippets:</span></span>

1. <span data-ttu-id="7ed65-134">Ouvrez une fenêtre de l’Explorateur Windows et accédez à l’atelier **Source\Setup** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-134">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="7ed65-135">Double-cliquez sur le **Setup.cmd** fichier dans ce dossier pour installer les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ed65-135">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="7ed65-136">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et vous souhaitez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide de l’annexe a :](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7ed65-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7ed65-137">Exercices</span><span class="sxs-lookup"><span data-stu-id="7ed65-137">Exercises</span></span>

<span data-ttu-id="7ed65-138">Cet atelier pratique inclut les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="7ed65-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="7ed65-139">Nouveaux modèles de projet ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7ed65-139">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="7ed65-140">Création de l’Application Web de la galerie de photos</span><span class="sxs-lookup"><span data-stu-id="7ed65-140">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="7ed65-141">Ajout de la prise en charge pour les appareils mobiles</span><span class="sxs-lookup"><span data-stu-id="7ed65-141">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="7ed65-142">À l’aide de contrôleurs asynchrones</span><span class="sxs-lookup"><span data-stu-id="7ed65-142">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="7ed65-143">Chaque exercice est accompagné par un **fin** dossier qui contient la solution résultante, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="7ed65-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7ed65-144">Si vous avez besoin d’aide fonctionne via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="7ed65-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="7ed65-145">Durée estimée pour effectuer ce laboratoire : **60 minutes**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-145">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="7ed65-146">Exercice 1 : Nouveaux modèles de projet ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7ed65-146">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="7ed65-147">Dans cet exercice, vous allez explorer les améliorations dans les modèles de projet ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7ed65-147">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="7ed65-148">Outre le modèle d’Application Internet, déjà présentes dans MVC 3, cette version inclut désormais un modèle séparé pour les applications mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-148">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="7ed65-149">Tout d’abord, vous allez examiner certaines fonctionnalités pertinentes de chacun des modèles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-149">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="7ed65-150">Ensuite, vous allez travailler sur votre page correctement sur les différentes plates-formes de rendu à l’aide de l’approche appropriée.</span><span class="sxs-lookup"><span data-stu-id="7ed65-150">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="7ed65-151">Tâche 1 : Explorer le modèle d’Application Internet</span><span class="sxs-lookup"><span data-stu-id="7ed65-151">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="7ed65-152">Ouvrez **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-152">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="7ed65-153">Sélectionnez le **fichier | Nouveau | Projet** commande de menu.</span><span class="sxs-lookup"><span data-stu-id="7ed65-153">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="7ed65-154">Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche de l’arborescence, puis choisissez **Application de Web ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-154">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="7ed65-155">Nommez le projet **PhotoGallery**, sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-155">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-156">Vous personnaliserez ultérieurement la solution PhotoGallery ASP.NET MVC 4 que vous créez.</span><span class="sxs-lookup"><span data-stu-id="7ed65-156">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="7ed65-157">![Création d’un projet](whats-new-in-aspnet-mvc-4/_static/image1.png "création d’un projet")</span><span class="sxs-lookup"><span data-stu-id="7ed65-157">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="7ed65-158">*Création d’un projet*</span><span class="sxs-lookup"><span data-stu-id="7ed65-158">*Creating a new project*</span></span>
3. <span data-ttu-id="7ed65-159">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Internet** modèle de projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-159">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="7ed65-160">Assurez-vous que vous avez sélectionné Razor comme moteur de vue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-160">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="7ed65-161">![Création d’une Application ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "création d’une Application ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="7ed65-161">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="7ed65-162">*Création d’une Application ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="7ed65-162">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-163">La syntaxe Razor a été introduite dans ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="7ed65-163">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="7ed65-164">Son objectif est de réduire le nombre de caractères et séquences de touches requis dans un fichier, en activant un rapide et le fluide de flux de travail de codage.</span><span class="sxs-lookup"><span data-stu-id="7ed65-164">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="7ed65-165">Tire parti de Razor existant C# / VB (ou autres) compétences linguistiques et fournit une syntaxe de balisage de modèle qui permet à un flux de travail de construction HTML impressionnant.</span><span class="sxs-lookup"><span data-stu-id="7ed65-165">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="7ed65-166">Appuyez sur **F5** pour exécuter la solution et d’afficher les modèles renouvelés.</span><span class="sxs-lookup"><span data-stu-id="7ed65-166">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="7ed65-167">Vous pouvez récupérer les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="7ed65-167">You can check out the following features:</span></span>

    <span data-ttu-id="7ed65-168">**Modèles moderne**</span><span class="sxs-lookup"><span data-stu-id="7ed65-168">**Modern-style templates**</span></span>

    <span data-ttu-id="7ed65-169">Les modèles ont été renouvelées, en fournissant plusieurs styles aspect moderne.</span><span class="sxs-lookup"><span data-stu-id="7ed65-169">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="7ed65-170">![Les modèles ASP.NET MVC 4 adaptée](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 adaptée des modèles")</span><span class="sxs-lookup"><span data-stu-id="7ed65-170">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="7ed65-171">*Modèles ASP.NET MVC 4 adaptée*</span><span class="sxs-lookup"><span data-stu-id="7ed65-171">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="7ed65-172">![Nouvelle page de Contact](whats-new-in-aspnet-mvc-4/_static/image4.png "page Nouveau Contact")</span><span class="sxs-lookup"><span data-stu-id="7ed65-172">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="7ed65-173">*Nouvelle page de Contact*</span><span class="sxs-lookup"><span data-stu-id="7ed65-173">*New Contact page*</span></span>

    <span data-ttu-id="7ed65-174">**Rendu adaptable**</span><span class="sxs-lookup"><span data-stu-id="7ed65-174">**Adaptive Rendering**</span></span>

    <span data-ttu-id="7ed65-175">Extraire le redimensionnement de la fenêtre de navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre.</span><span class="sxs-lookup"><span data-stu-id="7ed65-175">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="7ed65-176">Ces modèles utilisent la technique de rendu adaptable pour afficher correctement dans les plateformes de bureau et mobiles sans aucune personnalisation.</span><span class="sxs-lookup"><span data-stu-id="7ed65-176">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="7ed65-177">![Modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents](whats-new-in-aspnet-mvc-4/_static/image5.png "le modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents")</span><span class="sxs-lookup"><span data-stu-id="7ed65-177">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="7ed65-178">*Modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents*</span><span class="sxs-lookup"><span data-stu-id="7ed65-178">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="7ed65-179">**Interface utilisateur plus riche avec JavaScript**</span><span class="sxs-lookup"><span data-stu-id="7ed65-179">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="7ed65-180">Une autre amélioration apportée aux modèles de projet par défaut est l’utilisation de JavaScript pour fournir un code JavaScript plus interactif.</span><span class="sxs-lookup"><span data-stu-id="7ed65-180">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="7ed65-181">Les liens de connexion et le Registre utilisés dans le modèle illustrent comment utiliser la Validations jQuery pour valider les champs d’entrée à partir du côté client.</span><span class="sxs-lookup"><span data-stu-id="7ed65-181">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery Validation](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="7ed65-183">*jQuery Validation*</span><span class="sxs-lookup"><span data-stu-id="7ed65-183">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-184">Notez que les deux sections, dans la première section vous connecter peut se connecter à l’aide d’un compte inscrit à partir du site et dans la deuxième section, que vous pouvez altenativelly les journaux à l’aide d’un autre service d’authentification comme google (désactivé par défaut).</span><span class="sxs-lookup"><span data-stu-id="7ed65-184">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="7ed65-185">Fermez le navigateur pour arrêter le débogueur, revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ed65-185">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="7ed65-186">Ouvrez le fichier **AuthConfig.cs** situé sous le **application\_Démarrer** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-186">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="7ed65-187">Supprimez le commentaire de la dernière ligne pour inscrire le client Google pour *OAuth* l’authentification.</span><span class="sxs-lookup"><span data-stu-id="7ed65-187">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="7ed65-188">Notez que vous pouvez facilement activer l’authentification à l’aide de n’importe quel service OpenID ou OAuth tels que Facebook, Twitter, Microsoft, etc.</span><span class="sxs-lookup"><span data-stu-id="7ed65-188">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="7ed65-189">Appuyez sur **F5** pour exécuter la solution et de naviguer vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="7ed65-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="7ed65-190">Sélectionnez **Google** service pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="7ed65-190">Select **Google** service to log in.</span></span>

    ![En sélectionnant le journal dans le service](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="7ed65-192">*En sélectionnant le journal dans le service*</span><span class="sxs-lookup"><span data-stu-id="7ed65-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="7ed65-193">Connectez-vous en utilisant votre compte Google.</span><span class="sxs-lookup"><span data-stu-id="7ed65-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="7ed65-194">Autoriser le site (localhost) récupérer les informations de compte Google.</span><span class="sxs-lookup"><span data-stu-id="7ed65-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="7ed65-195">Enfin, vous devez enregistrer dans le site à associer le compte Google.</span><span class="sxs-lookup"><span data-stu-id="7ed65-195">Finally, you will have to register in the site to associate the Google account.</span></span>

    ![Associer votre compte Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

    <span data-ttu-id="7ed65-197">*Associer votre compte Google*</span><span class="sxs-lookup"><span data-stu-id="7ed65-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="7ed65-198">Fermez le navigateur pour arrêter le débogueur, revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ed65-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="7ed65-199">Maintenant Explorer la solution pour extraire certaines nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="7ed65-200">![Le modèle de projet ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image9.png "le modèle de projet ASP.NET MVC 4 Internet Application")</span><span class="sxs-lookup"><span data-stu-id="7ed65-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="7ed65-201">*Le modèle de projet ASP.NET MVC 4 Internet Application*</span><span class="sxs-lookup"><span data-stu-id="7ed65-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    - <span data-ttu-id="7ed65-202">**HTML 5 balisage**</span><span class="sxs-lookup"><span data-stu-id="7ed65-202">**HTML 5 Markup**</span></span>

        <span data-ttu-id="7ed65-203">Parcourir les vues de modèle pour déterminer le balisage de thème.</span><span class="sxs-lookup"><span data-stu-id="7ed65-203">Browse template views to find out the new theme markup.</span></span>

        <span data-ttu-id="7ed65-204">![Nouveau modèle, à l’aide de Razor et HTML5 balisage About.cshtml. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Nouveau modèle, à l’aide de Razor et HTML5 balisage About.cshtml.")</span><span class="sxs-lookup"><span data-stu-id="7ed65-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

        <span data-ttu-id="7ed65-205">*Nouveau modèle, à l’aide de balisage Razor et HTML5 (About.cshtml).*</span><span class="sxs-lookup"><span data-stu-id="7ed65-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
    - <span data-ttu-id="7ed65-206">**Bibliothèques JavaScript mis à jour**</span><span class="sxs-lookup"><span data-stu-id="7ed65-206">**Updated JavaScript libraries**</span></span>

        <span data-ttu-id="7ed65-207">Le modèle par défaut ASP.NET MVC 4 inclut désormais KnockoutJS, une infrastructure JavaScript MVVM qui vous permet de créer de riches et applications web hautement réactive à l’aide de JavaScript et HTML.</span><span class="sxs-lookup"><span data-stu-id="7ed65-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="7ed65-208">Comme dans MVC3, jQuery et jQuery bibliothèques d’interface utilisateur sont également inclus dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7ed65-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

        > [!NOTE]
        > <span data-ttu-id="7ed65-209">Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="7ed65-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="7ed65-210">En outre, vous pouvez en savoir plus sur jQuery et jQuery UI dans [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="7ed65-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="7ed65-211">Tâche 2 : Explorer le modèle d’Application Mobile</span><span class="sxs-lookup"><span data-stu-id="7ed65-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="7ed65-212">ASP.NET MVC 4 facilite le développement de sites Web pour mobile et les navigateurs de votre tablette.</span><span class="sxs-lookup"><span data-stu-id="7ed65-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="7ed65-213">Ce modèle a la même structure de l’application en tant que le modèle d’Application Internet (Notez que le code du contrôleur est pratiquement identique), mais son style a été modifié pour afficher correctement dans les appareils tactiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="7ed65-214">Sélectionnez le **fichier | Nouveau | Projet** commande de menu.</span><span class="sxs-lookup"><span data-stu-id="7ed65-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="7ed65-215">Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche arborescence, puis choisissez le **Application de Web ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="7ed65-216">Nommez le projet **PhotoGallery.Mobile**, sélectionnez un emplacement (ou conservez la valeur par défaut), sélectionnez &quot;ajouter à la solution&quot; et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="7ed65-217">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Mobile** modèle de projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="7ed65-218">Assurez-vous que vous avez sélectionné Razor comme moteur de vue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="7ed65-219">![Création d’une Application ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "création d’une Application ASP.NET MVC 4 Mobile")</span><span class="sxs-lookup"><span data-stu-id="7ed65-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="7ed65-220">*Création d’une Application ASP.NET MVC 4 Mobile*</span><span class="sxs-lookup"><span data-stu-id="7ed65-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="7ed65-221">Vous êtes en mesure d’Explorer la solution et passent en revue quelques-unes des nouvelles fonctionnalités introduites par le modèle de solution ASP.NET MVC 4 Mobile :</span><span class="sxs-lookup"><span data-stu-id="7ed65-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="7ed65-222">**jQuery Mobile bibliothèque**</span><span class="sxs-lookup"><span data-stu-id="7ed65-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="7ed65-223">Le modèle de projet d’Application Mobile inclut la bibliothèque jQuery Mobile, qui est une bibliothèque open source pour la compatibilité de navigateur mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="7ed65-224">jQuery Mobile applique amélioration progressive pour les navigateurs mobiles qui prennent en charge CSS et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7ed65-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="7ed65-225">Amélioration progressive permet à tous les navigateurs afficher le contenu de base d’une page web, pendant l’activation uniquement les navigateurs les plus puissantes afficher le contenu rich.</span><span class="sxs-lookup"><span data-stu-id="7ed65-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="7ed65-226">Les fichiers JavaScript et CSS, inclus dans la jQuery Mobile style, aident les navigateurs mobiles ajuster le contenu dans l’écran sans apporter toute modification dans le balisage de page.</span><span class="sxs-lookup"><span data-stu-id="7ed65-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="7ed65-228">*bibliothèque de jQuery mobile inclus dans le modèle*</span><span class="sxs-lookup"><span data-stu-id="7ed65-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="7ed65-229">**En fonction de HTML5 balisage**</span><span class="sxs-lookup"><span data-stu-id="7ed65-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="7ed65-231">*Modèle d’application mobile à l’aide de HTML5 balisage (Login.cshtml et index.cshtml)*</span><span class="sxs-lookup"><span data-stu-id="7ed65-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="7ed65-232">Appuyez sur **F5** pour exécuter la solution.</span><span class="sxs-lookup"><span data-stu-id="7ed65-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="7ed65-233">Ouvrez le **Windows Phone 7 Emulator**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="7ed65-234">Dans l’écran d’accueil téléphone, ouvrez Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="7ed65-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="7ed65-235">Extraire l’URL où l’application de bureau a démarré et accédez à cette URL à partir du téléphone (par exemple, `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="7ed65-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="7ed65-236">Maintenant vous êtes en mesure d’accéder à la page de connexion ou extraire le sur la page.</span><span class="sxs-lookup"><span data-stu-id="7ed65-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="7ed65-237">Notez que le style du site Web est basé sur la nouvelle application de Metro pour mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="7ed65-238">Le modèle de projet ASP.NET MVC 4 s’affiche correctement sur des périphériques mobiles, en veillant à ce que tous les éléments de la page sont visibles et activés.</span><span class="sxs-lookup"><span data-stu-id="7ed65-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="7ed65-239">Notez que les liens de l’en-tête sont assez grand pour être clic ou tapées.</span><span class="sxs-lookup"><span data-stu-id="7ed65-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="7ed65-240">![Des pages dans un appareil mobile de modèle de projet](whats-new-in-aspnet-mvc-4/_static/image14.png "des pages dans un appareil mobile de modèle de projet")</span><span class="sxs-lookup"><span data-stu-id="7ed65-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="7ed65-241">*Pages de modèle de projet dans un appareil mobile*</span><span class="sxs-lookup"><span data-stu-id="7ed65-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="7ed65-242">Le modèle utilise également le **balise meta de la fenêtre d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="7ed65-243">Les navigateurs mobiles plus définissent une largeur d’une fenêtre de navigateur virtuel ou &quot;la fenêtre d’affichage&quot;, qui est supérieur à la largeur réelle de l’appareil mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="7ed65-244">Ainsi, les navigateurs mobiles afficher la page web entière à l’intérieur de l’écran virtuel.</span><span class="sxs-lookup"><span data-stu-id="7ed65-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="7ed65-245">Le **balise meta Viewport** permet aux développeurs de web définir la largeur, hauteur et la mise à l’échelle de la zone du navigateur sur les appareils mobiles **.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="7ed65-246">Le modèle ASP.NET MVC 4 pour les Applications mobiles définit la fenêtre d’affichage de la largeur de l’appareil (&quot;largeur = largeur de l’appareil&quot;) dans le modèle de disposition (*Views\Shared\_Layout.cshtml*), de sorte que toutes les pages ont leur fenêtre d’affichage défini à la largeur d’écran de périphérique.</span><span class="sxs-lookup"><span data-stu-id="7ed65-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="7ed65-247">Notez que la balise meta de la fenêtre d’affichage ne change pas la vue du navigateur par défaut.</span><span class="sxs-lookup"><span data-stu-id="7ed65-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="7ed65-248">Ouvrez  **\_Layout.cshtml**, situé dans le **vues | Partagé** dossier et le commentaire de la balise meta de la fenêtre d’affichage.</span><span class="sxs-lookup"><span data-stu-id="7ed65-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="7ed65-249">Exécutez l’application, si ce n’est pas déjà ouvert et découvrez les différences.</span><span class="sxs-lookup"><span data-stu-id="7ed65-249">Run the application, if not already opened, and check out the differences.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    <span data-ttu-id="7ed65-250">![Le site après la balise meta de la fenêtre d’affichage de commentaires](whats-new-in-aspnet-mvc-4/_static/image15.png "le site après la balise meta de la fenêtre d’affichage de commentaires")</span><span class="sxs-lookup"><span data-stu-id="7ed65-250">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

    <span data-ttu-id="7ed65-251">*Le site après la balise meta de la fenêtre d’affichage de commentaires*</span><span class="sxs-lookup"><span data-stu-id="7ed65-251">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="7ed65-252">Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-252">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="7ed65-253">Supprimez les commentaires de la balise meta de la fenêtre d’affichage.</span><span class="sxs-lookup"><span data-stu-id="7ed65-253">Uncomment the viewport meta tag.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="7ed65-254">Tâche 3 - à l’aide de rendu adaptable</span><span class="sxs-lookup"><span data-stu-id="7ed65-254">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="7ed65-255">Dans cette tâche, vous allez apprendre à une autre méthode pour restituer une page Web correctement sur les appareils mobiles et les navigateurs Web en même temps sans aucune personnalisation.</span><span class="sxs-lookup"><span data-stu-id="7ed65-255">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="7ed65-256">Vous avez déjà utilisé la balise meta de la fenêtre d’affichage avec un objectif similaire.</span><span class="sxs-lookup"><span data-stu-id="7ed65-256">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="7ed65-257">Maintenant vous répondra à une autre méthode puissante : *rendu adaptable*.</span><span class="sxs-lookup"><span data-stu-id="7ed65-257">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="7ed65-258">Rendu adaptable est une technique qui utilise **support CSS3** pour personnaliser le style appliqué à une page.</span><span class="sxs-lookup"><span data-stu-id="7ed65-258">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="7ed65-259">Requêtes de média définissent les conditions à l’intérieur d’une feuille de style, les styles CSS sous certaines conditions de regroupement.</span><span class="sxs-lookup"><span data-stu-id="7ed65-259">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="7ed65-260">Uniquement lorsque la condition est true, le style est appliqué aux objets déclarés.</span><span class="sxs-lookup"><span data-stu-id="7ed65-260">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="7ed65-261">La souplesse de la technique de rendu adaptable permet à toute personnalisation pour afficher le site sur différents appareils.</span><span class="sxs-lookup"><span data-stu-id="7ed65-261">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="7ed65-262">Vous pouvez définir des styles autant que vous le souhaitez sur une feuille de style sans écrire de code de la logique pour choisir le style.</span><span class="sxs-lookup"><span data-stu-id="7ed65-262">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="7ed65-263">Par conséquent, il est un moyen très pratique de l’adaptation des styles de la page, car cela réduit la quantité de code dupliqué et une logique pour à des fins de rendu.</span><span class="sxs-lookup"><span data-stu-id="7ed65-263">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="7ed65-264">En revanche, la consommation de bande passante augmenterait, comme la taille de vos fichiers CSS augmente légèrement.</span><span class="sxs-lookup"><span data-stu-id="7ed65-264">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="7ed65-265">À l’aide de la technique de rendu adaptable, votre site sera **affiché correctement, quel que soit le navigateur.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-265">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="7ed65-266">Toutefois, vous devez envisager si le charge de la bande passante supplémentaire est un problème.</span><span class="sxs-lookup"><span data-stu-id="7ed65-266">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="7ed65-267">Le format de base d’une requête de média est : @media \[étendue : tous les | poche | imprimer | projection | écran\] ([propriété : valeur] et... propriété : valeur)</span><span class="sxs-lookup"><span data-stu-id="7ed65-267">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="7ed65-268">Exemples de requêtes de média : &gt;  **@media tous et (largeur maximale : 1000px) et (largeur minimale : 700px) {} :** pour toutes les résolutions entre 700px et 1000px.</span><span class="sxs-lookup"><span data-stu-id="7ed65-268">Examples of media queries: &gt;**@media all and (max-width: 1000px) and (min-width: 700px) {}:** For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="7ed65-269">**@mediaécran et (largeur minimale : 400 px) et (largeur maximale : 700px) {...} :** uniquement pour les écrans.</span><span class="sxs-lookup"><span data-stu-id="7ed65-269">**@media screen and (min-width: 400px) and (max-width: 700px) { ... }:** Only for screens.</span></span> <span data-ttu-id="7ed65-270">La résolution doit être compris entre 400 et 700px.</span><span class="sxs-lookup"><span data-stu-id="7ed65-270">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="7ed65-271">**@mediaordinateur de poche et (largeur minimale : 20em), écran et (largeur minimale : 20em) {...} :** pour les ordinateurs de poche (mobile et périphériques) et les écrans.</span><span class="sxs-lookup"><span data-stu-id="7ed65-271">**@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:** For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="7ed65-272">La largeur minimale doit être supérieure à 20em.</span><span class="sxs-lookup"><span data-stu-id="7ed65-272">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="7ed65-273">Vous trouverez plus d’informations sur la [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="7ed65-273">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="7ed65-274">Vous allez maintenant découvrir le fonctionnement de rendu adaptable, améliorer la lisibilité de l’ASP.NET MVC 4 par défaut le modèle de site Web.</span><span class="sxs-lookup"><span data-stu-id="7ed65-274">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="7ed65-275">Ouvrez le **PhotoGallery.sln** solution que vous avez créé à la tâche 1, puis sélectionnez le **PhotoGallery** projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-275">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="7ed65-276">Appuyez sur **F5** pour exécuter la solution.</span><span class="sxs-lookup"><span data-stu-id="7ed65-276">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="7ed65-277">Redimensionner la largeur du navigateur, définissant les fenêtres à la moitié ou inférieur à un quart de sa taille d’origine.</span><span class="sxs-lookup"><span data-stu-id="7ed65-277">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="7ed65-278">Notez que se passe-t-il avec les éléments dans l’en-tête : certains éléments n’apparaîtront pas dans la zone visible de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="7ed65-278">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="7ed65-279">Ouvrez **Site.css** fichier à partir de l’Explorateur de Solution Visual Studio, situé dans **contenu** dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-279">Open **Site.css** file from the Visual Studio Solution explorer, located in **Content** project folder.</span></span> <span data-ttu-id="7ed65-280">Appuyez sur **CTRL + F** pour ouvrir la recherche intégrée de Visual Studio et écrire  **@media**  pour localiser le **requête de média CSS**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-280">Press **CTRL + F** to open Visual Studio integrated search, and write **@media** to locate the **CSS media query**.</span></span>

    <span data-ttu-id="7ed65-281">La condition de requête de média définie dans ce modèle fonctionne ainsi : lorsque la taille de la fenêtre du navigateur est ci-dessous **850 px**, les règles CSS appliquées sont celles définies à l’intérieur de ce bloc de support.</span><span class="sxs-lookup"><span data-stu-id="7ed65-281">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="7ed65-282">![Localisation de la requête de média](whats-new-in-aspnet-mvc-4/_static/image16.png "recherche de la requête de média")</span><span class="sxs-lookup"><span data-stu-id="7ed65-282">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="7ed65-283">*Localisation de la requête de média*</span><span class="sxs-lookup"><span data-stu-id="7ed65-283">*Locating the media query*</span></span>
4. <span data-ttu-id="7ed65-284">Remplacez la valeur d’attribut de largeur nulle max dans 850 px avec **10px**, afin de désactiver le rendu adaptable, puis appuyez sur **CTRL + S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-284">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="7ed65-285">De retour vers le navigateur, puis appuyez sur **CTRL + F5** pour actualiser la page avec les modifications que vous avez apportées.</span><span class="sxs-lookup"><span data-stu-id="7ed65-285">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="7ed65-286">Remarquez les différences dans les deux pages lors de l’ajustement de la largeur de la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="7ed65-286">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="7ed65-287">![Dans la partie gauche, la page est mise la @media style, dans la droite, le style est omis](whats-new-in-aspnet-mvc-4/_static/image17.png "applique dans la partie gauche, la page le @media style, dans la droite, le style est omis")</span><span class="sxs-lookup"><span data-stu-id="7ed65-287">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="7ed65-288">*Dans la partie gauche, la page applique le @media style, dans la droite, le style est omis*</span><span class="sxs-lookup"><span data-stu-id="7ed65-288">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="7ed65-289">Maintenant, nous allons nous pencher de ce qui se passe sur les appareils mobiles :</span><span class="sxs-lookup"><span data-stu-id="7ed65-289">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="7ed65-290">![Dans la partie gauche, la page est mise la @media style, dans la droite, le style est omis](whats-new-in-aspnet-mvc-4/_static/image18.png "applique dans la partie gauche, la page le @media style, dans la droite, le style est omis")</span><span class="sxs-lookup"><span data-stu-id="7ed65-290">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="7ed65-291">*Dans la partie gauche, la page applique le @media style, dans la droite, le style est omis*</span><span class="sxs-lookup"><span data-stu-id="7ed65-291">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="7ed65-292">Bien que vous pouvez remarquer que les modifications lorsque la page est rendue dans un navigateur Web ne sont pas très importantes, lors de l’utilisation d’un appareil mobile les différences deviennent plus évidentes.</span><span class="sxs-lookup"><span data-stu-id="7ed65-292">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="7ed65-293">Sur le côté gauche de l’image, nous pouvons voir que le style personnalisé améliore la lisibilité.</span><span class="sxs-lookup"><span data-stu-id="7ed65-293">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="7ed65-294">Rendu adaptable peut être utilisé dans de nombreux scénarios de plus, rendant ainsi plus facile d’appliquer des styles à un site Web et la résolution des problèmes courants de style avec une approche intéressante conditionnel.</span><span class="sxs-lookup"><span data-stu-id="7ed65-294">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="7ed65-295">La balise meta de la fenêtre d’affichage et les requêtes de média CSS ne sont pas spécifiques à ASP.NET MVC 4, donc vous pouvez tirer parti de ces fonctionnalités dans les applications web.</span><span class="sxs-lookup"><span data-stu-id="7ed65-295">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="7ed65-296">Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-296">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="7ed65-297">Exercice 2 : Création de l’Application Web de la galerie de photos</span><span class="sxs-lookup"><span data-stu-id="7ed65-297">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="7ed65-298">Dans cet exercice, vous allez travailler sur une application de la galerie de photos pour afficher des photos.</span><span class="sxs-lookup"><span data-stu-id="7ed65-298">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="7ed65-299">Vous allez démarrer avec le modèle de projet ASP.NET MVC 4, et vous allez ensuite ajouter une fonctionnalité permettant d’extraire des photos à partir d’un service et les afficher dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="7ed65-299">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="7ed65-300">Dans l’exercice suivant, vous mettrez à jour cette solution pour améliorer la manière dont elle est affichée sur les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-300">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="7ed65-301">Tâche 1 : création d’un Service fictifs Photo</span><span class="sxs-lookup"><span data-stu-id="7ed65-301">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="7ed65-302">Dans cette tâche, vous allez créer un fictifs du service pour récupérer le contenu qui sera affiché dans la galerie de photos.</span><span class="sxs-lookup"><span data-stu-id="7ed65-302">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="7ed65-303">Pour ce faire, vous allez ajouter un nouveau contrôleur qui retourne simplement un fichier JSON avec les données de chaque photo.</span><span class="sxs-lookup"><span data-stu-id="7ed65-303">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="7ed65-304">Ouvrez **Visual Studio** si pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="7ed65-304">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="7ed65-305">Sélectionnez le **fichier | Nouveau | Projet** commande de menu.</span><span class="sxs-lookup"><span data-stu-id="7ed65-305">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="7ed65-306">Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche de l’arborescence, puis choisissez **Application de Web ASP.NET MVC 4.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-306">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="7ed65-307">Nommez le projet **PhotoGallery**, sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-307">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="7ed65-308">Ou bien, vous pouvez continuer à travailler à partir de votre existant ASP.NET MVC 4 **Application Internet** solution à partir de **exercice 1** et ignorer l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="7ed65-308">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="7ed65-309">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Internet** modèle de projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-309">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="7ed65-310">Assurez-vous que vous avez sélectionné en tant que le moteur d’affichage de Razor.</span><span class="sxs-lookup"><span data-stu-id="7ed65-310">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="7ed65-311">Dans le **l’Explorateur de solutions**, avec le bouton droit le **application\_données** dossier de votre projet, puis sélectionnez **ajouter | Un élément existant**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-311">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="7ed65-312">Accédez à la **Source\Assets\App\_données** dossier de ce laboratoire et ajouter la **Photos.json** fichier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-312">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="7ed65-313">Créer un nouveau contrôleur portant le nom **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-313">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="7ed65-314">Pour ce faire, cliquez sur le **contrôleurs** dossier, accédez à **ajouter** et sélectionnez **contrôleur.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-314">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="7ed65-315">Complétez le nom de contrôleur, laissez le **contrôleur MVC vide** modèle et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-315">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="7ed65-316">![Ajout de la PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "ajoutant la PhotoController")</span><span class="sxs-lookup"><span data-stu-id="7ed65-316">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="7ed65-317">*Ajout de la PhotoController*</span><span class="sxs-lookup"><span data-stu-id="7ed65-317">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="7ed65-318">Remplacez le **Index** méthode par le code suivant **galerie** action et retournent le contenu à partir du fichier JSON que vous avez récemment ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-318">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="7ed65-319">(Code d’extrait de code - *Action ASP.NET MVC 4 de la galerie de Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-319">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="7ed65-320">Appuyez sur **F5** pour exécuter la solution, puis accédez à l’URL suivante pour tester le service photo factices : `http://localhost:[port]/photo/gallery` (la valeur [port] dépend le port actif sur lequel l’application a été lancée).</span><span class="sxs-lookup"><span data-stu-id="7ed65-320">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="7ed65-321">La demande à cette URL doit récupérer le contenu de la **Photos.json** fichier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-321">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="7ed65-322">![Test du service photo factices](whats-new-in-aspnet-mvc-4/_static/image20.png "test du service factices photo")</span><span class="sxs-lookup"><span data-stu-id="7ed65-322">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="7ed65-323">*Test du service factices photo*</span><span class="sxs-lookup"><span data-stu-id="7ed65-323">*Testing the mocked photo service*</span></span>

<span data-ttu-id="7ed65-324">Dans une implémentation réelle, vous pouvez utiliser [API Web ASP.NET](../../../../web-api/index.md) pour implémenter le service de la galerie de photos.</span><span class="sxs-lookup"><span data-stu-id="7ed65-324">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="7ed65-325">API Web ASP.NET est une infrastructure qui permet de facilement créer des services HTTP qui atteignent un large éventail de clients, y compris les navigateurs et périphériques mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-325">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="7ed65-326">L'API Web ASP.NET est une plate-forme idéale pour générer des applications RESTful sur le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7ed65-326">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="7ed65-327">Tâche 2 - affichage de la galerie de photos</span><span class="sxs-lookup"><span data-stu-id="7ed65-327">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="7ed65-328">Dans cette tâche, vous mettrez à jour la page d’accueil pour afficher la galerie de photos à l’aide du service générée, que vous avez créé dans la première tâche de cet exercice.</span><span class="sxs-lookup"><span data-stu-id="7ed65-328">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="7ed65-329">Vous ajoutez des fichiers de modèle et mettre à jour les vues de la galerie.</span><span class="sxs-lookup"><span data-stu-id="7ed65-329">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="7ed65-330">Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-330">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="7ed65-331">Créer le **Photo** classe dans le **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-331">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="7ed65-332">Pour ce faire, cliquez sur le **modèles** dossier, sélectionnez **ajouter** et cliquez sur **classe**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-332">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="7ed65-333">Ensuite, définissez le nom **Photo.cs** et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-333">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="7ed65-334">Ajouter les membres suivants à la **Photo** classe.</span><span class="sxs-lookup"><span data-stu-id="7ed65-334">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="7ed65-335">(Code d’extrait de code - *modèle d’ASP.NET MVC 4 Lab - Ex02 - Photo*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-335">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="7ed65-336">Ouvrez le fichier **HomeController.cs** dans le dossier **Controllers**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-336">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="7ed65-337">Ajoutez les instructions using suivantes.</span><span class="sxs-lookup"><span data-stu-id="7ed65-337">Add the following using statements.</span></span>

    <span data-ttu-id="7ed65-338">(Code d’extrait de code - *ASP.NET MVC 4 les instructions Using HomeController Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="7ed65-339">Mise à jour la **Index** action utilise **HttpClient** pour récupérer les données de la galerie, puis utiliser le **JavaScriptSerializer** à désérialiser par le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="7ed65-339">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="7ed65-340">(Code d’extrait de code - *Action ASP.NET MVC 4 sur l’Index Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-340">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="7ed65-341">Ouvrez le **Index.cshtml** fichier situé dans le **Views\Home** dossier et remplacer tout le contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="7ed65-341">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="7ed65-342">Ce code effectue une itération sur toutes les photos extraites du service et les affiche dans une liste non triée.</span><span class="sxs-lookup"><span data-stu-id="7ed65-342">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="7ed65-343">(Code d’extrait de code - *liste de photos ASP.NET MVC 4 Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-343">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="7ed65-344">Dans le **l’Explorateur de solutions**, avec le bouton droit le **contenu** dossier de votre projet, puis sélectionnez **ajouter | Un élément existant**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-344">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="7ed65-345">Accédez à la **Source\Assets\Content** dossier de ce laboratoire et ajouter la **Site.css** fichier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-345">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="7ed65-346">Vous devrez confirmer son remplacement.</span><span class="sxs-lookup"><span data-stu-id="7ed65-346">You will have to confirm its replacement.</span></span> <span data-ttu-id="7ed65-347">Si vous avez le **Site.css** le fichier est ouvert, vous devez confirmer pour recharger le fichier également.</span><span class="sxs-lookup"><span data-stu-id="7ed65-347">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="7ed65-348">Ouvrez l’Explorateur de fichiers et de copier l’intégralité du **Photos** dossier situé sous le **Source\Assets** dossier de ce laboratoire dans le dossier racine de votre projet dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="7ed65-348">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="7ed65-349">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-349">Run the application.</span></span> <span data-ttu-id="7ed65-350">Vous devez maintenant voir la page d’accueil affiche les photos stockées dans la galerie.</span><span class="sxs-lookup"><span data-stu-id="7ed65-350">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="7ed65-351">![La galerie de photos](whats-new-in-aspnet-mvc-4/_static/image21.png "la galerie de photos")</span><span class="sxs-lookup"><span data-stu-id="7ed65-351">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="7ed65-352">*La galerie de photos*</span><span class="sxs-lookup"><span data-stu-id="7ed65-352">*Photo Gallery*</span></span>
11. <span data-ttu-id="7ed65-353">Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-353">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="7ed65-354">Exercice 3 : Ajout de la prise en charge pour les appareils mobiles</span><span class="sxs-lookup"><span data-stu-id="7ed65-354">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="7ed65-355">Une clé mises à jour dans ASP.NET MVC 4 est la prise en charge pour le développement mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-355">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="7ed65-356">Dans cet exercice, vous allez explorer les nouvelles fonctionnalités de ASP.NET MVC 4 pour les applications mobiles en étendant la solution de galerie que vous avez créé dans l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="7ed65-356">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="7ed65-357">Tâche 1 : installation jQuery Mobile dans une Application ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7ed65-357">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="7ed65-358">Ouvrez le **commencer** solution situé dans **début/MobileSupport-Ex3/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-358">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="7ed65-359">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="7ed65-359">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="7ed65-360">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="7ed65-360">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7ed65-361">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-361">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="7ed65-362">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="7ed65-362">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="7ed65-363">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-363">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-364">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-364">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7ed65-365">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-365">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7ed65-366">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="7ed65-366">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7ed65-367">Ouvrez le **Package Manager Console** en cliquant sur le **outils** &gt; **Gestionnaire de Package de bibliothèque** &gt; **Gestionnaire de Package Console** option de menu.</span><span class="sxs-lookup"><span data-stu-id="7ed65-367">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="7ed65-368">![Ouverture de la Console du Gestionnaire de Package NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "ouverture de la Console du Gestionnaire de Package NuGet")</span><span class="sxs-lookup"><span data-stu-id="7ed65-368">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="7ed65-369">*Ouverture de la Console Gestionnaire de Package NuGet*</span><span class="sxs-lookup"><span data-stu-id="7ed65-369">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="7ed65-370">Dans la Console du Gestionnaire de Package, exécutez la commande suivante pour installer le **jQuery.Mobile.MVC** package.</span><span class="sxs-lookup"><span data-stu-id="7ed65-370">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="7ed65-371">jQuery Mobile est une bibliothèque open source pour la création de l’interface utilisateur web tactiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-371">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="7ed65-372">Le package jQuery.Mobile.MVC NuGet inclut des programmes d’assistance pour utiliser jQuery Mobile avec une application ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7ed65-372">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-373">En exécutant la commande suivante, vous serez télécharger la bibliothèque jQuery.Mobile.MVC de Nuget.</span><span class="sxs-lookup"><span data-stu-id="7ed65-373">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="7ed65-374">PM</span><span class="sxs-lookup"><span data-stu-id="7ed65-374">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="7ed65-375">Cette commande installe jQuery Mobile et certains fichiers d’aide, notamment les suivantes :</span><span class="sxs-lookup"><span data-stu-id="7ed65-375">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="7ed65-376">**Views/Shared/\_Layout.Mobile.cshtml**: est une disposition jQuery Mobile optimisée pour un écran de petite taille.</span><span class="sxs-lookup"><span data-stu-id="7ed65-376">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="7ed65-377">Lorsque le site Web reçoit une demande à partir d’un navigateur mobile, il remplace la disposition d’origine (\_Layout.cshtml) avec celui-ci.</span><span class="sxs-lookup"><span data-stu-id="7ed65-377">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="7ed65-378">Un composant de sélecteur de vue : se compose de la **Views/Shared/\_ViewSwitcher.cshtml** vue partielle et le **ViewSwitcherController.cs** contrôleur.</span><span class="sxs-lookup"><span data-stu-id="7ed65-378">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="7ed65-379">Ce composant affiche un lien sur les navigateurs mobiles permettent aux utilisateurs de passer à la version bureau de la page.</span><span class="sxs-lookup"><span data-stu-id="7ed65-379">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="7ed65-380">![Projet de la galerie de photos avec prise en charge mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "projet de la galerie de photos avec prise en charge mobile")</span><span class="sxs-lookup"><span data-stu-id="7ed65-380">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="7ed65-381">*Projet de la galerie de photos avec prise en charge mobile*</span><span class="sxs-lookup"><span data-stu-id="7ed65-381">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="7ed65-382">Inscrire les regroupements Mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-382">Register the Mobile bundles.</span></span> <span data-ttu-id="7ed65-383">Pour ce faire, ouvrez le **Global.asax.cs** et ajoutez la ligne suivante.</span><span class="sxs-lookup"><span data-stu-id="7ed65-383">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="7ed65-384">(Code d’extrait de code - *ASP.NET MVC 4 Mobile offres groupées de Registre Lab - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-384">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="7ed65-385">Exécutez l’application à l’aide d’un navigateur web au bureau.</span><span class="sxs-lookup"><span data-stu-id="7ed65-385">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="7ed65-386">Ouvrez le **émulateur de Windows Phone 7,** situé dans **Menu Démarrer | Tous les programmes | Windows Phone SDK 7.1 | Émulateur de Windows Phone.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-386">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="7ed65-387">Dans l’écran d’accueil téléphone, ouvrez Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="7ed65-387">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="7ed65-388">Extraire l’URL où l’application est démarrée et accédez à cette URL avec le navigateur de téléphone (par exemple, `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="7ed65-388">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="7ed65-389">Vous remarquerez que votre application aura un aspect différente dans l’émulateur Windows Phone, comme le jQuery.Mobile.MVC a créé les nouvelles ressources dans votre projet qui montrent les vues optimisées pour les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-389">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="7ed65-390">Notez le message en haut du téléphone, en affichant le lien qui bascule vers l’affichage du bureau.</span><span class="sxs-lookup"><span data-stu-id="7ed65-390">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="7ed65-391">En outre, le  **\_Layout.Mobile.cshtml** mise en page qui a été créé par le package que vous avez installé, y compris une autre disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-391">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-392">Jusqu'à présent, il n’existe aucun lien pour revenir à l’affichage mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-392">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="7ed65-393">Il sera être inclus dans les versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="7ed65-393">It will be included in later versions.</span></span>

    <span data-ttu-id="7ed65-394">![Affichage mobile de la page d’accueil de la galerie de photos](whats-new-in-aspnet-mvc-4/_static/image24.png "affichage Mobile de la page d’accueil de la galerie de photos")</span><span class="sxs-lookup"><span data-stu-id="7ed65-394">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="7ed65-395">*Affichage mobile de la page d’accueil de la galerie de photos*</span><span class="sxs-lookup"><span data-stu-id="7ed65-395">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="7ed65-396">Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-396">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="7ed65-397">Tâche 2 : création de vues mobiles</span><span class="sxs-lookup"><span data-stu-id="7ed65-397">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="7ed65-398">Dans cette tâche, vous allez créer une version mobile de l’affichage de l’index avec un contenu adapté pour une meilleure appareance dans les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-398">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="7ed65-399">Copie le **Views\Home\Index.cshtml** afficher et les coller pour créer une copie, renommez le nouveau fichier à **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-399">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="7ed65-400">Ouvrir le nouveau créée **Index.Mobile.cshtml** permet d’afficher et de remplacer la &lt;ul&gt; balise avec ce code.</span><span class="sxs-lookup"><span data-stu-id="7ed65-400">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="7ed65-401">Ce faisant, vous mettront à jour la &lt;ul&gt; balise jQuery Mobile et annotations de données à utiliser des thèmes de jQuery mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-401">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="7ed65-402">Notez que :</span><span class="sxs-lookup"><span data-stu-id="7ed65-402">Notice that:</span></span>
    > 
    > - <span data-ttu-id="7ed65-403">Le **rôle de données** attribut la valeur **listview** affichera la liste en utilisant les styles de listview.</span><span class="sxs-lookup"><span data-stu-id="7ed65-403">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="7ed65-404">Le **incrustation de données** attribut défini sur true affiche la liste avec une bordure arrondie et marge.</span><span class="sxs-lookup"><span data-stu-id="7ed65-404">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="7ed65-405">Le **filtre de données** attribut la valeur **true** génère une zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="7ed65-405">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="7ed65-406">Plus d’informations sur les conventions de jQuery Mobile dans la documentation du projet : [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="7ed65-406">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="7ed65-407">Appuyez sur **CTRL + S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-407">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="7ed65-408">Basculez vers le **Windows Phone Emulator** et actualisez le site.</span><span class="sxs-lookup"><span data-stu-id="7ed65-408">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="7ed65-409">Notez que la nouvelle apparence de la liste de bibliothèque, ainsi que la nouvelle zone de recherche située en haut.</span><span class="sxs-lookup"><span data-stu-id="7ed65-409">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="7ed65-410">Ensuite, tapez un mot dans la zone de recherche (par exemple, **Tulips**) pour lancer une recherche dans la galerie de photos.</span><span class="sxs-lookup"><span data-stu-id="7ed65-410">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="7ed65-411">![La galerie à l’aide du style de listview avec filtrage](whats-new-in-aspnet-mvc-4/_static/image25.png "à l’aide du style de listview avec le filtrage de la galerie")</span><span class="sxs-lookup"><span data-stu-id="7ed65-411">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="7ed65-412">*À l’aide du style de listview avec le filtrage de la galerie*</span><span class="sxs-lookup"><span data-stu-id="7ed65-412">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="7ed65-413">Pour résumer, vous avez utilisé la recette encourage à se vue pour créer une copie de l’affichage de l’Index avec la &quot;mobile&quot; suffixe.</span><span class="sxs-lookup"><span data-stu-id="7ed65-413">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="7ed65-414">Ce suffixe indique à ASP.NET MVC 4 que chaque demande généré à partir d’un appareil mobile utilise cette copie de l’index.</span><span class="sxs-lookup"><span data-stu-id="7ed65-414">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="7ed65-415">En outre, vous avez mis à jour la version mobile de la vue Index jQuery Mobile pour améliorer l’apparence site dans les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-415">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="7ed65-416">Revenez à Visual Studio et ouvrez **Site.Mobile.css** situé sous le **contenu** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-416">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="7ed65-417">Corrigez le positionnement du titre de la photo pour qu’il s’affiche à droite de l’image.</span><span class="sxs-lookup"><span data-stu-id="7ed65-417">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="7ed65-418">Pour ce faire, ajoutez le code suivant à la **Site.Mobile.css** fichier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-418">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="7ed65-419">CSS</span><span class="sxs-lookup"><span data-stu-id="7ed65-419">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="7ed65-420">Appuyez sur **CTRL + S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-420">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="7ed65-421">Revenez à la **Windows Phone Emulator** et actualisez le site.</span><span class="sxs-lookup"><span data-stu-id="7ed65-421">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="7ed65-422">Notez que le titre de la photo est correctement positionné maintenant.</span><span class="sxs-lookup"><span data-stu-id="7ed65-422">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="7ed65-423">![Titre positionné sur le côté droit de l’image](whats-new-in-aspnet-mvc-4/_static/image26.png "titre positionné sur le côté droit de l’image")</span><span class="sxs-lookup"><span data-stu-id="7ed65-423">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="7ed65-424">*Titre positionné sur le côté droit de l’image*</span><span class="sxs-lookup"><span data-stu-id="7ed65-424">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="7ed65-425">Tâche 3 - jQuery Mobile thèmes</span><span class="sxs-lookup"><span data-stu-id="7ed65-425">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="7ed65-426">Chaque mise en page et le widget de jQuery Mobile sont conçu autour d’une infrastructure CSS et orienté objet nouvelle qui rend possible d’appliquer un thème de présentation complète de visual unifiée pour les sites et applications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-426">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="7ed65-427">Thème par défaut de jQuery Mobile comprend 5 échantillons sont fournis à des lettres (a, b, c, d, e) pour référence rapide.</span><span class="sxs-lookup"><span data-stu-id="7ed65-427">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="7ed65-428">Dans cette tâche, vous mettrez à jour la mise en page mobile pour utiliser un thème autre que la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="7ed65-428">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="7ed65-429">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ed65-429">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="7ed65-430">Ouvrez le  **\_Layout.Mobile.cshtml** fichier situé dans **Views\Shared**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-430">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="7ed65-431">Rechercher l’élément div avec le rôle de données la valeur &quot;page&quot; et mettre à jour le **données-theme** à &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="7ed65-431">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="7ed65-432">Appuyez sur **CTRL + S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-432">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="7ed65-433">Actualiser le site dans le **Windows Phone Emulator** et notez le nouveau jeu de couleurs.</span><span class="sxs-lookup"><span data-stu-id="7ed65-433">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="7ed65-434">![Mise en page mobile avec un jeu de couleurs](whats-new-in-aspnet-mvc-4/_static/image27.png "disposition Mobile avec un jeu de couleurs")</span><span class="sxs-lookup"><span data-stu-id="7ed65-434">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="7ed65-435">*Mise en page mobile avec un jeu de couleurs*</span><span class="sxs-lookup"><span data-stu-id="7ed65-435">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="7ed65-436">Tâche 4 : à l’aide du composant de sélecteur de vue et le navigateur de substitution de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="7ed65-436">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="7ed65-437">Une convention pour les pages web mobile optimisé est pour ajouter un lien dont le texte est un élément, telles que l’affichage du bureau ou en mode de site complète qui permet aux utilisateurs de basculer vers une version de bureau de la page.</span><span class="sxs-lookup"><span data-stu-id="7ed65-437">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="7ed65-438">Le package jQuery.Mobile.MVC comprend un exemple **sélecteur de vue** composant à cet effet utilisé dans le  **\_Layout.Mobile.cshtml** vue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-438">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="7ed65-439">![Lien pour basculer en mode de bureau](whats-new-in-aspnet-mvc-4/_static/image28.png "lien pour basculer vers l’affichage du bureau")</span><span class="sxs-lookup"><span data-stu-id="7ed65-439">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="7ed65-440">*Ce lien pour basculer vers l’affichage du bureau*</span><span class="sxs-lookup"><span data-stu-id="7ed65-440">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="7ed65-441">Le sélecteur de vue utilise une nouvelle fonctionnalité appelée **substitution de navigateur**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-441">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="7ed65-442">Cette fonctionnalité permet à votre application de traiter les demandes comme si elles provenaient d’un autre navigateur (agent utilisateur) que celui qu’ils proviennent réellement.</span><span class="sxs-lookup"><span data-stu-id="7ed65-442">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="7ed65-443">Dans cette tâche, vous allez explorer l’exemple d’implémentation d’un sélecteur de vue ajouté par jQuery.Mobile.MVC et le nouveau navigateur substitution de fonctionnalités dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7ed65-443">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="7ed65-444">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ed65-444">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="7ed65-445">Ouvrez le  **\_Layout.Mobile.cshtml** affichage situé sous le **Views\Shared** dossier et notez le composant de sélecteur de vue référencé par une vue partielle.</span><span class="sxs-lookup"><span data-stu-id="7ed65-445">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="7ed65-446">![Disposition Mobile à l’aide du composant de sélecteur de vue](whats-new-in-aspnet-mvc-4/_static/image29.png "disposition Mobile à l’aide du composant de sélecteur de vue")</span><span class="sxs-lookup"><span data-stu-id="7ed65-446">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="7ed65-447">*Disposition Mobile à l’aide du composant de sélecteur de vue*</span><span class="sxs-lookup"><span data-stu-id="7ed65-447">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="7ed65-448">Ouvrez le  **\_ViewSwitcher.cshtml** vue partielle.</span><span class="sxs-lookup"><span data-stu-id="7ed65-448">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="7ed65-449">La vue partielle utilise la nouvelle méthode **ViewContext.HttpContext.GetOverriddenBrowser()** pour déterminer l’origine de la demande web et afficher le lien correspondant pour passer à des vues de bureau ou Mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-449">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="7ed65-450">Le **GetOverridenBrowser** méthode retourne un **HttpBrowserCapabilitiesBase** instance qui correspond à l’agent utilisateur actuellement défini pour la demande (réel ou substituée).</span><span class="sxs-lookup"><span data-stu-id="7ed65-450">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="7ed65-451">Vous pouvez utiliser cette valeur pour obtenir des propriétés telles que **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-451">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="7ed65-452">![Vue partielle de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "vue partielle de ViewSwitcher")</span><span class="sxs-lookup"><span data-stu-id="7ed65-452">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="7ed65-453">*Vue partielle de ViewSwitcher*</span><span class="sxs-lookup"><span data-stu-id="7ed65-453">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="7ed65-454">Ouvrez le **ViewSwitcherController.cs** classe se trouve dans le **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-454">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="7ed65-455">Extraction de cette action SwitchView est appelée par le lien dans le composant ViewSwitcher et notez les nouvelles méthodes HttpContext.</span><span class="sxs-lookup"><span data-stu-id="7ed65-455">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="7ed65-456">Le **HttpContext.ClearOverridenBrowser()** méthode supprime tout agent utilisateur substitué pour la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="7ed65-456">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="7ed65-457">Le **HttpContext.SetOverridenBrowser()** méthode remplace la valeur de l’agent utilisateur réelle de la demande à l’aide de l’agent utilisateur spécifié.</span><span class="sxs-lookup"><span data-stu-id="7ed65-457">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="7ed65-458">![Contrôleur de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher contrôleur")</span><span class="sxs-lookup"><span data-stu-id="7ed65-458">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="7ed65-459">*ViewSwitcher contrôleur*</span><span class="sxs-lookup"><span data-stu-id="7ed65-459">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="7ed65-460">Substitution de navigateur est une fonctionnalité principale d’ASP.NET MVC 4, qui est également disponible même si vous n’installez pas le package jQuery.Mobile.MVC.</span><span class="sxs-lookup"><span data-stu-id="7ed65-460">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="7ed65-461">Toutefois, cette fonction affecte uniquement vue mise en page et vue partielle, et il n’affecte pas les fonctionnalités qui dépendent de l’objet Request.Browser.</span><span class="sxs-lookup"><span data-stu-id="7ed65-461">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="7ed65-462">Tâche 5 : ajout du sélecteur de vue dans l’affichage du bureau</span><span class="sxs-lookup"><span data-stu-id="7ed65-462">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="7ed65-463">Dans cette tâche, vous mettrez à jour la disposition de bureau pour inclure le sélecteur de vue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-463">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="7ed65-464">Cela permettra aux utilisateurs itinérants de revenir en arrière à la vue mobile lorsque vous parcourez l’affichage du bureau.</span><span class="sxs-lookup"><span data-stu-id="7ed65-464">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="7ed65-465">Actualiser le site dans le **Windows Phone Emulator**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-465">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="7ed65-466">Cliquez sur le **affichage du bureau** lien en haut de la galerie.</span><span class="sxs-lookup"><span data-stu-id="7ed65-466">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="7ed65-467">Ne remarquez aucun sélecteur de vue dans la vue bureau permettant de que revenir à l’affichage mobile.</span><span class="sxs-lookup"><span data-stu-id="7ed65-467">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="7ed65-468">Revenez à Visual Studio et ouvrez le  **\_Layout.cshtml** vue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-468">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="7ed65-469">Recherchez la section de connexion et insérez un appel à restituer le  **\_ViewSwitcher** vue partielle ci-dessous le  **\_LogOnPartial** vue partielle.</span><span class="sxs-lookup"><span data-stu-id="7ed65-469">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="7ed65-470">Ensuite, appuyez sur **CTRL + S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-470">Then, press **CTRL + S** to save the changes.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="7ed65-471">Appuyez sur **CTRL + S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-471">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="7ed65-472">Actualisez la page dans l’émulateur Windows Phone et double-cliquez sur l’écran pour effectuer un zoom.</span><span class="sxs-lookup"><span data-stu-id="7ed65-472">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="7ed65-473">Notez que la page d’accueil affiche maintenant le **affichage Mobile** lien bascule de mobile en mode bureau.</span><span class="sxs-lookup"><span data-stu-id="7ed65-473">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="7ed65-474">![Afficher le sélecteur de rendu en mode Bureau](whats-new-in-aspnet-mvc-4/_static/image32.png "basculeur restitué en mode bureau")</span><span class="sxs-lookup"><span data-stu-id="7ed65-474">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="7ed65-475">*Sélecteur de vue restitué en mode bureau*</span><span class="sxs-lookup"><span data-stu-id="7ed65-475">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="7ed65-476">Basculez à nouveau à la vue Mobile et accédez à **sur** page (http://localhost [port] / Home/sur).</span><span class="sxs-lookup"><span data-stu-id="7ed65-476">Switch to the Mobile view again and browse to **About** page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="7ed65-477">Notez que, même si vous n’avez pas créé une vue About.Mobile.cshtml, la page à propos est affichée à l’aide de la mise en page mobile (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="7ed65-477">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="7ed65-478">![Sur la page](whats-new-in-aspnet-mvc-4/_static/image33.png "sur la page")</span><span class="sxs-lookup"><span data-stu-id="7ed65-478">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="7ed65-479">*Sur la page*</span><span class="sxs-lookup"><span data-stu-id="7ed65-479">*About page*</span></span>
8. <span data-ttu-id="7ed65-480">Enfin, ouvrez le site dans un navigateur Web.</span><span class="sxs-lookup"><span data-stu-id="7ed65-480">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="7ed65-481">Notez qu’aucune des mises à jour précédentes a affecté l’affichage du bureau.</span><span class="sxs-lookup"><span data-stu-id="7ed65-481">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="7ed65-482">![Affichage du bureau PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery affichage du bureau")</span><span class="sxs-lookup"><span data-stu-id="7ed65-482">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="7ed65-483">*Affichage du bureau PhotoGallery*</span><span class="sxs-lookup"><span data-stu-id="7ed65-483">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="7ed65-484">Tâche 6 : création de nouveaux Modes d’affichage</span><span class="sxs-lookup"><span data-stu-id="7ed65-484">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="7ed65-485">La nouvelle fonctionnalité de modes d’affichage permet à une application de sélectionner les vues selon le navigateur qui génère la demande.</span><span class="sxs-lookup"><span data-stu-id="7ed65-485">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="7ed65-486">Par exemple, si un navigateur demande la page d’accueil, l’application retournera les **Views\Home\Index.cshtml** modèle.</span><span class="sxs-lookup"><span data-stu-id="7ed65-486">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="7ed65-487">Ensuite, si un navigateur mobile demande à la page d’accueil, l’application retournera les **Views\Home\Index.mobile.cshtml** modèle.</span><span class="sxs-lookup"><span data-stu-id="7ed65-487">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="7ed65-488">Dans cette tâche, vous allez créer une disposition personnalisée pour les appareils iPhone et avoir simuler des requêtes à partir d’appareils iPhone.</span><span class="sxs-lookup"><span data-stu-id="7ed65-488">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="7ed65-489">Pour ce faire, vous pouvez utiliser soit un émulateur/simulateur iPhone (comme [électrique Mobile simulateur](http://www.electricplum.com/)) ou d’un navigateur avec des modules complémentaires qui modifient l’agent utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7ed65-489">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="7ed65-490">Pour obtenir des instructions sur la définition de la chaîne de l’agent utilisateur dans un navigateur Safari émuler un iPhone, consultez [comment permettre Safari prétendez qu’il se IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) dans les blog de David Alison.</span><span class="sxs-lookup"><span data-stu-id="7ed65-490">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="7ed65-491">**Notez que cette tâche est facultative et vous pouvez continuer tout au long de l’atelier sans l’exécuter.**</span><span class="sxs-lookup"><span data-stu-id="7ed65-491">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="7ed65-492">Dans Visual Studio, appuyez sur **MAJ** + **F5** pour arrêter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-492">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="7ed65-493">Ouvrez **Global.asax.cs** et ajoutez le code suivant à l’aide d’instruction.</span><span class="sxs-lookup"><span data-stu-id="7ed65-493">Open **Global.asax.cs** and add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="7ed65-494">Ajoutez le code en surbrillance suivant à l’Application\_Start (méthode).</span><span class="sxs-lookup"><span data-stu-id="7ed65-494">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="7ed65-495">(Code d’extrait de code - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-495">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    <span data-ttu-id="7ed65-496">Vous avez enregistré un nouveau **DefaultDisplayMode nommé &quot;iPhone&quot;**, au sein de la méthode statique **DisplayModeProvider.Instance.Modes** liste statique, qui sera comparée à chaque demande entrante.</span><span class="sxs-lookup"><span data-stu-id="7ed65-496">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="7ed65-497">Si la demande entrante contient la chaîne &quot;iPhone&quot;, ASP.NET MVC trouvera les vues dont le nom contient la &quot;iPhone&quot; suffixe.</span><span class="sxs-lookup"><span data-stu-id="7ed65-497">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="7ed65-498">Le paramètre 0 indique la façon dont certains est le nouveau mode ; par exemple, cette vue est plus spécifique que le général &quot;.mobile&quot; règle qui correspond aux demandes à partir d’appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-498">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

    <span data-ttu-id="7ed65-499">Une fois ce code s’exécute lorsqu’un navigateur iPhone génère une demande, votre application utilisera le **Views\Shared\\_Layout.iPhone.cshtml** mise en page que vous créerez dans les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="7ed65-499">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-500">Cette méthode de test de la demande pour iPhone a été simplifié à des fins de démonstration et peut ne pas fonctionne comme prévu pour chaque chaîne d’agent utilisateur iPhone (pour le test de l’exemple respecte la casse).</span><span class="sxs-lookup"><span data-stu-id="7ed65-500">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>
4. <span data-ttu-id="7ed65-501">Créer une copie de la  **\_Layout.Mobile.cshtml** de fichiers dans le **Views\Shared** dossier et renommez la copie à &quot;  **\_Layout.iPhone.csthml** &quot;.</span><span class="sxs-lookup"><span data-stu-id="7ed65-501">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="7ed65-502">Ouvrez  **\_Layout.iPhone.csthml** vous avez créé à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="7ed65-502">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="7ed65-503">Rechercher l’élément div avec l’attribut de rôle de données la valeur **page** et modifiez le **thème de données** attribut &quot; **un**&quot;.</span><span class="sxs-lookup"><span data-stu-id="7ed65-503">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    <span data-ttu-id="7ed65-504">Vous avez maintenant 3 mises en page dans votre application ASP.NET MVC 4 :</span><span class="sxs-lookup"><span data-stu-id="7ed65-504">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

    1. <span data-ttu-id="7ed65-505">**\_Layout.cshtml**: présentation par défaut utilisée pour les navigateurs de bureau.</span><span class="sxs-lookup"><span data-stu-id="7ed65-505">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
    2. <span data-ttu-id="7ed65-506">**\_Layout.Mobile.cshtml**: présentation par défaut utilisée pour les appareils mobiles.</span><span class="sxs-lookup"><span data-stu-id="7ed65-506">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
    3. <span data-ttu-id="7ed65-507">**\_Layout.iPhone.cshtml**: mise en page spécifique pour les appareils iPhone, à l’aide d’un jeu de couleurs pour différencier \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="7ed65-507">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="7ed65-508">Appuyez sur **F5** pour exécuter l’application et de parcourir le site dans le **Windows Phone Emulator**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-508">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="7ed65-509">Ouvrir un **simulateur iPhone** (consultez [annexe C](#AppendixC) pour obtenir des instructions sur la façon d’installer et configurer un simulateur iPhone), puis accédez au site trop.</span><span class="sxs-lookup"><span data-stu-id="7ed65-509">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="7ed65-510">Notez que chaque téléphone utilise le modèle spécifique.</span><span class="sxs-lookup"><span data-stu-id="7ed65-510">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="7ed65-512">*À l’aide des vues différentes pour chaque périphérique mobile*</span><span class="sxs-lookup"><span data-stu-id="7ed65-512">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="7ed65-513">Exercice 4 : À l’aide de contrôleurs asynchrones</span><span class="sxs-lookup"><span data-stu-id="7ed65-513">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="7ed65-514">Microsoft .NET Framework 4.5 introduit de nouvelles fonctionnalités de langage dans c# et Visual Basic pour fournir une nouvelle base pour le comportement asynchrone dans la programmation .NET.</span><span class="sxs-lookup"><span data-stu-id="7ed65-514">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="7ed65-515">Cette nouvelle foundation rend la programmation asynchrone similaire à - et aussi simple que la programmation synchrone.</span><span class="sxs-lookup"><span data-stu-id="7ed65-515">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="7ed65-516">Vous pouvez désormais écrire des méthodes d’action asynchrones dans ASP.NET MVC 4 à l’aide du **AsyncController** classe.</span><span class="sxs-lookup"><span data-stu-id="7ed65-516">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="7ed65-517">Vous pouvez utiliser les méthodes d’action asynchrones pour la durée d’exécution longue, demandes de liaison de pas le processeur.</span><span class="sxs-lookup"><span data-stu-id="7ed65-517">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="7ed65-518">Cela évite de bloquer le serveur Web de réaliser un travail pendant le traitement de la demande.</span><span class="sxs-lookup"><span data-stu-id="7ed65-518">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="7ed65-519">La classe AsyncController est généralement utilisée pour les appels de service Web long terme.</span><span class="sxs-lookup"><span data-stu-id="7ed65-519">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="7ed65-520">Cet exercice explique les principes fondamentaux de l’opération asynchrone dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7ed65-520">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="7ed65-521">Si vous souhaitez un approfondissement, vous pouvez consulter l’article suivant : [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="7ed65-521">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="7ed65-522">Tâche 1 : implémentation d’un contrôleur asynchrone</span><span class="sxs-lookup"><span data-stu-id="7ed65-522">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="7ed65-523">Ouvrez le **commencer** solution situé dans **début/Async-Ex4/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-523">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="7ed65-524">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="7ed65-524">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="7ed65-525">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="7ed65-525">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7ed65-526">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-526">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="7ed65-527">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="7ed65-527">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="7ed65-528">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-528">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-529">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-529">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7ed65-530">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-530">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7ed65-531">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="7ed65-531">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7ed65-532">Ouvrez le **HomeController.cs** classe à partir de la **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-532">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="7ed65-533">Ajoutez le code suivant à l’aide d’instruction.</span><span class="sxs-lookup"><span data-stu-id="7ed65-533">Add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="7ed65-534">Mise à jour la **HomeController** classe hérite de **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-534">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="7ed65-535">Les contrôleurs qui dérivent de AsyncController activent ASP.NET traiter les requêtes asynchrones, et ils peuvent toujours les méthodes d’action synchrones service.</span><span class="sxs-lookup"><span data-stu-id="7ed65-535">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="7ed65-536">Ajouter le **async** mot clé à la **Index** (méthode) et le rendre le type de retour **tâche&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-536">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="7ed65-537">Le **async** (mot clé) est un des nouveaux mots clés du .NET Framework 4.5 fournit ; elle indique au compilateur que cette méthode contient du code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="7ed65-537">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="7ed65-538">A **tâche** objet représente une opération asynchrone qui peut se terminer à un moment donné dans le futur.</span><span class="sxs-lookup"><span data-stu-id="7ed65-538">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="7ed65-539">Remplacez le **client. GetAsync()** appel avec la version complète asynchrone à l’aide await (mot clé), comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7ed65-539">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="7ed65-540">(Code d’extrait de code - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-540">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="7ed65-541">Dans la version précédente, que vous utilisiez le **résultat** propriété à partir de la **tâche** objet bloque le thread jusqu'à ce que le résultat est retourné (version de synchronisation).</span><span class="sxs-lookup"><span data-stu-id="7ed65-541">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="7ed65-542">Ajout de la **await** mot clé indique au compilateur pour attendre la tâche retournée à partir de l’appel de méthode de manière asynchrone.</span><span class="sxs-lookup"><span data-stu-id="7ed65-542">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="7ed65-543">Cela signifie que le reste du code sera exécuté comme un rappel seulement après l’achève de la méthode attendue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-543">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="7ed65-544">Une autre chose à remarquer est que vous n’avez pas besoin de modifier votre bloc try-catch afin de résoudre ce problème : les exceptions qui se produisent en arrière-plan ou au premier plan sont encore interceptées sans aucun travail supplémentaire à l’aide d’un gestionnaire fourni par l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="7ed65-544">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="7ed65-545">Modifier le code pour continuer avec l’implémentation asynchrone, en remplaçant les lignes avec le nouveau code, comme indiqué ci-dessous</span><span class="sxs-lookup"><span data-stu-id="7ed65-545">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="7ed65-546">(Code d’extrait de code - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-546">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="7ed65-547">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-547">Run the application.</span></span> <span data-ttu-id="7ed65-548">Vous ne remarquerez aucune modification majeure, mais votre code ne bloquera pas d’un thread du pool de threads qui effectue une meilleure utilisation des ressources du serveur et améliore les performances.</span><span class="sxs-lookup"><span data-stu-id="7ed65-548">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-549">Plus d’informations sur les nouvelles fonctionnalités de programmation asynchrones dans le laboratoire &quot; **de programmation asynchrone dans .NET 4.5 avec c# et Visual Basic** &quot; inclus dans le Kit de formation Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ed65-549">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="7ed65-550">Tâche 2 : délais d’expiration de la gestion des jetons d’annulation</span><span class="sxs-lookup"><span data-stu-id="7ed65-550">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="7ed65-551">Méthodes d’action asynchrones qui retournent des instances de tâche peuvent prennent également en charge les délais d’expiration.</span><span class="sxs-lookup"><span data-stu-id="7ed65-551">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="7ed65-552">Dans cette tâche, vous mettrez à jour le code de la méthode Index pour traiter un scénario de délai d’attente à l’aide d’un jeton d’annulation.</span><span class="sxs-lookup"><span data-stu-id="7ed65-552">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="7ed65-553">Revenez à Visual Studio, puis appuyez sur **MAJ + F5** pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="7ed65-553">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="7ed65-554">Ajoutez le code suivant à l’aide de l’instruction à la **HomeController.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="7ed65-554">Add the following using statement to the **HomeController.cs** file.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="7ed65-555">Mettre à jour de l’action de l’Index à recevoir un **CancellationToken** argument.</span><span class="sxs-lookup"><span data-stu-id="7ed65-555">Update the Index action to receive a **CancellationToken** argument.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="7ed65-556">Mise à jour la **GetAsync** appel pour passer le jeton d’annulation.</span><span class="sxs-lookup"><span data-stu-id="7ed65-556">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="7ed65-557">(Code d’extrait de code - *SendAsync de Lab - Ex04 - ASP.NET MVC 4 avec CancellationToken*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-557">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="7ed65-558">Décorer le *Index* méthode avec un **AsyncTimeout** attribut la valeur 500 millisecondes et un **HandleError** attribut configurée pour traiter les  **TaskCanceledException** en redirigeant vers un **TimedOut** vue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-558">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="7ed65-559">(Code d’extrait de code - *ASP.NET MVC 4 Lab - Ex04 - attributs*)</span><span class="sxs-lookup"><span data-stu-id="7ed65-559">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="7ed65-560">Ouvrir le **PhotoController** classe et mise à jour la **galerie** méthode différer les millisecondes d’exécution 1000 (1 seconde) pour simuler une tâche de longue durée.</span><span class="sxs-lookup"><span data-stu-id="7ed65-560">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="7ed65-561">Ouvrez le **Web.config** de fichiers et activez les erreurs personnalisées en ajoutant l’élément suivant.</span><span class="sxs-lookup"><span data-stu-id="7ed65-561">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="7ed65-562">Créer une nouvelle vue dans **Views\Shared** nommé **TimedOut** et utiliser la disposition par défaut.</span><span class="sxs-lookup"><span data-stu-id="7ed65-562">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="7ed65-563">Dans l’Explorateur de solutions, cliquez sur le **Views\Shared** et sélectionnez **ajouter | Vue**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-563">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="7ed65-564">![À l’aide des vues différentes pour chaque périphérique mobile](whats-new-in-aspnet-mvc-4/_static/image36.png "à l’aide des vues différentes pour chaque périphérique mobile")</span><span class="sxs-lookup"><span data-stu-id="7ed65-564">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="7ed65-565">*À l’aide des vues différentes pour chaque périphérique mobile*</span><span class="sxs-lookup"><span data-stu-id="7ed65-565">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="7ed65-566">Mise à jour la **TimedOut** afficher le contenu comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7ed65-566">Update the **TimedOut** view content as shown below.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="7ed65-567">Exécutez l’application et accédez à l’URL racine.</span><span class="sxs-lookup"><span data-stu-id="7ed65-567">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="7ed65-568">Que vous avez ajouté un **Thread.Sleep** de 1 000 millisecondes, vous obtiendrez une erreur de délai d’attente, générée par le **AsyncTimeout** d’attribut et d’intercepter par le **HandleError** attribut.</span><span class="sxs-lookup"><span data-stu-id="7ed65-568">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="7ed65-569">![Exception de délai d’attente de traitement](whats-new-in-aspnet-mvc-4/_static/image37.png "exception de délai d’attente de traitement")</span><span class="sxs-lookup"><span data-stu-id="7ed65-569">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="7ed65-570">*Exception de délai d’attente de traitement*</span><span class="sxs-lookup"><span data-stu-id="7ed65-570">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="7ed65-571">En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [annexe d : publication une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="7ed65-571">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7ed65-572">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="7ed65-572">Summary</span></span>

<span data-ttu-id="7ed65-573">Cette exercices laboratoire, vous avez observé certaines des nouvelles fonctionnalités qui se trouvent dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7ed65-573">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="7ed65-574">Les concepts suivants ont été présentées :</span><span class="sxs-lookup"><span data-stu-id="7ed65-574">The following concepts have been discussed:</span></span>

- <span data-ttu-id="7ed65-575">Tirer parti des améliorations apportées à la ASP.NET MVC projet modèles, y compris le nouveau modèle de projet d’application mobile</span><span class="sxs-lookup"><span data-stu-id="7ed65-575">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="7ed65-576">Utilisez l’attribut de la fenêtre d’affichage de HTML5 et requêtes de média CSS pour améliorer l’affichage sur les périphériques mobiles</span><span class="sxs-lookup"><span data-stu-id="7ed65-576">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="7ed65-577">Utilisez jQuery Mobile pour les améliorations progressives et pour la création de l’interface utilisateur web tactiles</span><span class="sxs-lookup"><span data-stu-id="7ed65-577">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="7ed65-578">Créer des affichages mobiles spécifiques</span><span class="sxs-lookup"><span data-stu-id="7ed65-578">Create mobile-specific views</span></span>
- <span data-ttu-id="7ed65-579">Utilisez le composant de sélecteur de vue pour basculer entre les vues mobiles et de bureau dans l’application</span><span class="sxs-lookup"><span data-stu-id="7ed65-579">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="7ed65-580">Créer des contrôleurs asynchrones à l’aide de la prise en charge de la tâche</span><span class="sxs-lookup"><span data-stu-id="7ed65-580">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="7ed65-581">Annexe a : à l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="7ed65-581">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="7ed65-582">Avec des extraits de code, vous avez tout le code que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="7ed65-582">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7ed65-583">Le document lab vous indique exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="7ed65-583">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7ed65-584">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](whats-new-in-aspnet-mvc-4/_static/image38.png "des extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="7ed65-584">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7ed65-585">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="7ed65-585">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7ed65-586">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="7ed65-586">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7ed65-587">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="7ed65-587">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7ed65-588">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="7ed65-588">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7ed65-589">Observez comment IntelliSense affiche les noms des extraits de code de mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="7ed65-589">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7ed65-590">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entier est sélectionné).</span><span class="sxs-lookup"><span data-stu-id="7ed65-590">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7ed65-591">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="7ed65-591">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7ed65-592">![Commencez à taper le nom de l’extrait de code](whats-new-in-aspnet-mvc-4/_static/image39.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="7ed65-592">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7ed65-593">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="7ed65-593">*Start typing the snippet name*</span></span>

<span data-ttu-id="7ed65-594">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](whats-new-in-aspnet-mvc-4/_static/image40.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="7ed65-594">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7ed65-595">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="7ed65-595">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7ed65-596">![Appuyez sur Tab à nouveau et l’extrait de code sont développés](whats-new-in-aspnet-mvc-4/_static/image41.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="7ed65-596">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7ed65-597">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="7ed65-597">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7ed65-598">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)***</span><span class="sxs-lookup"><span data-stu-id="7ed65-598">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="7ed65-599">Clic droit où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="7ed65-599">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="7ed65-600">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-600">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="7ed65-601">Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="7ed65-601">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7ed65-602">![Avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](whats-new-in-aspnet-mvc-4/_static/image42.png "avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="7ed65-602">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7ed65-603">*Avec le bouton droit sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="7ed65-603">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7ed65-604">![Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus](whats-new-in-aspnet-mvc-4/_static/image43.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="7ed65-604">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7ed65-605">*Sélectionnez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="7ed65-605">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7ed65-606">Annexe b : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="7ed65-606">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7ed65-607">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="7ed65-607">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7ed65-608">Les instructions suivantes vous guident à travers les étapes requises pour installer *Visual studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="7ed65-608">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7ed65-609">Accédez à [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7ed65-609">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7ed65-610">Sinon, si vous avez déjà installé Web Platform Installer, vous pouvez ouvrir il et recherchez le produit &quot; *Visual Studio Express 2012 pour le Web avec Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="7ed65-610">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="7ed65-611">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-611">Click on **Install Now**.</span></span> <span data-ttu-id="7ed65-612">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="7ed65-612">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7ed65-613">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="7ed65-613">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7ed65-614">![Installer Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7ed65-614">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7ed65-615">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7ed65-615">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7ed65-616">Lisez les termes et les licences de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="7ed65-616">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="7ed65-618">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="7ed65-618">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7ed65-619">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="7ed65-619">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="7ed65-621">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="7ed65-621">*Installation progress*</span></span>
6. <span data-ttu-id="7ed65-622">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-622">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="7ed65-624">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="7ed65-624">*Installation completed*</span></span>
7. <span data-ttu-id="7ed65-625">Cliquez sur **Exit** fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="7ed65-625">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7ed65-626">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="7ed65-626">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour la vignette du Web](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="7ed65-628">*VS Express pour la vignette du Web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-628">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="7ed65-629">Annexe c : installation WebMatrix 2 et iPhone simulateur</span><span class="sxs-lookup"><span data-stu-id="7ed65-629">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="7ed65-630">Pour exécuter votre site sur un appareil simulé iPhone, vous pouvez utiliser l’extension de WebMatrix &quot;électrique simulateur Mobile pour l’iPhone&quot;.</span><span class="sxs-lookup"><span data-stu-id="7ed65-630">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="7ed65-631">En outre, vous pouvez configurer la même extension pour exécuter le simulateur de Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7ed65-631">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="7ed65-632">Tâche 1 : installation de WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="7ed65-632">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="7ed65-633">Accédez à [ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7ed65-633">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7ed65-634">Sinon, si vous avez déjà installé Web Platform Installer, vous pouvez ouvrir il et recherchez le produit &quot; *WebMatrix 2*&quot;.</span><span class="sxs-lookup"><span data-stu-id="7ed65-634">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*WebMatrix 2*&quot;.</span></span>
2. <span data-ttu-id="7ed65-635">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-635">Click on **Install Now**.</span></span> <span data-ttu-id="7ed65-636">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="7ed65-636">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7ed65-637">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="7ed65-637">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7ed65-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="7ed65-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="7ed65-639">*Install WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="7ed65-639">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="7ed65-640">Lisez les termes et les licences de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="7ed65-640">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="7ed65-641">![Accepter les termes du contrat de licence](whats-new-in-aspnet-mvc-4/_static/image50.png "acceptant les termes du contrat de licence")</span><span class="sxs-lookup"><span data-stu-id="7ed65-641">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="7ed65-642">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="7ed65-642">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7ed65-643">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="7ed65-643">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="7ed65-644">![Progression de l’installation](whats-new-in-aspnet-mvc-4/_static/image51.png "progression de l’Installation")</span><span class="sxs-lookup"><span data-stu-id="7ed65-644">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="7ed65-645">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="7ed65-645">*Installation progress*</span></span>
6. <span data-ttu-id="7ed65-646">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-646">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="7ed65-647">![L’installation est terminée](whats-new-in-aspnet-mvc-4/_static/image52.png "l’Installation est terminée")</span><span class="sxs-lookup"><span data-stu-id="7ed65-647">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="7ed65-648">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="7ed65-648">*Installation completed*</span></span>
7. <span data-ttu-id="7ed65-649">Cliquez sur **Exit** fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="7ed65-649">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="7ed65-650">Tâche 2 : installation de l’Extension de simulateur iPhone</span><span class="sxs-lookup"><span data-stu-id="7ed65-650">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="7ed65-651">Exécutez **WebMatrix** et ouvrir un site Web existant ou créez-en un.</span><span class="sxs-lookup"><span data-stu-id="7ed65-651">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="7ed65-652">Cliquez sur le **exécuter** bouton à partir de la **accueil** du ruban, puis sélectionnez **ajouter un nouveau**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-652">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="7ed65-653">![Ajout d’une nouvelle extension WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "ajouter la nouvelle extension de WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="7ed65-653">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="7ed65-654">*Ajout d’une nouvelle extension WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="7ed65-654">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="7ed65-655">Sélectionnez **iPhone simulateur** et cliquez sur **installer**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-655">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="7ed65-656">![Exploration des extensions de WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "extensions de navigation de WebMatrix")</span><span class="sxs-lookup"><span data-stu-id="7ed65-656">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="7ed65-657">*Exploration des extensions de WebMatrix*</span><span class="sxs-lookup"><span data-stu-id="7ed65-657">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="7ed65-658">Dans les détails du package, cliquez sur **installer** pour poursuivre l’installation de l’extension.</span><span class="sxs-lookup"><span data-stu-id="7ed65-658">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="7ed65-659">![iPhone d’extension de simulateur](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone extension du simulateur")</span><span class="sxs-lookup"><span data-stu-id="7ed65-659">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="7ed65-660">*iPhone d’extension du simulateur*</span><span class="sxs-lookup"><span data-stu-id="7ed65-660">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="7ed65-661">Lisez et acceptez la CLUF de l’extension.</span><span class="sxs-lookup"><span data-stu-id="7ed65-661">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="7ed65-662">![Extension de WebMatrix CLUF](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension CLUF")</span><span class="sxs-lookup"><span data-stu-id="7ed65-662">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="7ed65-663">*Extension de WebMatrix CLUF*</span><span class="sxs-lookup"><span data-stu-id="7ed65-663">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="7ed65-664">Maintenant, vous pouvez exécuter votre site Web à partir de WebMatrix à l’aide de l’option de simulateur iPhone.</span><span class="sxs-lookup"><span data-stu-id="7ed65-664">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="7ed65-665">![Exécuter à l’aide d’iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "exécutés à l’aide d’iPhone")</span><span class="sxs-lookup"><span data-stu-id="7ed65-665">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="7ed65-666">*Exécuter à l’aide d’iPhone*</span><span class="sxs-lookup"><span data-stu-id="7ed65-666">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="7ed65-667">Tâche 3 : configuration de Visual Studio 2012 pour exécuter iPhone simulateur</span><span class="sxs-lookup"><span data-stu-id="7ed65-667">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="7ed65-668">Ouvrez **Visual Studio 2012** et ouvrir n’importe quel site Web ou créez un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="7ed65-668">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="7ed65-669">Cliquez sur la flèche vers le bas à partir du bouton d’exécution et sélectionnez **naviguer avec**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-669">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="7ed65-670">![Naviguer avec](whats-new-in-aspnet-mvc-4/_static/image58.png "naviguer avec")</span><span class="sxs-lookup"><span data-stu-id="7ed65-670">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="7ed65-671">*Naviguer avec*</span><span class="sxs-lookup"><span data-stu-id="7ed65-671">*Browse with*</span></span>
3. <span data-ttu-id="7ed65-672">Dans le &quot;naviguer avec&quot; boîte de dialogue, cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-672">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="7ed65-673">Dans le &quot;ajouter un programme&quot; boîte de dialogue, utilisez les valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="7ed65-673">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

    - <span data-ttu-id="7ed65-674">**Programme**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(mise à jour en conséquence le chemin d’accès)*</span><span class="sxs-lookup"><span data-stu-id="7ed65-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)*</span></span>
    - <span data-ttu-id="7ed65-675">**Arguments**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="7ed65-675">**Arguments**: &quot;1&quot;</span></span>
    - <span data-ttu-id="7ed65-676">**Nom convivial**: iPhone simulateur</span><span class="sxs-lookup"><span data-stu-id="7ed65-676">**Friendly name**: iPhone Simulator</span></span>

    <span data-ttu-id="7ed65-677">![Ajouter un programme](whats-new-in-aspnet-mvc-4/_static/image59.png "ajouter un programme")</span><span class="sxs-lookup"><span data-stu-id="7ed65-677">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

    <span data-ttu-id="7ed65-678">*Ajouter un programme à naviguer avec*</span><span class="sxs-lookup"><span data-stu-id="7ed65-678">*Add program to browse with*</span></span>
5. <span data-ttu-id="7ed65-679">Cliquez sur **OK** et fermer les boîtes de dialogue.</span><span class="sxs-lookup"><span data-stu-id="7ed65-679">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="7ed65-680">Vous êtes en mesure d’exécuter vos applications Web dans le simulateur iPhone à partir de Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7ed65-680">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="7ed65-681">![Naviguer avec iPhone simulateur](whats-new-in-aspnet-mvc-4/_static/image60.png "naviguer avec iPhone simulateur")</span><span class="sxs-lookup"><span data-stu-id="7ed65-681">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="7ed65-682">*Naviguer avec iPhone simulateur*</span><span class="sxs-lookup"><span data-stu-id="7ed65-682">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7ed65-683">Annexe d : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7ed65-683">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="7ed65-684">Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et de publier l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7ed65-684">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="7ed65-685">Tâche 1 - Création d’un Site Web à partir de Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7ed65-685">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="7ed65-686">Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="7ed65-686">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-687">Avec Windows Azure, vous pouvez héberger des Sites Web ASP.NET 10 gratuitement et puis faire évoluer à mesure que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="7ed65-687">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="7ed65-688">Vous pouvez vous inscrire [ici](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="7ed65-688">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="7ed65-689">![Ouvrez une session sur le portail Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "connectez-vous au portail Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="7ed65-689">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="7ed65-690">*Ouvrez une session sur le portail de gestion Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="7ed65-690">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="7ed65-691">Cliquez sur **nouveau** sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="7ed65-691">Click **New** on the command bar.</span></span>

    <span data-ttu-id="7ed65-692">![Création d’un Site Web](whats-new-in-aspnet-mvc-4/_static/image62.png "création d’un Site Web")</span><span class="sxs-lookup"><span data-stu-id="7ed65-692">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="7ed65-693">*Création d’un Site Web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-693">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="7ed65-694">Cliquez sur **de calcul** | **Site Web**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-694">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="7ed65-695">Puis sélectionnez **création rapide** option.</span><span class="sxs-lookup"><span data-stu-id="7ed65-695">Then select **Quick Create** option.</span></span> <span data-ttu-id="7ed65-696">Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-696">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-697">Un Site Web de Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="7ed65-697">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="7ed65-698">L’option Création rapide vous permet de déployer une application web complète pour le Site Web Windows Azure à partir d’à l’extérieur du portail.</span><span class="sxs-lookup"><span data-stu-id="7ed65-698">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="7ed65-699">Il n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="7ed65-699">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="7ed65-700">![Création d’un Site Web à l’aide de la création rapide](whats-new-in-aspnet-mvc-4/_static/image63.png "création d’un Site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="7ed65-700">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="7ed65-701">*Création d’un Site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="7ed65-701">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="7ed65-702">Attendez que la nouvelle **Site Web** est créé.</span><span class="sxs-lookup"><span data-stu-id="7ed65-702">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="7ed65-703">Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="7ed65-703">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="7ed65-704">Vérifiez que le nouveau Site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="7ed65-704">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="7ed65-705">![Navigation vers le nouveau site web](whats-new-in-aspnet-mvc-4/_static/image64.png "exploration vers le nouveau site web")</span><span class="sxs-lookup"><span data-stu-id="7ed65-705">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="7ed65-706">*Navigation vers le nouveau site web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-706">*Browsing to the new web site*</span></span>

    <span data-ttu-id="7ed65-707">![Site Web en cours d’exécution](whats-new-in-aspnet-mvc-4/_static/image65.png "site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="7ed65-707">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="7ed65-708">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="7ed65-708">*Web site running*</span></span>
6. <span data-ttu-id="7ed65-709">Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="7ed65-709">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="7ed65-710">![Ouvrir les pages de gestion du site web](whats-new-in-aspnet-mvc-4/_static/image66.png "ouvrir les pages de gestion de site web")</span><span class="sxs-lookup"><span data-stu-id="7ed65-710">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="7ed65-711">*Ouvrir les pages de gestion de Site Web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-711">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="7ed65-712">Dans le **tableau de bord** sous le **coup de œil rapide** , cliquez sur le **télécharger le profil de publication** lien.</span><span class="sxs-lookup"><span data-stu-id="7ed65-712">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-713">Le *le profil de publication* contient toutes les informations nécessaires pour publier une application web sur un site Web de Windows Azure pour chaque méthode de publication activée.</span><span class="sxs-lookup"><span data-stu-id="7ed65-713">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="7ed65-714">Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour se connecter à et de s’authentifier auprès de chacun des points de terminaison pour laquelle une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="7ed65-714">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="7ed65-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge la lecture de publier les profils pour automatiser la configuration de ces programmes pour publication d’applications web pour les sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="7ed65-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="7ed65-716">![Profil de publication de téléchargement du site web](whats-new-in-aspnet-mvc-4/_static/image67.png "profil de publication de téléchargement du site web")</span><span class="sxs-lookup"><span data-stu-id="7ed65-716">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="7ed65-717">*Profil de publication de téléchargement du Site Web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-717">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="7ed65-718">Téléchargez le fichier de profil de publication à un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="7ed65-718">Download the publish profile file to a known location.</span></span> <span data-ttu-id="7ed65-719">Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7ed65-719">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="7ed65-720">![L’enregistrement du fichier de profil de publication](whats-new-in-aspnet-mvc-4/_static/image68.png "l’enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="7ed65-720">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="7ed65-721">*L’enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="7ed65-721">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="7ed65-722">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="7ed65-722">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="7ed65-723">Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données.</span><span class="sxs-lookup"><span data-stu-id="7ed65-723">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="7ed65-724">Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="7ed65-724">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="7ed65-725">Vous devez un serveur de base de données SQL pour stocker la base de données de l’application.</span><span class="sxs-lookup"><span data-stu-id="7ed65-725">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="7ed65-726">Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-726">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="7ed65-727">Si vous ne disposez pas d’un serveur créé, vous pouvez créer un à l’aide de la **ajouter** bouton sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="7ed65-727">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="7ed65-728">Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et un mot de passe**, comme vous allez l’utiliser dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="7ed65-728">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="7ed65-729">Ne créez pas encore, la base de données telle qu’elle sera créée dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="7ed65-729">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="7ed65-730">![Tableau de bord de serveur SQL de base de données](whats-new-in-aspnet-mvc-4/_static/image69.png "tableau de bord de serveur SQL de base de données")</span><span class="sxs-lookup"><span data-stu-id="7ed65-730">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="7ed65-731">*Tableau de bord de serveur SQL de base de données*</span><span class="sxs-lookup"><span data-stu-id="7ed65-731">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="7ed65-732">Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-732">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="7ed65-733">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP du Client actuel** et le coller dans le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) bouton.</span><span class="sxs-lookup"><span data-stu-id="7ed65-733">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![Ajout d’adresse IP du Client](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="7ed65-735">*Ajout d’adresse IP du Client*</span><span class="sxs-lookup"><span data-stu-id="7ed65-735">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="7ed65-736">Une fois la **adresse IP du Client** est ajouté aux adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="7ed65-736">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="7ed65-738">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="7ed65-738">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="7ed65-739">Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="7ed65-739">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="7ed65-740">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7ed65-740">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="7ed65-741">Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-741">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="7ed65-742">![Publication de l’Application](whats-new-in-aspnet-mvc-4/_static/image73.png "publication de l’Application")</span><span class="sxs-lookup"><span data-stu-id="7ed65-742">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="7ed65-743">*Publier le site web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-743">*Publishing the web site*</span></span>
2. <span data-ttu-id="7ed65-744">Importer le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="7ed65-744">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="7ed65-745">![L’importation du profil de publication](whats-new-in-aspnet-mvc-4/_static/image74.png "l’importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="7ed65-745">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="7ed65-746">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="7ed65-746">*Importing publish profile*</span></span>
3. <span data-ttu-id="7ed65-747">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-747">Click **Validate Connection**.</span></span> <span data-ttu-id="7ed65-748">Une fois la Validation terminée. Cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-748">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ed65-749">La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="7ed65-749">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="7ed65-750">![Validation de la connexion](whats-new-in-aspnet-mvc-4/_static/image75.png "validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="7ed65-750">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="7ed65-751">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="7ed65-751">*Validating connection*</span></span>
4. <span data-ttu-id="7ed65-752">Dans le **paramètres** sous le **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="7ed65-752">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="7ed65-753">![Configuration de déploiement Web](whats-new-in-aspnet-mvc-4/_static/image76.png "configuration de déploiement Web")</span><span class="sxs-lookup"><span data-stu-id="7ed65-753">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="7ed65-754">*Configuration de déploiement Web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-754">*Web deploy configuration*</span></span>
5. <span data-ttu-id="7ed65-755">Configurer la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="7ed65-755">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="7ed65-756">Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.</span><span class="sxs-lookup"><span data-stu-id="7ed65-756">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="7ed65-757">Dans **nom d’utilisateur** tapez le nom de connexion de votre administrateur de serveur.</span><span class="sxs-lookup"><span data-stu-id="7ed65-757">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="7ed65-758">Dans **mot de passe** votre mot de passe du compte de connexion administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="7ed65-758">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="7ed65-759">Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="7ed65-759">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="7ed65-760">![Configuration de chaîne de connexion de destination](whats-new-in-aspnet-mvc-4/_static/image77.png "configuration de chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="7ed65-760">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="7ed65-761">*Configuration de chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="7ed65-761">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="7ed65-762">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-762">Then click **OK**.</span></span> <span data-ttu-id="7ed65-763">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-763">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="7ed65-764">![Création de la base de données](whats-new-in-aspnet-mvc-4/_static/image78.png "création de la chaîne de la base de données")</span><span class="sxs-lookup"><span data-stu-id="7ed65-764">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="7ed65-765">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="7ed65-765">*Creating the database*</span></span>
7. <span data-ttu-id="7ed65-766">La chaîne de connexion que vous allez utiliser pour se connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte par défaut de connexion.</span><span class="sxs-lookup"><span data-stu-id="7ed65-766">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="7ed65-767">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-767">Then click **Next**.</span></span>

    <span data-ttu-id="7ed65-768">![Chaîne de connexion pointant vers la base de données SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "chaîne de connexion pointant vers la base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="7ed65-768">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="7ed65-769">*Chaîne de connexion pointant vers la base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="7ed65-769">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="7ed65-770">Dans le **aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="7ed65-770">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="7ed65-771">![Publication de l’application web](whats-new-in-aspnet-mvc-4/_static/image80.png "publication de l’application web")</span><span class="sxs-lookup"><span data-stu-id="7ed65-771">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="7ed65-772">*Publication de l’application web*</span><span class="sxs-lookup"><span data-stu-id="7ed65-772">*Publishing the web application*</span></span>
9. <span data-ttu-id="7ed65-773">Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.</span><span class="sxs-lookup"><span data-stu-id="7ed65-773">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="7ed65-774">![Publication d’application sur Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application publiée dans Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="7ed65-774">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="7ed65-775">*Application de publication sur Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="7ed65-775">*Application published to Windows Azure*</span></span>
