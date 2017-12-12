---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: "Ateliers pratiques : Visual Studio 2013 Web Tools | Documents Microsoft"
author: rick-anderson
description: "Visual Studio est un excellent environnement de développement pour. Windows basé sur le réseau et les projets web. Il inclut un éditeur de texte puissant qui peut facilement être utilisé pour..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="19fea-104">Ateliers pratiques : Outils de Web Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="19fea-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>
====================
<span data-ttu-id="19fea-105">par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="19fea-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="19fea-106">Télécharger Camps Web Kit de formation</span><span class="sxs-lookup"><span data-stu-id="19fea-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="19fea-107">Visual Studio est un excellent environnement de développement pour. Windows basé sur le réseau et les projets web.</span><span class="sxs-lookup"><span data-stu-id="19fea-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="19fea-108">Il inclut un éditeur de texte puissant qui peut facilement être utilisé pour modifier les fichiers autonomes sans un projet.</span><span class="sxs-lookup"><span data-stu-id="19fea-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="19fea-109">Visual Studio conserve une arborescence de l’analyse complète lorsque vous modifiez chaque fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="19fea-110">Cela permet à Visual Studio fournir une saisie semi-automatique et des actions basées sur le document lors de l’expérience de développement beaucoup plus rapide et plus agréable.</span><span class="sxs-lookup"><span data-stu-id="19fea-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="19fea-111">Ces fonctionnalités sont particulièrement efficace dans des documents HTML et CSS.</span><span class="sxs-lookup"><span data-stu-id="19fea-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="19fea-112">Cette puissance est également disponible pour les extensions, rendant simple à étendre les éditeurs de nouvelles fonctionnalités puissantes pour répondre à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="19fea-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="19fea-113">Web Essentials est un ensemble d’améliorations (principalement) liés au web pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="19fea-114">Il comprend un grand nombre de nouveau achèvements IntelliSense (en particulier pour CSS), les nouvelles fonctionnalités de lien de navigateur, automatique des fichiers JSHint pour JavaScript, nouveaux avertissements pour HTML et CSS et de nombreuses autres fonctionnalités qui sont essentielles pour le développement web moderne.</span><span class="sxs-lookup"><span data-stu-id="19fea-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="19fea-115">Tous les exemples de code et des extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="19fea-115">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="19fea-116">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="19fea-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="19fea-117">Objectifs</span><span class="sxs-lookup"><span data-stu-id="19fea-117">Objectives</span></span>

<span data-ttu-id="19fea-118">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="19fea-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="19fea-119">Utiliser les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, tels que des extraits de code HTML5 riches et la simplicité de codage</span><span class="sxs-lookup"><span data-stu-id="19fea-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="19fea-120">Utiliser les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials, tels que le sélecteur de couleurs et une info-bulle de la matrice navigateur</span><span class="sxs-lookup"><span data-stu-id="19fea-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="19fea-121">Utiliser les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Essentials Web tels que d’extraire vers un fichier et IntelliSense pour tous les éléments HTML</span><span class="sxs-lookup"><span data-stu-id="19fea-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="19fea-122">Échanger des données entre votre navigateur et de Visual Studio à l’aide du lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="19fea-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="19fea-123">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="19fea-123">Prerequisites</span></span>

<span data-ttu-id="19fea-124">Les éléments suivants sont nécessaire pour terminer cet atelier pratique :</span><span class="sxs-lookup"><span data-stu-id="19fea-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="19fea-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou supérieur</span><span class="sxs-lookup"><span data-stu-id="19fea-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="19fea-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="19fea-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="19fea-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="19fea-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="19fea-128">Installation</span><span class="sxs-lookup"><span data-stu-id="19fea-128">Setup</span></span>

<span data-ttu-id="19fea-129">Pour exécuter les exercices de cet atelier, vous devez configurer votre environnement tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="19fea-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="19fea-130">Ouvrez une fenêtre de l’Explorateur Windows et accédez à l’atelier **Source** dossier.</span><span class="sxs-lookup"><span data-stu-id="19fea-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="19fea-131">Avec le bouton droit **Setup.cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui sera configurer votre environnement et installer les extraits de code Visual Studio pour ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="19fea-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="19fea-132">Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.</span><span class="sxs-lookup"><span data-stu-id="19fea-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="19fea-133">Assurez-vous que vous avez activé toutes les dépendances pour ce laboratoire avant d’exécuter le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="19fea-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="19fea-134">À l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="19fea-134">Using the Code Snippets</span></span>

<span data-ttu-id="19fea-135">Dans le document de laboratoire, vous serez invité à insérer des blocs de code.</span><span class="sxs-lookup"><span data-stu-id="19fea-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="19fea-136">Pour des raisons pratiques, la majeure partie de ce code est fourni en tant que Visual Studio extraits de Code, vous pouvez accéder à partir de Visual Studio 2013 pour éviter d’avoir à ajouter manuellement.</span><span class="sxs-lookup"><span data-stu-id="19fea-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="19fea-137">Chaque exercice est accompagnée d’une solution de départ située dans le **commencer** dossier de l’exercice qui vous permet de suivre chaque exercice indépendamment des autres.</span><span class="sxs-lookup"><span data-stu-id="19fea-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="19fea-138">Sachez que les extraits de code sont ajoutés au cours d’un exercice sont manquants à partir de ces solutions de démarrage et peut ne pas fonctionneront tant que vous avez terminé l’exercice.</span><span class="sxs-lookup"><span data-stu-id="19fea-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="19fea-139">Dans le code source pour un exercice, vous trouverez également une **fin** dossier qui contient une solution Visual Studio avec le code qui résulte de la procédure dans l’exercice correspondant.</span><span class="sxs-lookup"><span data-stu-id="19fea-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="19fea-140">Si vous avez besoin d’aide au cours de cet atelier, vous pouvez utiliser ces solutions comme guide.</span><span class="sxs-lookup"><span data-stu-id="19fea-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="19fea-141">Exercices</span><span class="sxs-lookup"><span data-stu-id="19fea-141">Exercises</span></span>

<span data-ttu-id="19fea-142">Cet atelier pratique inclut les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="19fea-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="19fea-143">Utilisation de lien de navigateur et Web Essentials</span><span class="sxs-lookup"><span data-stu-id="19fea-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="19fea-144">Tirant parti des extraits de Code IntelliSense</span><span class="sxs-lookup"><span data-stu-id="19fea-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="19fea-145">Lorsque vous démarrez Visual Studio, vous devez sélectionner une des collections de paramètres prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="19fea-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="19fea-146">Chaque collection prédéfinie est conçue pour faire correspondre un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, extraits de code IntelliSense et les options de boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="19fea-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="19fea-147">Les procédures de cet atelier décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lorsque vous utilisez la **paramètres de développement généraux** collection.</span><span class="sxs-lookup"><span data-stu-id="19fea-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="19fea-148">Si vous choisissez une collection de paramètres différents pour votre environnement de développement, il est possible les différences dans les étapes que vous devez prendre en compte.</span><span class="sxs-lookup"><span data-stu-id="19fea-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="19fea-149">Exercice 1 : Utilisation de lien du navigateur et Web Essentials</span><span class="sxs-lookup"><span data-stu-id="19fea-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="19fea-150">**Web Essentials** est une extension de Visual Studio qui ajoute de nombreuses fonctionnalités utiles pour le développement web moderne, principalement pour but de rendre l’expérience de développement web beaucoup plus rapide et plus agréable.</span><span class="sxs-lookup"><span data-stu-id="19fea-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="19fea-151">Vous pouvez installer Web Essentials à partir de la galerie d’extensions dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="19fea-152">**Lien du navigateur** est une nouvelle fonctionnalité incluse dans Visual Studio 2013 qui fournit un canal entre l’IDE de Visual Studio et sur n’importe quel navigateur ouvert pour échanger des données entre votre application web et de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="19fea-153">Web Essentials étend le lien du navigateur avec des outils pour manipuler le modèle d’objet DOM et les styles CSS de vos pages web directement à partir du navigateur.</span><span class="sxs-lookup"><span data-stu-id="19fea-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="19fea-154">Dans cet exercice, vous allez explorer certaines des fonctionnalités prises en charge par **Web Essentials** et **lien du navigateur** pour améliorer une page simple questionnaire.</span><span class="sxs-lookup"><span data-stu-id="19fea-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="19fea-155">Tâche 1 : le projet en cours d’exécution dans plusieurs navigateurs</span><span class="sxs-lookup"><span data-stu-id="19fea-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="19fea-156">Dans cette tâche, vous allez configurer votre application web à exécuter dans plusieurs navigateurs à la fois, ce qui est utile pour le test de plusieurs navigateurs.</span><span class="sxs-lookup"><span data-stu-id="19fea-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="19fea-157">Ouvrez **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="19fea-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="19fea-158">Dans le **fichier** menu, sélectionnez **ouvrir | Projet/Solution...**  et accédez à **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** dans les **Source** dossier de l’atelier (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="19fea-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="19fea-159">Sélectionnez **Begin.sln** et cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="19fea-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="19fea-160">Dans la barre d’outils Visual Studio, développez le menu du navigateur, puis sélectionnez **naviguer avec...** .</span><span class="sxs-lookup"><span data-stu-id="19fea-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="19fea-161">![Naviguer avec l’option de menu](visual-studio-2013-web-tools/_static/image1.png "naviguer avec... dans le menu du navigateur")</span><span class="sxs-lookup"><span data-stu-id="19fea-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="19fea-162">*Naviguer avec l’option de menu*</span><span class="sxs-lookup"><span data-stu-id="19fea-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="19fea-163">Dans le **naviguer avec** boîte de dialogue, sélectionnez **Google Chrome** et **Internet Explorer** en maintenant enfoncée la **CTRL** clé et cliquez sur  **Définir comme valeur par défaut**.</span><span class="sxs-lookup"><span data-stu-id="19fea-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="19fea-164">![Accédez à la boîte de dialogue](visual-studio-2013-web-tools/_static/image2.png "naviguer avec la boîte de dialogue")</span><span class="sxs-lookup"><span data-stu-id="19fea-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="19fea-165">*Sélection de plusieurs navigateurs par défaut*</span><span class="sxs-lookup"><span data-stu-id="19fea-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="19fea-166">Google Chrome et Internet Explorer doivent maintenant apparaître en tant que les navigateurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="19fea-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="19fea-167">Cliquez sur **Annuler** pour fermer la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="19fea-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="19fea-168">![Google Chrome et Internet Explorer en tant que des navigateurs par défaut](visual-studio-2013-web-tools/_static/image3.png "des navigateurs par défaut Google Chrome et Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="19fea-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="19fea-169">*Google Chrome et Internet Explorer en tant que des navigateurs par défaut*</span><span class="sxs-lookup"><span data-stu-id="19fea-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-170">Après avoir configuré les navigateurs par défaut, le **plusieurs navigateurs** option est sélectionnée dans le menu du navigateur.</span><span class="sxs-lookup"><span data-stu-id="19fea-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="19fea-171">![Plusieurs navigateurs](visual-studio-2013-web-tools/_static/image4.png "plusieurs navigateurs")</span><span class="sxs-lookup"><span data-stu-id="19fea-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="19fea-172">Appuyez sur **CTRL** + **F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="19fea-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="19fea-173">Lorsque les deux fenêtres du navigateur s’ouvre, placez un d’eux au-dessus de l’autre pour voir les mises à jour sur les deux navigateurs simultanément.</span><span class="sxs-lookup"><span data-stu-id="19fea-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="19fea-174">Les navigateurs doivent afficher une page web avec un rectangle bleu clair.</span><span class="sxs-lookup"><span data-stu-id="19fea-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="19fea-175">![Placement d’un navigateur au-dessus des autres](visual-studio-2013-web-tools/_static/image5.png "plaçant au-dessus de l’autre navigateur")</span><span class="sxs-lookup"><span data-stu-id="19fea-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="19fea-176">*Placement d’un navigateur au-dessus de l’autre*</span><span class="sxs-lookup"><span data-stu-id="19fea-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="19fea-177">Ne fermez pas les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="19fea-177">Do not close the browsers.</span></span> <span data-ttu-id="19fea-178">Vous allez les utiliser dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="19fea-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="19fea-179">Tâche 2 : à l’aide de simplicité de codage pour créer des éléments HTML</span><span class="sxs-lookup"><span data-stu-id="19fea-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="19fea-180">**Simplicité de codage** est un éditeur de code de plug-in pour haut débit HTML, XML, XSL (ou tout autre format de code structuré) et la modification.</span><span class="sxs-lookup"><span data-stu-id="19fea-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="19fea-181">Le cœur de ce plug-in est un moteur abréviation puissant qui vous permet de développer des expressions - semblables aux sélecteurs CSS - dans le code HTML.</span><span class="sxs-lookup"><span data-stu-id="19fea-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="19fea-182">Simplicité de codage est un moyen rapide d’écrire la syntaxe du sélecteur de style HTML à l’aide de CSS.</span><span class="sxs-lookup"><span data-stu-id="19fea-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="19fea-183">Dans cet exercice, vous utiliserez la fonctionnalité de simplicité de codage fournie par Web Essentials pour générer les boutons HTML qui représentent les options de la question.</span><span class="sxs-lookup"><span data-stu-id="19fea-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="19fea-184">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="19fea-185">Ouvrez le **Index.cshtml** fichier situé dans le **vues** | **accueil** dossier.</span><span class="sxs-lookup"><span data-stu-id="19fea-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="19fea-186">Remplacez le  **&lt;!--TODO : ajoutez ici--options de&gt;**  commentaire avec le code suivant et appuyez sur **onglet**.</span><span class="sxs-lookup"><span data-stu-id="19fea-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="19fea-187">Le code doit être développé au format HTML.</span><span class="sxs-lookup"><span data-stu-id="19fea-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="19fea-188">![Développé HTML](visual-studio-2013-web-tools/_static/image6.png "développé HTML")</span><span class="sxs-lookup"><span data-stu-id="19fea-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="19fea-189">*HTML développé*</span><span class="sxs-lookup"><span data-stu-id="19fea-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-190">Pour en savoir plus sur la syntaxe de simplicité de codage, consultez la rubrique suivante [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span><span class="sxs-lookup"><span data-stu-id="19fea-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="19fea-191">Cliquez sur le **actualiser navigateurs liés** bouton Mettre à jour les deux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="19fea-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="19fea-192">![Actualiser les navigateurs liés](visual-studio-2013-web-tools/_static/image7.png "actualiser navigateurs liés")</span><span class="sxs-lookup"><span data-stu-id="19fea-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="19fea-193">*Actualiser les navigateurs liés*</span><span class="sxs-lookup"><span data-stu-id="19fea-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="19fea-194">![Internet Explorer - Page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page mise à jour avec quatre boutons")</span><span class="sxs-lookup"><span data-stu-id="19fea-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="19fea-195">*Internet Explorer - Page mise à jour avec quatre boutons*</span><span class="sxs-lookup"><span data-stu-id="19fea-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="19fea-196">![Google Chrome - Page mise à jour avec quatre boutons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page mise à jour avec quatre boutons")</span><span class="sxs-lookup"><span data-stu-id="19fea-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="19fea-197">*Google Chrome - Page mise à jour avec quatre boutons*</span><span class="sxs-lookup"><span data-stu-id="19fea-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="19fea-198">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="19fea-199">Vous avez ajouté les boutons à la page, mais vous devez toujours ajouter une question de modèle.</span><span class="sxs-lookup"><span data-stu-id="19fea-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="19fea-200">Pour ce faire, vous allez utiliser une nouvelle fonctionnalité dans Essentials Web appelé **Lorem Ipsum Générateur**.</span><span class="sxs-lookup"><span data-stu-id="19fea-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="19fea-201">Recherchez le **div** élément avec la **classe** attribut **avant**.</span><span class="sxs-lookup"><span data-stu-id="19fea-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="19fea-202">Ajoutez le code suivant en tant que le premier élément enfant de le **div**, puis appuyez sur **onglet**.</span><span class="sxs-lookup"><span data-stu-id="19fea-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="19fea-203">Le code doit être développé au format HTML.</span><span class="sxs-lookup"><span data-stu-id="19fea-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="19fea-204">![Généré automatiquement de Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum automatiquement")</span><span class="sxs-lookup"><span data-stu-id="19fea-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="19fea-205">*Lorem Ipsum automatiquement*</span><span class="sxs-lookup"><span data-stu-id="19fea-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-206">Dans le cadre de la simplicité de codage, vous pouvez maintenant générer Lorem Ipsum code directement dans l’éditeur HTML.</span><span class="sxs-lookup"><span data-stu-id="19fea-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="19fea-207">Tapez simplement **lorem** puis appuyez sur **onglet** et un 30 word Lorem Ipsum texte sera inséré.</span><span class="sxs-lookup"><span data-stu-id="19fea-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="19fea-208">Par exemple</span><span class="sxs-lookup"><span data-stu-id="19fea-208">E.g.</span></span> <span data-ttu-id="19fea-209">*lorem10* insère 10 Lorem Ipsum mots.</span><span class="sxs-lookup"><span data-stu-id="19fea-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="19fea-210">Vous allez ajouter un logo en haut de la question à l’aide d’une autre fonctionnalité nouvelle dans Essentials Web appelé **Générateur de Pixel de Lorem**.</span><span class="sxs-lookup"><span data-stu-id="19fea-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="19fea-211">Ajoutez le code suivant en tant que le premier élément enfant de le **div** élément avec **conteneur** en tant que **classe** valeur et appuyez sur **onglet**.</span><span class="sxs-lookup"><span data-stu-id="19fea-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="19fea-212">Le code doit se développer au format HTML.</span><span class="sxs-lookup"><span data-stu-id="19fea-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="19fea-213">![Généré automatiquement de Lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel automatiquement")</span><span class="sxs-lookup"><span data-stu-id="19fea-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="19fea-214">*Généré automatiquement de Lorem Pixel*</span><span class="sxs-lookup"><span data-stu-id="19fea-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-215">Dans le cadre de la simplicité de codage, vous pouvez également générer le code Lorem pixels directement dans l’éditeur HTML.</span><span class="sxs-lookup"><span data-stu-id="19fea-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="19fea-216">Tapez simplement **pix-200 x 200-animaux** puis appuyez sur **onglet** et un **img** balise avec une image de 200 x 200 d’un animal sera insérée.</span><span class="sxs-lookup"><span data-stu-id="19fea-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="19fea-217">Pour plus d’informations, reportez-vous à [Lorem Pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="19fea-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="19fea-218">Cliquez sur le **actualiser navigateurs liés** bouton Mettre à jour les deux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="19fea-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="19fea-219">![Internet Explorer - générées automatiquement image et texte](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - générées automatiquement image et texte")</span><span class="sxs-lookup"><span data-stu-id="19fea-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="19fea-220">*Internet Explorer - générées automatiquement image et texte*</span><span class="sxs-lookup"><span data-stu-id="19fea-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="19fea-221">![Google Chrome - générées automatiquement image et texte](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - générées automatiquement image et texte")</span><span class="sxs-lookup"><span data-stu-id="19fea-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="19fea-222">*Google Chrome - générées automatiquement image et texte*</span><span class="sxs-lookup"><span data-stu-id="19fea-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-223">Étant donné que l’image est sélectionnée de manière aléatoire lors de l’ajout de l’extrait de code, l’image affichée dans les navigateurs peut-être différer.</span><span class="sxs-lookup"><span data-stu-id="19fea-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="19fea-224">Ne fermez pas les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="19fea-224">Do not close the browsers.</span></span> <span data-ttu-id="19fea-225">Vous allez les utiliser dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="19fea-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="19fea-226">Tâche 3 : mise à jour d’une propriété de Style</span><span class="sxs-lookup"><span data-stu-id="19fea-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="19fea-227">Dans cette tâche, vous allez utiliser le lien de navigateur **Inspecter le Mode** fonctionnalité pour détecter l’emplacement exact où l’élément DOM spécifique est généré et ensuite mettre à jour la propriété de couleur de l’élément à l’aide d’un sélecteur de couleurs fourni par le Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="19fea-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="19fea-228">Dans le navigateur Internet Explorer, appuyez sur **CTRL** + **ALT** + **I** pour activer le Mode d’inspecter.</span><span class="sxs-lookup"><span data-stu-id="19fea-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="19fea-229">Déplacez le pointeur sur la bordure bleue claire et cliquez sur.</span><span class="sxs-lookup"><span data-stu-id="19fea-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="19fea-230">![Placez le pointeur sur la bordure bleue claire](visual-studio-2013-web-tools/_static/image14.png "déplacement du pointeur sur la bordure bleue claire")</span><span class="sxs-lookup"><span data-stu-id="19fea-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="19fea-231">*Placez le pointeur sur la bordure bleue claire*</span><span class="sxs-lookup"><span data-stu-id="19fea-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="19fea-232">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="19fea-233">Notez comment l’élément HTML que vous avez sélectionné dans le navigateur est également sélectionné dans l’éditeur HTML de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="19fea-234">![Élément HTML sélectionné dans l’éditeur HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "élément HTML sélectionné dans l’éditeur HTML de Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="19fea-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="19fea-235">*Élément HTML sélectionné dans l’éditeur HTML de Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="19fea-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="19fea-236">Vous met alors à jour la **avant** classe CSS pour modifier le style de l’élément sélectionné.</span><span class="sxs-lookup"><span data-stu-id="19fea-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="19fea-237">Pour ce faire, appuyez sur **CTRL** + **,** pour ouvrir le **naviguer vers** zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="19fea-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="19fea-238">Type **site.css** et appuyez sur **entrée** pour ouvrir le fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="19fea-239">![Ouverture du fichier Site.css](visual-studio-2013-web-tools/_static/image16.png "d’ouverture du fichier Site.css")</span><span class="sxs-lookup"><span data-stu-id="19fea-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="19fea-240">*Ouverture du fichier Site.css*</span><span class="sxs-lookup"><span data-stu-id="19fea-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="19fea-241">Appuyez sur **CTRL** + **F** et type **.flip-Container .front** pour trouver le sélecteur CSS.</span><span class="sxs-lookup"><span data-stu-id="19fea-241">Press **CTRL** + **F** and type **.flip-containter .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="19fea-242">Cliquez sur le carré bleu clair dans la propriété de la bordure de la classe pour ouvrir le sélecteur de couleurs.</span><span class="sxs-lookup"><span data-stu-id="19fea-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="19fea-243">![Ouvrir le sélecteur de couleurs](visual-studio-2013-web-tools/_static/image17.png "ouvrir le sélecteur de couleurs")</span><span class="sxs-lookup"><span data-stu-id="19fea-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="19fea-244">*Ouvrir le sélecteur de couleurs*</span><span class="sxs-lookup"><span data-stu-id="19fea-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="19fea-245">Développez le sélecteur de couleurs en cliquant sur le bouton chevron et sélectionnez une nouvelle couleur.</span><span class="sxs-lookup"><span data-stu-id="19fea-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="19fea-246">![Développer le sélecteur de couleurs](visual-studio-2013-web-tools/_static/image18.png "développer le sélecteur de couleurs")</span><span class="sxs-lookup"><span data-stu-id="19fea-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="19fea-247">*Développer le sélecteur de couleurs*</span><span class="sxs-lookup"><span data-stu-id="19fea-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="19fea-248">Appuyez sur **CTRL** + **ALT** + **entrée** pour actualiser les navigateurs liés.</span><span class="sxs-lookup"><span data-stu-id="19fea-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="19fea-249">Basculez vers Internet Explorer et notez la manière dont la couleur de la bordure a changé.</span><span class="sxs-lookup"><span data-stu-id="19fea-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="19fea-250">![Internet Explorer - mise à jour de couleur de bordure](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - mise à jour de couleur de bordure")</span><span class="sxs-lookup"><span data-stu-id="19fea-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="19fea-251">*Internet Explorer - mise à jour de couleur de bordure*</span><span class="sxs-lookup"><span data-stu-id="19fea-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="19fea-252">Basculez vers Google Chrome et notez la manière dont la couleur de la bordure a changé.</span><span class="sxs-lookup"><span data-stu-id="19fea-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="19fea-253">![Google Chrome - couleur de bordure de mise à jour](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - couleur de bordure de mise à jour")</span><span class="sxs-lookup"><span data-stu-id="19fea-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="19fea-254">*Google Chrome - couleur de bordure de mise à jour*</span><span class="sxs-lookup"><span data-stu-id="19fea-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="19fea-255">Revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="19fea-256">Accédez à la fin de la **Site.css** de fichiers et appuyez sur **CTRL** + **F** pour localiser le **.btn** sélecteur.</span><span class="sxs-lookup"><span data-stu-id="19fea-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="19fea-257">Notez que la **- webkit-border-radius** propriété est soulignée en vert.</span><span class="sxs-lookup"><span data-stu-id="19fea-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="19fea-258">![propriété - webkit-border-radius du bouton sélecteur](visual-studio-2013-web-tools/_static/image21.png "propriété - webkit-border-radius du sélecteur de bouton")</span><span class="sxs-lookup"><span data-stu-id="19fea-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="19fea-259">*propriété - webkit-border-radius du sélecteur de bouton*</span><span class="sxs-lookup"><span data-stu-id="19fea-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="19fea-260">Placer le signe insertion dans la **- webkit-border-radius** propriété.</span><span class="sxs-lookup"><span data-stu-id="19fea-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="19fea-261">Une ligne bleue doit apparaître sous la première lettre du premier mot de la propriété.</span><span class="sxs-lookup"><span data-stu-id="19fea-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="19fea-262">Il s’agit de la **des balises actives**.</span><span class="sxs-lookup"><span data-stu-id="19fea-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="19fea-263">Appuyez sur **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="19fea-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="19fea-264">Pour ouvrir le menu des suggestions sur **Ajouter propriété standard (valeurs border-radius) manquante**.</span><span class="sxs-lookup"><span data-stu-id="19fea-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="19fea-265">![Ajouter manquant suggestion de propriété standard](visual-studio-2013-web-tools/_static/image22.png "ajouter manquant suggestion de propriété standard")</span><span class="sxs-lookup"><span data-stu-id="19fea-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="19fea-266">*Ajouter manquant suggestion de propriété standard*</span><span class="sxs-lookup"><span data-stu-id="19fea-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="19fea-267">Le **valeurs border-radius** propriété est automatiquement ajoutée à la règle CSS.</span><span class="sxs-lookup"><span data-stu-id="19fea-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="19fea-268">![Propriété standard ajouté](visual-studio-2013-web-tools/_static/image23.png "pas ajoutée de propriété standard")</span><span class="sxs-lookup"><span data-stu-id="19fea-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="19fea-269">*Propriété standard ajoutée manquante*</span><span class="sxs-lookup"><span data-stu-id="19fea-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="19fea-270">Déplacez le pointeur sur le **valeurs border-radius** propriété pour afficher le **info-bulle de la matrice navigateur**.</span><span class="sxs-lookup"><span data-stu-id="19fea-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="19fea-271">Le **info-bulle de la matrice navigateur** montre la disponibilité de la propriété dans chaque navigateur.</span><span class="sxs-lookup"><span data-stu-id="19fea-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="19fea-272">![Info-bulle de la matrice navigateur](visual-studio-2013-web-tools/_static/image24.png "info-bulle de la matrice navigateur")</span><span class="sxs-lookup"><span data-stu-id="19fea-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="19fea-273">*Info-bulle de la matrice navigateur*</span><span class="sxs-lookup"><span data-stu-id="19fea-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="19fea-274">Notez que la valeur de la **valeurs border-radius** propriété est toujours soulignée.</span><span class="sxs-lookup"><span data-stu-id="19fea-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="19fea-275">Déplacez le pointeur sur la valeur pour afficher le message d’avertissement.</span><span class="sxs-lookup"><span data-stu-id="19fea-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="19fea-276">![Avertissement de valeur de propriété de valeurs border-radius](visual-studio-2013-web-tools/_static/image25.png "avertissement de valeur de propriété Border-radius")</span><span class="sxs-lookup"><span data-stu-id="19fea-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="19fea-277">*Avertissement de valeur de propriété de valeurs border-radius*</span><span class="sxs-lookup"><span data-stu-id="19fea-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="19fea-278">Supprimez l’unité de la **valeurs border-radius** valeur de la propriété comme suggéré par l’info-bulle.</span><span class="sxs-lookup"><span data-stu-id="19fea-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="19fea-279">En tant que **valeurs border-radius** est la propriété standard pour définir comment arrondi bordure angles sont, vous pouvez supprimer la **- webkit-border-radius** propriété et la valeur de la règle CSS.</span><span class="sxs-lookup"><span data-stu-id="19fea-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="19fea-280">Placer le signe insertion dans la **retour** propriété et notez que la balise active apparaît également sous.</span><span class="sxs-lookup"><span data-stu-id="19fea-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="19fea-281">Ouvrez le menu et cliquez sur **manquant fournisseur préciser**.</span><span class="sxs-lookup"><span data-stu-id="19fea-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="19fea-282">![Ajouter la suggestion de caractéristiques de l’objet fournisseur manquant](visual-studio-2013-web-tools/_static/image26.png "ajouter suggestion de caractéristiques de l’objet fournisseur manquant")</span><span class="sxs-lookup"><span data-stu-id="19fea-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="19fea-283">*Ajouter la suggestion de caractéristiques de l’objet fournisseur manquant*</span><span class="sxs-lookup"><span data-stu-id="19fea-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="19fea-284">Le **-ms-word-wrap** propriété est automatiquement ajoutée à la règle CSS.</span><span class="sxs-lookup"><span data-stu-id="19fea-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="19fea-285">![Propriété spécifique de fournisseur ajoutée](visual-studio-2013-web-tools/_static/image27.png "propriété spécifique de fournisseur ajoutée")</span><span class="sxs-lookup"><span data-stu-id="19fea-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="19fea-286">*Propriété spécifique de fournisseur ajoutée*</span><span class="sxs-lookup"><span data-stu-id="19fea-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="19fea-287">Tâche 4 - mise à jour le Code HTML à partir du navigateur</span><span class="sxs-lookup"><span data-stu-id="19fea-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="19fea-288">Dans cette tâche, vous allez utiliser le lien de navigateur **en Mode Création** fonctionnalité pour modifier l’objet DOM à partir du navigateur et de transférer les modifications effectuées dans le fichier source HTML dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19fea-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="19fea-289">Google Chrome, appuyez sur **CTRL** + **ALT** + **D** pour activer le Mode Création.</span><span class="sxs-lookup"><span data-stu-id="19fea-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="19fea-290">Déplacez le pointeur sur le **Lorem Ipsum dolor sit amet** étiquette et cliquez sur.</span><span class="sxs-lookup"><span data-stu-id="19fea-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="19fea-291">![Modification de la question](visual-studio-2013-web-tools/_static/image28.png "modification de la question")</span><span class="sxs-lookup"><span data-stu-id="19fea-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="19fea-292">*Modification de la question*</span><span class="sxs-lookup"><span data-stu-id="19fea-292">*Editing the question*</span></span>
3. <span data-ttu-id="19fea-293">Un curseur doit apparaître.</span><span class="sxs-lookup"><span data-stu-id="19fea-293">A cursor should appear.</span></span> <span data-ttu-id="19fea-294">Remplacez le texte d’origine par *à quoi elle ressemble lors de l’écriture d’une question de plus de temps ?*, puis appuyez sur **ÉCHAP** pour quitter le Mode de conception.</span><span class="sxs-lookup"><span data-stu-id="19fea-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="19fea-295">![Question modifiée](visual-studio-2013-web-tools/_static/image29.png "Question modifiée")</span><span class="sxs-lookup"><span data-stu-id="19fea-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="19fea-296">*Question modifiée*</span><span class="sxs-lookup"><span data-stu-id="19fea-296">*Question edited*</span></span>
4. <span data-ttu-id="19fea-297">Commutateur vers Visual Studio et ouvrez **Index.cshtml**, si pas déjà ouvert.</span><span class="sxs-lookup"><span data-stu-id="19fea-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="19fea-298">Notez que le texte interne de la  **&lt;p&gt;**  élément a été mis à jour.</span><span class="sxs-lookup"><span data-stu-id="19fea-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="19fea-299">![Point de mise à jour dans la page HTML](visual-studio-2013-web-tools/_static/image30.png "question de mise à jour dans la page HTML")</span><span class="sxs-lookup"><span data-stu-id="19fea-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="19fea-300">*Point de mise à jour dans la page HTML*</span><span class="sxs-lookup"><span data-stu-id="19fea-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="19fea-301">Avertissements liés à la tâche 5 : révision des moteurs de recherche</span><span class="sxs-lookup"><span data-stu-id="19fea-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="19fea-302">**L’optimisation des moteurs de recherche** (SEO) est le processus de fabrication d’un site Web sont classés dans la liste d’un moteur de recherche de résultats.</span><span class="sxs-lookup"><span data-stu-id="19fea-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="19fea-303">Plus le site évalue et plus cohérente, il est répertorié, les visiteurs plus le site obtiennent à partir de ce moteur de recherche.</span><span class="sxs-lookup"><span data-stu-id="19fea-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="19fea-304">Web Essentials intègre un outil d’analyse qui examine le code HTML, les rapports les problèmes trouvés et fournit une assistance pour les corriger.</span><span class="sxs-lookup"><span data-stu-id="19fea-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="19fea-305">Accédez à la **vue** menu et cliquez sur **liste d’erreurs** pour ouvrir le **liste d’erreurs** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="19fea-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="19fea-306">![Liste d’erreurs dans la vue menu](visual-studio-2013-web-tools/_static/image31.png "liste d’erreurs dans le menu Affichage")</span><span class="sxs-lookup"><span data-stu-id="19fea-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="19fea-307">*Liste d’erreurs dans la vue menu*</span><span class="sxs-lookup"><span data-stu-id="19fea-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="19fea-308">Notez qu’il existe un avertissement de moteurs de recherche qui notifier un  **&lt;meta&gt;**  de balise pour la description de la page est manquante.</span><span class="sxs-lookup"><span data-stu-id="19fea-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="19fea-309">Double-cliquez sur l’entrée d’avertissement de moteurs de recherche pour le corriger.</span><span class="sxs-lookup"><span data-stu-id="19fea-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="19fea-310">![Fenêtre liste d’erreurs](visual-studio-2013-web-tools/_static/image32.png "fenêtre liste d’erreurs")</span><span class="sxs-lookup"><span data-stu-id="19fea-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="19fea-311">*Fenêtre liste d’erreurs*</span><span class="sxs-lookup"><span data-stu-id="19fea-311">*Error List window*</span></span>
3. <span data-ttu-id="19fea-312">Dans le **Web Essentials** boîte de dialogue, cliquez sur **Oui** pour insérer une description &lt;meta&gt; balise.</span><span class="sxs-lookup"><span data-stu-id="19fea-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="19fea-313">![Boîte de dialogue Web Essentials](visual-studio-2013-web-tools/_static/image33.png "boîte de dialogue Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="19fea-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="19fea-314">*Boîte de dialogue Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="19fea-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="19fea-315">L’éditeur pour  **\_Layout.cshtml** s’ouvre et le  **&lt;meta&gt;**  balise est ajoutée automatiquement à la **head** section de la Fichier HTML.</span><span class="sxs-lookup"><span data-stu-id="19fea-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="19fea-316">![Balise META automatiquement ajoutée dans la page de _Layout](visual-studio-2013-web-tools/_static/image34.png "balise Meta automatiquement ajoutée dans la page de _Layout")</span><span class="sxs-lookup"><span data-stu-id="19fea-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="19fea-317">*Balise META automatiquement ajouté à \_page de disposition*</span><span class="sxs-lookup"><span data-stu-id="19fea-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="19fea-318">Modifiez la valeur de la **contenu** attribut *GeekQuiz* et enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="19fea-319">Exercice 2 : En tirant parti des extraits de Code IntelliSense</span><span class="sxs-lookup"><span data-stu-id="19fea-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="19fea-320">Avec Web Essentials, l’éditeur HTML a été étendue avec des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="19fea-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="19fea-321">Dans cet exercice, vous verrez certaines nouvelles fonctionnalités qui sont utiles lors du développement d’applications web.</span><span class="sxs-lookup"><span data-stu-id="19fea-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="19fea-322">Tâche 1 : utilisation d’IntelliSense dans des Documents HTML</span><span class="sxs-lookup"><span data-stu-id="19fea-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="19fea-323">La nouvelle fonctionnalité premier que vous voyez dans cette tâche est appelée **IntelliSense dynamique**.</span><span class="sxs-lookup"><span data-stu-id="19fea-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="19fea-324">IntelliSense dynamique lit les autres balises et les attributs de déduire les ID possibles, que vous allez utiliser.</span><span class="sxs-lookup"><span data-stu-id="19fea-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="19fea-325">Dans cette tâche, vous allez créer un nouvel élément de formulaire HTML qui contient une étiquette et un champ d’entrée.</span><span class="sxs-lookup"><span data-stu-id="19fea-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="19fea-326">Ensuite, vous allez ajouter un **pour** d’attribut de l’étiquette à lier à l’entrée, et vous verrez des suggestions IntelliSense basées sur les ID des entrées dans la portée.</span><span class="sxs-lookup"><span data-stu-id="19fea-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="19fea-327">Ouvrez **Visual Studio Express 2013 pour le Web** et **Begin.sln** solution situé dans le **début/TakingAdvantageofCodeSnippetsandIntelliSense-Ex2/Source** dossier.</span><span class="sxs-lookup"><span data-stu-id="19fea-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="19fea-328">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu dans l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="19fea-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="19fea-329">Dans **l’Explorateur de solutions**, ouvrez le **Index.cshtml** fichier situé dans le **vues** | **accueil** dossier.</span><span class="sxs-lookup"><span data-stu-id="19fea-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="19fea-330">Ajoutez un formulaire à l’intérieur de la  **&lt;section&gt;**  élément.</span><span class="sxs-lookup"><span data-stu-id="19fea-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="19fea-331">(Code d’extrait de code - *VisualStudio2013WebTooling* - *Ex2* - *formulaire*)</span><span class="sxs-lookup"><span data-stu-id="19fea-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="19fea-332">La balise d’entrée doit être précédée d’une étiquette avec une description du champ.</span><span class="sxs-lookup"><span data-stu-id="19fea-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="19fea-333">Ajoutez l’intitulé suivant avant la balise d’entrée.</span><span class="sxs-lookup"><span data-stu-id="19fea-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="19fea-334">Le **pour** attribut d’un  **&lt;étiquette&gt;**  indique à quel élément de formulaire une étiquette est liée.</span><span class="sxs-lookup"><span data-stu-id="19fea-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="19fea-335">La valeur d’attribut doit être égale à l’id de l’élément associé.</span><span class="sxs-lookup"><span data-stu-id="19fea-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="19fea-336">Ajouter le **pour** d’attribut pour le  **&lt;étiquette&gt;**  élément.</span><span class="sxs-lookup"><span data-stu-id="19fea-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="19fea-337">Comme indiqué dans l’illustration suivante, le &quot;nom&quot; valeur s’affiche dans la zone IntelliSense, en fonction de l’id des éléments dans la même portée (englobants  **&lt;formulaire&gt;**).</span><span class="sxs-lookup"><span data-stu-id="19fea-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="19fea-338">![Indiquer l’id dans IntelliSense](visual-studio-2013-web-tools/_static/image35.png "indiquant l’id dans IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="19fea-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="19fea-339">*Indiquer l’id dans IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="19fea-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="19fea-340">Supprimer la dernière ajoutée  **&lt;formulaire&gt;**  élément et son contenu.</span><span class="sxs-lookup"><span data-stu-id="19fea-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="19fea-341">Tâche 2 - extraits de Code HTML à l’aide de</span><span class="sxs-lookup"><span data-stu-id="19fea-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="19fea-342">HTML5 a introduit plus de 25 nouvelles balises sémantiques.</span><span class="sxs-lookup"><span data-stu-id="19fea-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="19fea-343">Visual Studio a déjà été prise en charge IntelliSense pour ces balises, mais Visual Studio 2013 est plus rapide et plus facile d’écrire le balisage en ajoutant de nouveaux extraits de code.</span><span class="sxs-lookup"><span data-stu-id="19fea-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="19fea-344">Bien que ces étiquettes ne sont pas compliquées, ils sont livrés avec quelques subtilités, telles que l’ajout les secours codec correct pour le *audio* balise.</span><span class="sxs-lookup"><span data-stu-id="19fea-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="19fea-345">Dans cette tâche, vous verrez les extraits de code HTML pour la balise audio.</span><span class="sxs-lookup"><span data-stu-id="19fea-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="19fea-346">Dans le **Index.cshtml** de fichier, tapez  **&lt;aud** à l’intérieur de la  **&lt;section&gt;**  élément, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="19fea-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="19fea-347">![Insertion d’un élément audio](visual-studio-2013-web-tools/_static/image36.png "insertion d’un élément audio")</span><span class="sxs-lookup"><span data-stu-id="19fea-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="19fea-348">*Insertion d’un élément audio*</span><span class="sxs-lookup"><span data-stu-id="19fea-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="19fea-349">Appuyez sur **onglet** deux fois et notez la façon dont le code suivant est ajouté dans la page et le curseur est placé sur le **src** attribut de la première source.</span><span class="sxs-lookup"><span data-stu-id="19fea-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="19fea-350">En appuyant sur la **onglet** clé à deux reprises, l’extrait de code est inséré.</span><span class="sxs-lookup"><span data-stu-id="19fea-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="19fea-351">L’extrait de code audio illustre l’utilisation standard de la *audio* balise, avec deux fichiers sources pour la prise en charge améliorée.</span><span class="sxs-lookup"><span data-stu-id="19fea-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="19fea-352">Supprimez la deuxième ligne et de mettre à jour de la source de la première ligne avec le lien suivant pour l’afficher WebCampsTV Katana : [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="19fea-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="19fea-353">Le code résultant est indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="19fea-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="19fea-354">Le fichier source *KatanaProject.mp3* est utilisé comme un exemple.</span><span class="sxs-lookup"><span data-stu-id="19fea-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="19fea-355">Vous pouvez utiliser une autre source si vous préférez.</span><span class="sxs-lookup"><span data-stu-id="19fea-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="19fea-356">Appuyez sur **CTRL** + **S** pour enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="19fea-357">Appuyez sur **CTRL** + **F5** pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="19fea-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="19fea-358">Notez qu’un lecteur a été ajouté à l’application.</span><span class="sxs-lookup"><span data-stu-id="19fea-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="19fea-359">![Lecteur audio dans Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player dans Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="19fea-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="19fea-360">*Lecteur audio dans Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="19fea-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="19fea-361">![Lecteur audio Google Chrome](visual-studio-2013-web-tools/_static/image38.png "lecteur Audio Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="19fea-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="19fea-362">*Lecteur audio Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="19fea-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="19fea-363">Ne fermez pas les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="19fea-363">Do not close the browsers.</span></span> <span data-ttu-id="19fea-364">Vous allez les utiliser dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="19fea-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="19fea-365">Tâche 3 : utilisation d’IntelliSense dans les Documents de JavaScript</span><span class="sxs-lookup"><span data-stu-id="19fea-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="19fea-366">Avec Web Essentials 2013, les feuilles de style et des pages HTML produisent une liste d’ID et noms de classe.</span><span class="sxs-lookup"><span data-stu-id="19fea-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="19fea-367">Dans cette tâche, vous allez apprendre comment ces listes améliorent la prise en charge de JavaScript IntelliSense dans Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="19fea-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="19fea-368">Dans le **Index.cshtml** , ajoutez le code suivant pour définir un **script** balise pour le code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="19fea-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="19fea-369">Ajoutez le code suivant à l’intérieur de la **script** balise pour définir la fonction de rappel prêt.</span><span class="sxs-lookup"><span data-stu-id="19fea-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="19fea-370">(Code d’extrait de code - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="19fea-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="19fea-371">Placer le signe insertion dans la **script** balise, puis appuyez sur **CTRL** + **.**</span><span class="sxs-lookup"><span data-stu-id="19fea-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="19fea-372">Pour ouvrir le menu de suggestion.</span><span class="sxs-lookup"><span data-stu-id="19fea-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="19fea-373">Cliquez sur **extraire vers le fichier**.</span><span class="sxs-lookup"><span data-stu-id="19fea-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="19fea-374">![JavaScript extraire vers suggestion du fichier](visual-studio-2013-web-tools/_static/image39.png "JavaScript extraire de suggestion de fichier")</span><span class="sxs-lookup"><span data-stu-id="19fea-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="19fea-375">*JavaScript extraire de suggestion de fichier*</span><span class="sxs-lookup"><span data-stu-id="19fea-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="19fea-376">Dans le **enregistrer en tant que** fenêtre, sélectionnez le **Scripts** dossier, nommez le fichier **init.js** et cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="19fea-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="19fea-377">![Enregistrer en tant que fenêtre](visual-studio-2013-web-tools/_static/image40.png "enregistrer en tant que fenêtre")</span><span class="sxs-lookup"><span data-stu-id="19fea-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="19fea-378">*Enregistrer en tant que fenêtre*</span><span class="sxs-lookup"><span data-stu-id="19fea-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-379">Le **init.js** fichier est créé et le contenu du script est déplacé vers le fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="19fea-380">![Fichier init.js créé avec le contenu inclus](visual-studio-2013-web-tools/_static/image41.png "fichier Init.js créé avec le contenu inclus")</span><span class="sxs-lookup"><span data-stu-id="19fea-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="19fea-381">*Fichier init.js créé avec le contenu inclus*</span><span class="sxs-lookup"><span data-stu-id="19fea-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="19fea-382">Ouvrez le **Index.cshtml** de fichier et vérifiez que la balise de script a été remplacée par une référence à la **init.js** fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="19fea-383">![Référence de html init.js](visual-studio-2013-web-tools/_static/image42.png "référence de html Init.js")</span><span class="sxs-lookup"><span data-stu-id="19fea-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="19fea-384">*Référence de html init.js*</span><span class="sxs-lookup"><span data-stu-id="19fea-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="19fea-385">Accédez à la **l’Explorateur de solutions** et notez que le **init.js** fichier a été automatiquement inclus dans la solution.</span><span class="sxs-lookup"><span data-stu-id="19fea-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="19fea-386">![Fichier init.js inclus dans la solution](visual-studio-2013-web-tools/_static/image43.png "fichier Init.js inclus dans la solution")</span><span class="sxs-lookup"><span data-stu-id="19fea-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="19fea-387">*Fichier init.js inclus dans la solution*</span><span class="sxs-lookup"><span data-stu-id="19fea-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="19fea-388">Revenez à la **init.js** fichier pour mettre à jour le **prêt** fonction de rappel.</span><span class="sxs-lookup"><span data-stu-id="19fea-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="19fea-389">À l’intérieur de la définition de rappel de fonction qui est passée à *prêt*, ajoutez le code suivant pour obtenir tous les éléments par un attribut de classe spécifique.</span><span class="sxs-lookup"><span data-stu-id="19fea-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="19fea-390">Appuyez sur **CTRL** + **espace** entre guillemets à l’intérieur de la **getElementsByClassName** l’appel de fonction.</span><span class="sxs-lookup"><span data-stu-id="19fea-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="19fea-391">![Affichage d’IntelliSense pour la fonction getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "affichage d’IntelliSense pour la fonction getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="19fea-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="19fea-392">*Affichage d’IntelliSense pour la fonction getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="19fea-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-393">Notez qu’IntelliSense affiche les classes définies dans les feuilles de style du projet.</span><span class="sxs-lookup"><span data-stu-id="19fea-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="19fea-394">Remplacez la ligne que vous avez créés avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="19fea-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="19fea-395">Placez le curseur après **au** à l’intérieur des guillemets dans les **getElementsByTagName** (fonction), puis appuyez sur **CTRL** + **espace**.</span><span class="sxs-lookup"><span data-stu-id="19fea-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="19fea-396">![Affichage d’IntelliSense pour la méthode getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "affichage d’IntelliSense pour la méthode getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="19fea-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="19fea-397">*Affichage d’IntelliSense pour la méthode getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="19fea-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="19fea-398">Sélectionnez  **&quot;audio&quot;**  à partir de la liste et appuyez sur **entrée**.</span><span class="sxs-lookup"><span data-stu-id="19fea-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="19fea-399">Le résultat est affiché dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="19fea-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="19fea-400">![Récupération des éléments Audio](visual-studio-2013-web-tools/_static/image46.png "la récupération des éléments Audio")</span><span class="sxs-lookup"><span data-stu-id="19fea-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="19fea-401">*Récupération des éléments Audio*</span><span class="sxs-lookup"><span data-stu-id="19fea-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="19fea-402">Dans **l’Explorateur de solutions**, avec le bouton droit le **init.js** de fichiers dans le **Scripts** et sélectionnez **ou les fichiers JavaScript de minimiser** à partir de la **Web Essentials** menu.</span><span class="sxs-lookup"><span data-stu-id="19fea-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="19fea-403">![Minimiser l’ou les fichiers JavaScript](visual-studio-2013-web-tools/_static/image47.png "fichiers minimiser de JavaScript")</span><span class="sxs-lookup"><span data-stu-id="19fea-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="19fea-404">*Minimiser l’ou les fichiers JavaScript*</span><span class="sxs-lookup"><span data-stu-id="19fea-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="19fea-405">Lorsque vous êtes invité à activer la réduction automatique lorsque les modifications de fichier source, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="19fea-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="19fea-406">![Activer l’avertissement de réduction automatique](visual-studio-2013-web-tools/_static/image48.png "l’activation d’avertissement de réduction automatique")</span><span class="sxs-lookup"><span data-stu-id="19fea-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="19fea-407">*Activer l’avertissement de réduction automatique*</span><span class="sxs-lookup"><span data-stu-id="19fea-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="19fea-408">Le **init.min.js** est créé et est ajouté en tant que dépendance de la **init.js** fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="19fea-409">![Fichier init.min.js créé](visual-studio-2013-web-tools/_static/image49.png "fichier Init.min.js créé")</span><span class="sxs-lookup"><span data-stu-id="19fea-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="19fea-410">*Fichier init.min.js créé*</span><span class="sxs-lookup"><span data-stu-id="19fea-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="19fea-411">Ouvrez le **init.min.js** de fichiers et notez que le fichier est réduite.</span><span class="sxs-lookup"><span data-stu-id="19fea-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="19fea-412">![Contenu du fichier init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js le contenu du fichier")</span><span class="sxs-lookup"><span data-stu-id="19fea-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="19fea-413">*Contenu du fichier init.min.js*</span><span class="sxs-lookup"><span data-stu-id="19fea-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="19fea-414">Dans le **init.js** , ajoutez le code suivant la **getElementsByTagName** lire tous les éléments audio de l’appel de fonction.</span><span class="sxs-lookup"><span data-stu-id="19fea-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="19fea-415">(Code d’extrait de code - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="19fea-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="19fea-416">Cliquez sur **CTRL** + **S** pour enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="19fea-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="19fea-417">Étant donné que le fichier réduite est déjà ouvert, vous verrez une boîte de dialogue indiquant que le fichier a été modifié en dehors de l’éditeur de code source.</span><span class="sxs-lookup"><span data-stu-id="19fea-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="19fea-418">Cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="19fea-418">Click **Yes**.</span></span>

    <span data-ttu-id="19fea-419">![Avertissement de Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "avertissement de Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="19fea-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="19fea-420">*Avertissement de Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="19fea-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="19fea-421">Revenez à la **init.min.js** fichier pour vérifier que le fichier a été mis à jour avec le nouveau code.</span><span class="sxs-lookup"><span data-stu-id="19fea-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="19fea-422">![Mise à jour du fichier init.min.js](visual-studio-2013-web-tools/_static/image52.png "fichier Init.min.js mis à jour")</span><span class="sxs-lookup"><span data-stu-id="19fea-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="19fea-423">*Fichier init.min.js mis à jour*</span><span class="sxs-lookup"><span data-stu-id="19fea-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="19fea-424">Cliquez sur le **Actualiser du navigateur lien** bouton.</span><span class="sxs-lookup"><span data-stu-id="19fea-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="19fea-425">Une fois que les deux navigateurs sont actualisées les lecteurs audio, que vous avez vu dans la tâche précédente lecture commence automatiquement.</span><span class="sxs-lookup"><span data-stu-id="19fea-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="19fea-426">![Lecteur audio inclus dans la vue](visual-studio-2013-web-tools/_static/image53.png "lecteur Audio inclus dans la vue")</span><span class="sxs-lookup"><span data-stu-id="19fea-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="19fea-427">*Lecteur audio inclus dans la vue*</span><span class="sxs-lookup"><span data-stu-id="19fea-427">*Audio player included in view*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="19fea-428">Résumé</span><span class="sxs-lookup"><span data-stu-id="19fea-428">Summary</span></span>

<span data-ttu-id="19fea-429">À la fin de cet atelier pratique, vous avez appris comment :</span><span class="sxs-lookup"><span data-stu-id="19fea-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="19fea-430">Utiliser les nouvelles fonctionnalités de l’éditeur HTML incluses dans Web Essentials, tels que des extraits de code HTML5 riches et la simplicité de codage</span><span class="sxs-lookup"><span data-stu-id="19fea-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="19fea-431">Utiliser les nouvelles fonctionnalités de l’éditeur CSS incluses dans Web Essentials, tels que le sélecteur de couleurs et une info-bulle de la matrice navigateur</span><span class="sxs-lookup"><span data-stu-id="19fea-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="19fea-432">Utiliser les nouvelles fonctionnalités de l’éditeur JavaScript incluses dans Essentials Web tels que d’extraire vers un fichier et IntelliSense pour tous les éléments HTML</span><span class="sxs-lookup"><span data-stu-id="19fea-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="19fea-433">Échanger des données entre votre navigateur et de Visual Studio à l’aide du lien du navigateur</span><span class="sxs-lookup"><span data-stu-id="19fea-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
