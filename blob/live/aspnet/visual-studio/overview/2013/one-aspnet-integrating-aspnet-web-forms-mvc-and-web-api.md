---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "Ateliers pratiques : One ASP.NET : intégration ASP.NET Web Forms, MVC et API Web | Documents Microsoft"
author: rick-anderson
description: "ASP.NET est une infrastructure pour la création de sites Web, applications et services à l’aide de technologies spécialisées, telles que MVC, API Web et d’autres. À l’expansion de ASP.NET h..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="e2c52-104">Ateliers pratiques : One ASP.NET : intégration ASP.NET Web Forms, MVC et API Web</span><span class="sxs-lookup"><span data-stu-id="e2c52-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="e2c52-105">par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e2c52-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e2c52-106">Télécharger Camps Web Kit de formation</span><span class="sxs-lookup"><span data-stu-id="e2c52-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="e2c52-107">ASP.NET est une infrastructure pour la création de sites Web, applications et services à l’aide de technologies spécialisées, telles que MVC, API Web et d’autres.</span><span class="sxs-lookup"><span data-stu-id="e2c52-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="e2c52-108">À l’expansion de ASP.NET a constaté depuis sa création et devez l’avoir ces technologies intégrées, des efforts récentes ont été dans le travail vers **One ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="e2c52-109">Visual Studio 2013 introduit un nouveau système de projet unifiée qui vous permet de créer une application et utiliser toutes les technologies ASP.NET dans un seul projet.</span><span class="sxs-lookup"><span data-stu-id="e2c52-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="e2c52-110">Cette fonctionnalité élimine le besoin de choisir une technologie au début d’un projet et le stick avec lui et à la place encourage l’utilisation de plusieurs infrastructures d’ASP.NET au sein d’un projet.</span><span class="sxs-lookup"><span data-stu-id="e2c52-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="e2c52-111">Tous les exemples de code et des extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e2c52-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="e2c52-112">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e2c52-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e2c52-113">Objectifs</span><span class="sxs-lookup"><span data-stu-id="e2c52-113">Objectives</span></span>

<span data-ttu-id="e2c52-114">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="e2c52-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="e2c52-115">Créer un site Web basé sur le **One ASP.NET** type de projet</span><span class="sxs-lookup"><span data-stu-id="e2c52-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="e2c52-116">Utilisez différents **ASP.NET** comme **MVC** et **API Web** dans le même projet</span><span class="sxs-lookup"><span data-stu-id="e2c52-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="e2c52-117">Identifier les principaux composants d’un **ASP.NET** application</span><span class="sxs-lookup"><span data-stu-id="e2c52-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="e2c52-118">Tirer parti de la **génération de modèles automatique ASP.NET** framework pour créer automatiquement les contrôleurs et les vues pour exécuter des opérations CRUD basé sur vos classes de modèle</span><span class="sxs-lookup"><span data-stu-id="e2c52-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="e2c52-119">Exposer le même ensemble d’informations dans des formats machine - et explicite à l’aide de l’outil approprié pour chaque tâche</span><span class="sxs-lookup"><span data-stu-id="e2c52-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e2c52-120">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="e2c52-120">Prerequisites</span></span>

<span data-ttu-id="e2c52-121">Les éléments suivants sont nécessaire pour terminer cet atelier pratique :</span><span class="sxs-lookup"><span data-stu-id="e2c52-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="e2c52-122">[Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/) ou supérieur</span><span class="sxs-lookup"><span data-stu-id="e2c52-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="e2c52-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="e2c52-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e2c52-124">Installation</span><span class="sxs-lookup"><span data-stu-id="e2c52-124">Setup</span></span>

<span data-ttu-id="e2c52-125">Pour exécuter les exercices de cet atelier, vous devez configurer votre environnement tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="e2c52-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="e2c52-126">Ouvrez l’Explorateur Windows et accédez à de l’atelier **Source** dossier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="e2c52-127">Avec le bouton droit sur **Setup.cmd** et sélectionnez **exécuter en tant qu’administrateur** pour lancer le processus d’installation qui sera configurer votre environnement et installer les extraits de code Visual Studio pour ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="e2c52-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="e2c52-128">Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, confirmez l’action pour continuer.</span><span class="sxs-lookup"><span data-stu-id="e2c52-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c52-129">Assurez-vous que vous avez activé toutes les dépendances pour ce laboratoire avant d’exécuter le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="e2c52-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="e2c52-130">À l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="e2c52-130">Using the Code Snippets</span></span>

<span data-ttu-id="e2c52-131">Dans le document de laboratoire, vous serez invité à insérer des blocs de code.</span><span class="sxs-lookup"><span data-stu-id="e2c52-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="e2c52-132">Pour des raisons pratiques, la majeure partie de ce code est fourni en tant que Visual Studio extraits de Code, vous pouvez accéder à partir de Visual Studio 2013 pour éviter d’avoir à ajouter manuellement.</span><span class="sxs-lookup"><span data-stu-id="e2c52-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c52-133">Chaque exercice est accompagnée d’une solution de départ située dans le **commencer** dossier de l’exercice qui vous permet de suivre chaque exercice indépendamment des autres.</span><span class="sxs-lookup"><span data-stu-id="e2c52-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="e2c52-134">Sachez que les extraits de code sont ajoutés au cours d’un exercice sont manquants à partir de ces solutions de démarrage et peut ne pas fonctionneront tant que vous avez terminé l’exercice.</span><span class="sxs-lookup"><span data-stu-id="e2c52-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="e2c52-135">Dans le code source pour un exercice, vous trouverez également une **fin** dossier qui contient une solution Visual Studio avec le code qui résulte de la procédure dans l’exercice correspondant.</span><span class="sxs-lookup"><span data-stu-id="e2c52-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="e2c52-136">Si vous avez besoin d’aide au cours de cet atelier, vous pouvez utiliser ces solutions comme guide.</span><span class="sxs-lookup"><span data-stu-id="e2c52-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e2c52-137">Exercices</span><span class="sxs-lookup"><span data-stu-id="e2c52-137">Exercises</span></span>

<span data-ttu-id="e2c52-138">Cet atelier pratique inclut les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="e2c52-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="e2c52-139">Création d’un projet Web Forms</span><span class="sxs-lookup"><span data-stu-id="e2c52-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="e2c52-140">Création d’un contrôleur MVC à l’aide de la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="e2c52-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="e2c52-141">Création d’un contrôleur d’API Web à l’aide de la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="e2c52-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="e2c52-142">Durée estimée pour effectuer ce laboratoire : **60 minutes**</span><span class="sxs-lookup"><span data-stu-id="e2c52-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="e2c52-143">Lorsque vous démarrez Visual Studio, vous devez sélectionner une des collections de paramètres prédéfinis.</span><span class="sxs-lookup"><span data-stu-id="e2c52-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="e2c52-144">Chaque collection prédéfinie est conçue pour faire correspondre un style de développement particulier et détermine les dispositions de fenêtres, le comportement de l’éditeur, extraits de code IntelliSense et les options de boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="e2c52-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="e2c52-145">Les procédures de cet atelier décrivent les actions nécessaires pour accomplir une tâche donnée dans Visual Studio lorsque vous utilisez la **paramètres de développement généraux** collection.</span><span class="sxs-lookup"><span data-stu-id="e2c52-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="e2c52-146">Si vous choisissez une collection de paramètres différents pour votre environnement de développement, il est possible les différences dans les étapes que vous devez prendre en compte.</span><span class="sxs-lookup"><span data-stu-id="e2c52-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="e2c52-147">Exercice 1 : Création d’un projet Web Forms</span><span class="sxs-lookup"><span data-stu-id="e2c52-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="e2c52-148">Dans cet exercice, vous allez créer un nouveau site Web Forms dans Visual Studio 2013 à l’aide de la **One ASP.NET** unifiée expérience de projet, ce qui vous permet de s’intégrer facilement des composants Web Forms, MVC et API Web dans la même application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="e2c52-149">Vous allez ensuite Explorer la solution générée et identifier ses parties, et enfin, vous verrez le site Web dans l’action.</span><span class="sxs-lookup"><span data-stu-id="e2c52-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="e2c52-150">Tâche 1 : création d’un Site à l’aide de l’une expérience d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e2c52-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="e2c52-151">Dans cette tâche, vous allez démarrer la création d’un nouveau site Web dans Visual Studio basé sur le **One ASP.NET** type de projet.</span><span class="sxs-lookup"><span data-stu-id="e2c52-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="e2c52-152">**Un ASP.NET** unifie toutes les technologies ASP.NET et vous permet de combiner et de les mettre en correspondance comme vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="e2c52-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="e2c52-153">Puis facile à reconnaître les différents composants de Web Forms, MVC et API Web live côte à côte dans votre application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="e2c52-154">Ouvrez **Visual Studio Express 2013 pour le Web** et sélectionnez **fichier | Nouveau projet...**  pour démarrer une nouvelle solution.</span><span class="sxs-lookup"><span data-stu-id="e2c52-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Création d'un projet](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="e2c52-156">*Création d’un projet*</span><span class="sxs-lookup"><span data-stu-id="e2c52-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="e2c52-157">Dans le **nouveau projet** boîte de dialogue, sélectionnez **Application Web ASP.NET** sous le **Visual C# | Web** onglet et vérifiez que **.NET Framework 4.5** est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="e2c52-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="e2c52-158">Nommez le projet *MyHybridSite*, choisissez un **emplacement** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Nouveau projet d’Application Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="e2c52-160">*Création d’un projet d’Application Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="e2c52-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="e2c52-161">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **Web Forms** modèle et sélectionnez le **MVC** et **API Web** options.</span><span class="sxs-lookup"><span data-stu-id="e2c52-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="e2c52-162">En outre, assurez-vous que le **authentification** option est définie sur **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="e2c52-163">Cliquez sur **OK** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="e2c52-163">Click **OK** to continue.</span></span>

    ![Création d’un projet avec le modèle Web Forms, y compris les composants Web API et MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="e2c52-165">*Création d’un projet avec le modèle Web Forms, y compris les composants Web API et MVC*</span><span class="sxs-lookup"><span data-stu-id="e2c52-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="e2c52-166">Vous pouvez maintenant Explorer la structure de la solution générée.</span><span class="sxs-lookup"><span data-stu-id="e2c52-166">You can now explore the structure of the generated solution.</span></span>

    ![Exploration de la solution générée](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="e2c52-168">*Exploration de la solution générée*</span><span class="sxs-lookup"><span data-stu-id="e2c52-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="e2c52-169">**Compte :** ce dossier contient les pages de formulaire Web pour vous inscrire, connectez-vous à et gérer les comptes d’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="e2c52-170">Ce dossier est ajouté quand le **comptes d’utilisateur individuels** option d’authentification est sélectionnée lors de la configuration du modèle de projet Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e2c52-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="e2c52-171">**Modèles :** ce dossier contient les classes qui représentent les données de vos applications.</span><span class="sxs-lookup"><span data-stu-id="e2c52-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="e2c52-172">**Contrôleurs** et **vues**: ces dossiers sont requis pour le **ASP.NET MVC** et **API Web ASP.NET** composants.</span><span class="sxs-lookup"><span data-stu-id="e2c52-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="e2c52-173">Vous allez explorer les technologies MVC et les API Web dans les exercices suivants.</span><span class="sxs-lookup"><span data-stu-id="e2c52-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="e2c52-174">Le **Default.aspx**, **Contact.aspx** et **About.aspx** fichiers sont des pages Web Form prédéfinies que vous pouvez utiliser comme points de départ pour générer les pages spécifiques à votre application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="e2c52-175">La logique de programmation de ces fichiers se trouve dans un fichier distinct appelé le &quot;code-behind&quot; fichier, qui a un &quot;. aspx.vb&quot; ou &quot;. aspx.cs&quot; extension (en fonction de la langage utilisé).</span><span class="sxs-lookup"><span data-stu-id="e2c52-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="e2c52-176">La logique de code-behind s’exécute sur le serveur et génère dynamiquement la sortie HTML de votre page.</span><span class="sxs-lookup"><span data-stu-id="e2c52-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="e2c52-177">Le **Site.Master** et **Site.Mobile.Master** pages définissent l’apparence et le comportement standard de toutes les pages dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="e2c52-178">Double-cliquez sur le **Default.aspx** fichier pour Explorer le contenu de la page.</span><span class="sxs-lookup"><span data-stu-id="e2c52-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Exploration de la page Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="e2c52-180">*Exploration de la page Default.aspx*</span><span class="sxs-lookup"><span data-stu-id="e2c52-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2c52-181">Le **Page** directive en haut du fichier définit les attributs de la page Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e2c52-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="e2c52-182">Par exemple, le **MasterPageFile** attribut spécifie le chemin d’accès au contrôleur de page - dans ce cas, le *Site.Master* page - et **Inherits** attribut définit le classe code-behind pour la page à hériter.</span><span class="sxs-lookup"><span data-stu-id="e2c52-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="e2c52-183">Cette classe se trouve dans le fichier déterminé par le **code-behind** attribut.</span><span class="sxs-lookup"><span data-stu-id="e2c52-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="e2c52-184">Le **asp : Content** contrôle contient le contenu réel de la page (texte, balisage et contrôles) et est mappé à un **asp : ContentPlaceHolder** contrôle sur la page maître.</span><span class="sxs-lookup"><span data-stu-id="e2c52-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="e2c52-185">Dans ce cas, le contenu de la page est rendu dans le *MainContent* contrôle défini dans le *Site.Master* page.</span><span class="sxs-lookup"><span data-stu-id="e2c52-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="e2c52-186">Développez le **application\_Démarrer** dossier et notez la **WebApiConfig.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e2c52-187">Visual Studio inclus ce fichier dans la solution générée, car vous avez inclus les API Web lors de la configuration de votre projet avec le modèle One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e2c52-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="e2c52-188">Ouvrez le **WebApiConfig.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e2c52-189">Dans le *WebApiConfig* classe, vous trouverez la configuration associée aux API Web, qui mappe HTTP achemine vers **contrôleurs de l’API Web**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="e2c52-190">Ouvrez le **RouteConfig.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="e2c52-191">À l’intérieur de la *RegisterRoutes* méthode vous trouverez la configuration associée MVC, qui mappe les itinéraires HTTP à **contrôleurs MVC**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e2c52-192">Tâche 2 : exécution de la Solution</span><span class="sxs-lookup"><span data-stu-id="e2c52-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e2c52-193">Dans cette tâche, vous allez exécuter la solution générée, explorez l’application et certaines de ses fonctionnalités, telles que la réécriture d’URL et l’authentification intégrée.</span><span class="sxs-lookup"><span data-stu-id="e2c52-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="e2c52-194">Pour exécuter la solution, appuyez sur **F5** ou cliquez sur le **Démarrer** bouton situé sur la barre d’outils.</span><span class="sxs-lookup"><span data-stu-id="e2c52-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="e2c52-195">Page d’accueil de l’application doit ouvrir dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e2c52-195">The application home page should open in the browser.</span></span>

    ![Exécution de la solution](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="e2c52-197">Vérifiez que les pages Web Forms sont appelées.</span><span class="sxs-lookup"><span data-stu-id="e2c52-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="e2c52-198">Pour ce faire, ajoutez **/contact.aspx** à l’URL dans la barre d’adresses et appuyez sur **entrée**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![URL conviviales](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="e2c52-200">*URL conviviales*</span><span class="sxs-lookup"><span data-stu-id="e2c52-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2c52-201">Comme vous pouvez le voir, l’URL devient **/contactez**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="e2c52-202">À partir **ASP.NET 4**, les fonctionnalités de routage d’URL ont été ajoutées aux Web Forms, afin d’écrire des URL comme  *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)*  au lieu de  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="e2c52-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="e2c52-203">Pour plus d’informations, reportez-vous à [routage d’URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="e2c52-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="e2c52-204">Vous allez maintenant découvrir le flux d’authentification intégré dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="e2c52-205">Pour ce faire, cliquez sur **enregistrer** dans le coin supérieur droit de la page.</span><span class="sxs-lookup"><span data-stu-id="e2c52-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![L’inscription d’un nouvel utilisateur](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="e2c52-207">*L’inscription d’un nouvel utilisateur*</span><span class="sxs-lookup"><span data-stu-id="e2c52-207">*Registering a new user*</span></span>
4. <span data-ttu-id="e2c52-208">Dans le **inscrire** , entrez un **nom d’utilisateur** et **mot de passe**, puis cliquez sur **inscrire**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Page d’inscription](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="e2c52-210">*Page d’inscription*</span><span class="sxs-lookup"><span data-stu-id="e2c52-210">*Register page*</span></span>
5. <span data-ttu-id="e2c52-211">L’application enregistre le nouveau compte, et l’utilisateur est authentifié.</span><span class="sxs-lookup"><span data-stu-id="e2c52-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Utilisateur authentifié](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="e2c52-213">*Utilisateur authentifié*</span><span class="sxs-lookup"><span data-stu-id="e2c52-213">*User authenticated*</span></span>
6. <span data-ttu-id="e2c52-214">Revenez à Visual Studio, puis appuyez sur **MAJ + F5** pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="e2c52-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="e2c52-215">Exercice 2 : Création d’un contrôleur MVC à l’aide de la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="e2c52-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="e2c52-216">Dans cet exercice vous tirera parti de l’infrastructure de génération de modèles automatique ASP.NET fourni par Visual Studio pour créer un contrôleur ASP.NET MVC 5 avec des actions et vues Razor permettant d’effectuer des opérations CRUD, sans écrire une seule ligne de code.</span><span class="sxs-lookup"><span data-stu-id="e2c52-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="e2c52-217">Le processus de génération de modèles automatique utilise Entity Framework Code First pour générer le contexte de données et le schéma de base de données dans la base de données SQL.</span><span class="sxs-lookup"><span data-stu-id="e2c52-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="e2c52-218">**À propos d’Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="e2c52-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="e2c52-219">Entity Framework (EF) est un mappeur objet / relationnel (ORM) qui vous permet de créer des applications d’accès aux données par programmation avec un modèle d’application conceptuel plutôt que directement à l’aide d’un schéma de stockage relationnel.</span><span class="sxs-lookup"><span data-stu-id="e2c52-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="e2c52-220">Le flux de travail de modélisation Entity Framework Code First vous permet d’utiliser vos propres classes de domaine pour représenter le modèle EF s’appuie sur lors de l’exécution de requêtes, les fonctions de suivi des modifications et mise à jour.</span><span class="sxs-lookup"><span data-stu-id="e2c52-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="e2c52-221">Utilisant le workflow de développement Code First, vous n’avez pas besoin commencer votre application en créant une base de données ou de spécification d’un schéma.</span><span class="sxs-lookup"><span data-stu-id="e2c52-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="e2c52-222">Au lieu de cela, vous pouvez écrire des classes .NET standard qui définissent les objets de modèle de domaine plus appropriés pour votre application, et Entity Framework crée la base de données pour vous.</span><span class="sxs-lookup"><span data-stu-id="e2c52-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c52-223">Plus d’informations sur Entity Framework [ici](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="e2c52-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="e2c52-224">Tâche 1 : création d’un modèle</span><span class="sxs-lookup"><span data-stu-id="e2c52-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="e2c52-225">Vous allez maintenant définir un **personne** (classe), qui sera le modèle utilisé par le processus de génération de modèles automatique pour créer le contrôleur MVC et les vues.</span><span class="sxs-lookup"><span data-stu-id="e2c52-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="e2c52-226">Vous allez commencer par créer un **personne** classe de modèle et les opérations CRUD dans le contrôleur seront automatiquement créé à l’aide des fonctionnalités de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="e2c52-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="e2c52-227">Ouvrez **Visual Studio Express 2013 pour le Web** et le **MyHybridSite.sln** solution situé dans le **Source/Ex2-MvcScaffolding/début** dossier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="e2c52-228">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu dans l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="e2c52-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="e2c52-229">Dans **l’Explorateur de solutions**, avec le bouton droit le **modèles** dossier de la **MyHybridSite** de projet et sélectionnez **ajouter | Classe...** .</span><span class="sxs-lookup"><span data-stu-id="e2c52-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Ajout de la classe de modèle personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="e2c52-231">*Ajout de la classe de modèle personne*</span><span class="sxs-lookup"><span data-stu-id="e2c52-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="e2c52-232">Dans le **ajouter un nouvel élément** boîte de dialogue, nommez le fichier *Person.cs* et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Création de la classe de modèle personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="e2c52-234">*Création de la classe de modèle personne*</span><span class="sxs-lookup"><span data-stu-id="e2c52-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="e2c52-235">Remplacez le contenu de la **Person.cs** fichier avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="e2c52-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="e2c52-236">Appuyez sur **CTRL + S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="e2c52-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="e2c52-237">(Code d’extrait de code - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="e2c52-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="e2c52-238">Dans **l’Explorateur de solutions**, avec le bouton droit le **MyHybridSite** de projet et sélectionnez **générer**, ou appuyez sur **CTRL + MAJ + B** pour générer le projet.</span><span class="sxs-lookup"><span data-stu-id="e2c52-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="e2c52-239">Tâche 2 : création d’un contrôleur MVC</span><span class="sxs-lookup"><span data-stu-id="e2c52-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="e2c52-240">Maintenant que le **personne** modèle est créé, vous allez utiliser la génération de modèles automatique ASP.NET MVC avec Entity Framework pour créer les actions de contrôleur CRUD et les vues pour **personne**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="e2c52-241">Dans **l’Explorateur de solutions**, avec le bouton droit le **contrôleurs** dossier de la **MyHybridSite** de projet et sélectionnez **ajouter | Nouvel élément structuré...** .</span><span class="sxs-lookup"><span data-stu-id="e2c52-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Création d’un nouveau contrôleur de modèle généré automatiquement](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="e2c52-243">*Création d’un nouveau contrôleur structuré*</span><span class="sxs-lookup"><span data-stu-id="e2c52-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="e2c52-244">Dans le **ajouter une vue de structure** boîte de dialogue, sélectionnez **contrôleur MVC 5 avec vues, utilisant Entity Framework** puis cliquez sur **ajouter.**</span><span class="sxs-lookup"><span data-stu-id="e2c52-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Sélection du contrôleur MVC 5 avec des vues et Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="e2c52-246">*Sélection du contrôleur MVC 5 avec des vues et Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="e2c52-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="e2c52-247">Définir *MvcPersonController* en tant que le **nom du contrôleur**, sélectionnez le **utiliser les actions de contrôleur asynchrones** option et sélectionnez **personne (MyHybridSite.Models)**  comme le **classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Ajout d’un contrôleur MVC avec la génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="e2c52-249">*Ajout d’un contrôleur MVC avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="e2c52-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="e2c52-250">Sous **classe de contexte de données**, cliquez sur **nouveau contexte de données...** .</span><span class="sxs-lookup"><span data-stu-id="e2c52-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Création d’un nouveau contexte de données](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="e2c52-252">*Création d’un nouveau contexte de données*</span><span class="sxs-lookup"><span data-stu-id="e2c52-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="e2c52-253">Dans le **nouveau contexte de données** boîte de dialogue, le nom du nouveau contexte de données *PersonContext* et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Création de la nouvelle PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="e2c52-255">*Création du nouveau type de PersonContext*</span><span class="sxs-lookup"><span data-stu-id="e2c52-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="e2c52-256">Cliquez sur **ajouter** pour créer le nouveau contrôleur de **personne** avec la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="e2c52-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="e2c52-257">Visual Studio génère ensuite les actions de contrôleur, le contexte de données de personne et les vues Razor.</span><span class="sxs-lookup"><span data-stu-id="e2c52-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Après avoir créé le contrôleur MVC avec la génération de modèles automatique](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="e2c52-259">*Après avoir créé le contrôleur MVC avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="e2c52-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="e2c52-260">Ouvrez le **MvcPersonController.cs** de fichiers dans le **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="e2c52-261">Notez que les méthodes d’action CRUD ont été générées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="e2c52-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="e2c52-262">En sélectionnant le **utiliser les actions de contrôleur asynchrones** options de case à cocher de la génération de modèles automatique dans les étapes précédentes, Visual Studio génère des méthodes d’action asynchrones pour toutes les actions qui impliquent l’accès au contexte de données Person.</span><span class="sxs-lookup"><span data-stu-id="e2c52-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="e2c52-263">Il est recommandé que vous utilisez des méthodes d’action asynchrones pour longue, UC non lié demandes pour éviter de bloquer le serveur Web de réaliser un travail pendant le traitement de la demande.</span><span class="sxs-lookup"><span data-stu-id="e2c52-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="e2c52-264">Tâche 3 : exécution de la Solution</span><span class="sxs-lookup"><span data-stu-id="e2c52-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="e2c52-265">Dans cette tâche, vous allez exécuter la solution pour vous assurer que les vues pour **personne** fonctionnent comme prévu.</span><span class="sxs-lookup"><span data-stu-id="e2c52-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="e2c52-266">Vous allez ajouter une nouvelle personne pour vérifier qu’il est correctement enregistré dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="e2c52-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="e2c52-267">Appuyez sur **F5** pour exécuter la solution.</span><span class="sxs-lookup"><span data-stu-id="e2c52-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="e2c52-268">Accédez à **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="e2c52-269">La vue de modèle généré automatiquement qui présente la liste des personnes doit apparaître.</span><span class="sxs-lookup"><span data-stu-id="e2c52-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="e2c52-270">Cliquez sur **créer un nouveau** pour ajouter une nouvelle personne.</span><span class="sxs-lookup"><span data-stu-id="e2c52-270">Click **Create New** to add a new person.</span></span>

    ![Accéder aux vues MVC modèle généré automatiquement](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="e2c52-272">*Accéder aux vues MVC modèle généré automatiquement*</span><span class="sxs-lookup"><span data-stu-id="e2c52-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="e2c52-273">Dans le **créer** afficher, fournissez un **nom** et un **âge** pour la personne, puis cliquez sur **créer**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Ajout d’une nouvelle personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="e2c52-275">*Ajout d’une nouvelle personne*</span><span class="sxs-lookup"><span data-stu-id="e2c52-275">*Adding a new person*</span></span>
5. <span data-ttu-id="e2c52-276">La nouvelle personne est ajoutée à la liste.</span><span class="sxs-lookup"><span data-stu-id="e2c52-276">The new person is added to the list.</span></span> <span data-ttu-id="e2c52-277">Dans la liste d’éléments, cliquez sur **détails** pour afficher les détails de la personne.</span><span class="sxs-lookup"><span data-stu-id="e2c52-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="e2c52-278">Puis, dans le **détails** afficher, cliquez sur **retour à la liste** pour revenir à la vue liste.</span><span class="sxs-lookup"><span data-stu-id="e2c52-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Afficher les détails de la personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="e2c52-280">*Afficher les détails de la personne*</span><span class="sxs-lookup"><span data-stu-id="e2c52-280">*Person's details view*</span></span>
6. <span data-ttu-id="e2c52-281">Cliquez sur le **supprimer** lien à supprimer de la personne.</span><span class="sxs-lookup"><span data-stu-id="e2c52-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="e2c52-282">Dans le **supprimer** afficher, cliquez sur **supprimer** à confirmer l’opération.</span><span class="sxs-lookup"><span data-stu-id="e2c52-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![La suppression d’une personne](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="e2c52-284">*La suppression d’une personne*</span><span class="sxs-lookup"><span data-stu-id="e2c52-284">*Deleting a person*</span></span>
7. <span data-ttu-id="e2c52-285">Revenez à Visual Studio, puis appuyez sur **MAJ + F5** pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="e2c52-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="e2c52-286">Exercice 3 : Création d’un contrôleur d’API Web à l’aide de la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="e2c52-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="e2c52-287">L’infrastructure API Web fait partie de la pile ASP.NET et conçu pour faciliter la mise en œuvre des services HTTP, généralement envoyer et recevoir des données JSON ou XML via une API RESTful.</span><span class="sxs-lookup"><span data-stu-id="e2c52-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="e2c52-288">Dans cet exercice, vous utiliserez la structure de ASP.NET à nouveau pour générer un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="e2c52-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="e2c52-289">Vous allez utiliser le même **personne** et **PersonContext** classes à partir de l’exercice précédent pour fournir les mêmes données person au format JSON.</span><span class="sxs-lookup"><span data-stu-id="e2c52-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="e2c52-290">Vous verrez comment vous pouvez exposer les mêmes ressources de plusieurs manières dans l’application ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e2c52-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="e2c52-291">Tâche 1 : création d’un contrôleur d’API Web</span><span class="sxs-lookup"><span data-stu-id="e2c52-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="e2c52-292">Dans cette tâche, vous allez créer un nouveau **contrôleur d’API Web** qui expose les données de la personne dans un format utilisables par des ordinateurs tels que JSON.</span><span class="sxs-lookup"><span data-stu-id="e2c52-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="e2c52-293">Si pas déjà ouvert, ouvrez **Visual Studio Express 2013 pour le Web** et ouvrez le **MyHybridSite.sln** solution situé dans le **début/WebAPI-Ex3/Source** dossier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="e2c52-294">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu dans l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="e2c52-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2c52-295">Si vous commencez avec la solution de début à partir de l’exercice 3, appuyez sur **CTRL + MAJ + B** pour générer la solution.</span><span class="sxs-lookup"><span data-stu-id="e2c52-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="e2c52-296">Dans **l’Explorateur de solutions**, avec le bouton droit le **contrôleurs** dossier de la **MyHybridSite** de projet et sélectionnez **ajouter | Nouvel élément structuré...** .</span><span class="sxs-lookup"><span data-stu-id="e2c52-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Création d’un nouveau contrôleur de modèle généré automatiquement](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="e2c52-298">*Création d’un nouveau contrôleur de modèle généré automatiquement*</span><span class="sxs-lookup"><span data-stu-id="e2c52-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="e2c52-299">Dans le **ajouter une vue de structure** boîte de dialogue, sélectionnez **API Web** dans le volet gauche, puis **Web API 2 contrôleur avec actions, utilisant Entity Framework** dans le volet central, puis  **Ajouter.**</span><span class="sxs-lookup"><span data-stu-id="e2c52-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="e2c52-300">![Sélection du contrôleur de Web API 2 avec Entity Framework et les actions](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "en sélectionnant des contrôleur Web API 2 avec Entity Framework et les actions")</span><span class="sxs-lookup"><span data-stu-id="e2c52-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="e2c52-301">*Sélection du contrôleur de Web API 2 avec Entity Framework et les actions*</span><span class="sxs-lookup"><span data-stu-id="e2c52-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="e2c52-302">Définir *ApiPersonController* en tant que le **nom du contrôleur**, sélectionnez le **utiliser les actions de contrôleur asynchrones** option et sélectionnez **personne (MyHybridSite.Models)**  et **PersonContext (MyHybridSite.Models)** comme le **modèle** et **contexte de données** classes respectivement.</span><span class="sxs-lookup"><span data-stu-id="e2c52-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="e2c52-303">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-303">Then click **Add**.</span></span>

    <span data-ttu-id="e2c52-304">![Ajout d’un contrôleur d’API Web avec la structure](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Ajout d’un contrôleur d’API Web avec la génération de modèles automatique")</span><span class="sxs-lookup"><span data-stu-id="e2c52-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e2c52-305">*Ajout d’un contrôleur d’API Web avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="e2c52-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="e2c52-306">Visual Studio génère ensuite le **ApiPersonController** classe avec les quatre actions CRUD pour travailler avec vos données.</span><span class="sxs-lookup"><span data-stu-id="e2c52-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="e2c52-307">![Après avoir créé le contrôleur d’API Web avec la structure](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "après avoir créé le contrôleur d’API Web avec la génération de modèles automatique")</span><span class="sxs-lookup"><span data-stu-id="e2c52-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e2c52-308">*Après avoir créé le contrôleur d’API Web avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="e2c52-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="e2c52-309">Ouvrez le **ApiPersonController.cs** de fichiers et d’inspecter le *GetPeople* méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="e2c52-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="e2c52-310">Cette méthode interroge le champ de base de données de **PersonContext** type afin d’obtenir des données de personnes.</span><span class="sxs-lookup"><span data-stu-id="e2c52-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="e2c52-311">Vous remarquez que le commentaire au-dessus de la définition de méthode.</span><span class="sxs-lookup"><span data-stu-id="e2c52-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="e2c52-312">Il fournit l’URI qui expose cette action que vous utiliserez dans la tâche suivante.</span><span class="sxs-lookup"><span data-stu-id="e2c52-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="e2c52-313">Par défaut, les API Web est configuré pour intercepter les requêtes pour le */API* chemin d’accès pour éviter les conflits avec les contrôleurs MVC.</span><span class="sxs-lookup"><span data-stu-id="e2c52-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="e2c52-314">Si vous devez modifier ce paramètre, reportez-vous à [le routage ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e2c52-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e2c52-315">Tâche 2 : exécution de la Solution</span><span class="sxs-lookup"><span data-stu-id="e2c52-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e2c52-316">Dans cette tâche, vous allez utiliser Internet Explorer **outils de développement F12** pour inspecter la réponse complète à partir du contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="e2c52-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="e2c52-317">Vous verrez comment vous pouvez capturer le trafic réseau pour obtenir plus de détails sur vos données d’application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="e2c52-318">Assurez-vous que **Internet Explorer** est sélectionné dans le **Démarrer** bouton situé sur la barre d’outils de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2c52-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Option d’Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="e2c52-320">Le **outils de développement F12** ont un large éventail de fonctionnalités qui ne sont pas couverte dans ce exercices laboratoire.</span><span class="sxs-lookup"><span data-stu-id="e2c52-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="e2c52-321">Si vous souhaitez plus d’informations, reportez-vous à [utilisant les outils de développement F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="e2c52-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="e2c52-322">Appuyez sur **F5** pour exécuter la solution.</span><span class="sxs-lookup"><span data-stu-id="e2c52-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2c52-323">Pour effectuer cette tâche correctement, votre application doit avoir des données.</span><span class="sxs-lookup"><span data-stu-id="e2c52-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="e2c52-324">Si votre base de données est vide, vous pouvez revenir en arrière à la tâche 3 dans l’exercice 2 et suivez les étapes de la création d’une nouvelle personne à l’aide de vues MVC.</span><span class="sxs-lookup"><span data-stu-id="e2c52-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="e2c52-325">Dans le navigateur, appuyez sur **F12** pour ouvrir le **outils de développement** Panneau de configuration.</span><span class="sxs-lookup"><span data-stu-id="e2c52-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="e2c52-326">Appuyez sur **CTRL** + **4** ou cliquez sur le **réseau** icône, puis cliquez sur la flèche verte pour commencer la capture du trafic réseau.</span><span class="sxs-lookup"><span data-stu-id="e2c52-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="e2c52-327">![Initialisation de capture de réseau d’API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "capture réseau de lancement des API Web")</span><span class="sxs-lookup"><span data-stu-id="e2c52-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="e2c52-328">*Initialisation de capture de réseau d’API Web*</span><span class="sxs-lookup"><span data-stu-id="e2c52-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="e2c52-329">Ajouter **api/ApiPerson** à l’URL dans la barre d’adresses du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e2c52-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="e2c52-330">Vous inspecte maintenant les détails de la réponse de la **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="e2c52-331">![La récupération de données via l’API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "la récupération de données via l’API Web")</span><span class="sxs-lookup"><span data-stu-id="e2c52-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="e2c52-332">*La récupération des données via l’API Web*</span><span class="sxs-lookup"><span data-stu-id="e2c52-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2c52-333">Une fois le téléchargement terminé, vous devrez effectuer une action avec le fichier téléchargé.</span><span class="sxs-lookup"><span data-stu-id="e2c52-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="e2c52-334">Laissez la boîte de dialogue ouverte pour pouvoir être en mesure d’observer le contenu de réponse via la fenêtre outil de développeurs.</span><span class="sxs-lookup"><span data-stu-id="e2c52-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="e2c52-335">Maintenant vous inspecte le corps de la réponse.</span><span class="sxs-lookup"><span data-stu-id="e2c52-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="e2c52-336">Pour ce faire, cliquez sur le **détails** onglet, puis cliquez sur **corps de réponse**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="e2c52-337">Vous pouvez vérifier que les données téléchargées sont une liste d’objets avec les propriétés **Id**, **nom** et **âge** qui correspondent à la **personne** classe.</span><span class="sxs-lookup"><span data-stu-id="e2c52-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="e2c52-338">![Affichage Web de corps de la réponse de l’API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "affichage Web des corps de la réponse de l’API")</span><span class="sxs-lookup"><span data-stu-id="e2c52-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="e2c52-339">*Corps de réponse de l’API Web de visualisation*</span><span class="sxs-lookup"><span data-stu-id="e2c52-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="e2c52-340">Tâche 3 : ajout de Pages de l’aide API Web</span><span class="sxs-lookup"><span data-stu-id="e2c52-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="e2c52-341">Lorsque vous créez une API Web, il est utile de créer une page d’aide afin que les autres développeurs sache comment appeler votre API.</span><span class="sxs-lookup"><span data-stu-id="e2c52-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="e2c52-342">Vous pouvez créer et mettre à jour manuellement les pages de la documentation, mais il est préférable de générer automatiquement pour éviter d’avoir à effectuer les tâches de maintenance.</span><span class="sxs-lookup"><span data-stu-id="e2c52-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="e2c52-343">Dans cette tâche, vous utiliserez un package Nuget pour générer automatiquement des pages d’aide API Web à la solution.</span><span class="sxs-lookup"><span data-stu-id="e2c52-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="e2c52-344">À partir de la **outils** menu dans Visual Studio, sélectionnez **Gestionnaire de Package de bibliothèque**, puis cliquez sur **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="e2c52-345">Dans le **Package Manager Console** fenêtre, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e2c52-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="e2c52-346">Le **Microsoft.AspNet.WebApi.HelpPage** package installe les assemblys nécessaires et ajoute des vues MVC pour les pages d’aide sous le **zones/HelpPage** dossier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="e2c52-347">![Zone de HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage zone")</span><span class="sxs-lookup"><span data-stu-id="e2c52-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="e2c52-348">*Zone de HelpPage*</span><span class="sxs-lookup"><span data-stu-id="e2c52-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="e2c52-349">Par défaut, l’aide de pages ont des chaînes d’espace réservé pour la documentation.</span><span class="sxs-lookup"><span data-stu-id="e2c52-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="e2c52-350">Vous pouvez utiliser des commentaires de documentation XML pour créer la documentation.</span><span class="sxs-lookup"><span data-stu-id="e2c52-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="e2c52-351">Pour activer cette fonctionnalité, ouvrez le **HelpPageConfig.cs** fichier situé dans le **zones/HelpPage/application\_Démarrer** dossier et ne commentez pas la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="e2c52-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="e2c52-352">Dans **l’Explorateur de solutions**, cliquez sur le projet **MyHybridSite**, sélectionnez **propriétés** et cliquez sur le **générer** onglet.</span><span class="sxs-lookup"><span data-stu-id="e2c52-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="e2c52-353">![Onglet Générer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "générer la section")</span><span class="sxs-lookup"><span data-stu-id="e2c52-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="e2c52-354">*Onglet Générer*</span><span class="sxs-lookup"><span data-stu-id="e2c52-354">*Build tab*</span></span>
5. <span data-ttu-id="e2c52-355">Sous **sortie**, sélectionnez **fichier de documentation XML**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="e2c52-356">Dans la zone d’édition, tapez **application\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="e2c52-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="e2c52-357">![Section dans l’onglet générer de sortie](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "section dans l’onglet générer de sortie")</span><span class="sxs-lookup"><span data-stu-id="e2c52-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="e2c52-358">*Section de sortie dans l’onglet Générer*</span><span class="sxs-lookup"><span data-stu-id="e2c52-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="e2c52-359">Appuyez sur **CTRL** + **S** pour enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="e2c52-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="e2c52-360">Ouvrir le **ApiPersonController.cs** de fichiers à partir de la **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="e2c52-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="e2c52-361">Entrez une nouvelle ligne entre les *GetPeople* signature de méthode et la */ / api/ApiPerson d’obtenir* de commentaire, puis tapez les trois barres obliques.</span><span class="sxs-lookup"><span data-stu-id="e2c52-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2c52-362">Visual Studio insère automatiquement les éléments XML qui définissent la documentation de la méthode.</span><span class="sxs-lookup"><span data-stu-id="e2c52-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="e2c52-363">Ajouter un texte de résumé et de la valeur de retour pour la *GetPeople* (méthode).</span><span class="sxs-lookup"><span data-stu-id="e2c52-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="e2c52-364">Il doit se présenter comme suit.</span><span class="sxs-lookup"><span data-stu-id="e2c52-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="e2c52-365">Appuyez sur **F5** pour exécuter la solution.</span><span class="sxs-lookup"><span data-stu-id="e2c52-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="e2c52-366">Ajouter **/Help** à l’URL dans la barre d’adresses pour accéder à la page d’aide.</span><span class="sxs-lookup"><span data-stu-id="e2c52-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="e2c52-367">![Page d’aide de l’API Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "Page d’aide de l’API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="e2c52-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="e2c52-368">*Page d’aide de l’API Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="e2c52-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2c52-369">Le contenu principal de la page est une table d’API, regroupés par contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e2c52-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="e2c52-370">Les entrées de table sont générées dynamiquement, à l’aide de la **IApiExplorer** interface.</span><span class="sxs-lookup"><span data-stu-id="e2c52-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="e2c52-371">Si vous ajoutez ou mettez à jour le contrôleur d’API, la table mettra à jour automatiquement la prochaine fois que vous générez l’application.</span><span class="sxs-lookup"><span data-stu-id="e2c52-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="e2c52-372">Le **API** colonne répertorie la méthode HTTP et un URI relatif.</span><span class="sxs-lookup"><span data-stu-id="e2c52-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="e2c52-373">Le **Description** colonne contienne des informations qui ont été extraites de la documentation de la méthode.</span><span class="sxs-lookup"><span data-stu-id="e2c52-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="e2c52-374">Notez que la description que vous avez ajouté au-dessus de la définition de méthode s’affiche dans la colonne description.</span><span class="sxs-lookup"><span data-stu-id="e2c52-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="e2c52-375">![Description de la méthode API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "description de la méthode API")</span><span class="sxs-lookup"><span data-stu-id="e2c52-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="e2c52-376">*Description de la méthode API*</span><span class="sxs-lookup"><span data-stu-id="e2c52-376">*API method description*</span></span>
13. <span data-ttu-id="e2c52-377">Cliquez sur une des méthodes API pour accéder à une page avec des informations plus détaillées, y compris le corps de réponse d’exemple.</span><span class="sxs-lookup"><span data-stu-id="e2c52-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="e2c52-378">![Page d’informations de détail](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "page d’informations sur les détails")</span><span class="sxs-lookup"><span data-stu-id="e2c52-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="e2c52-379">*Page d’informations détaillées*</span><span class="sxs-lookup"><span data-stu-id="e2c52-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e2c52-380">Résumé</span><span class="sxs-lookup"><span data-stu-id="e2c52-380">Summary</span></span>

<span data-ttu-id="e2c52-381">À la fin de cet atelier pratique, vous avez appris comment :</span><span class="sxs-lookup"><span data-stu-id="e2c52-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="e2c52-382">Créez une application Web à l’aide de l’une expérience d’ASP.NET dans Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e2c52-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="e2c52-383">Intégrer plusieurs technologies ASP.NET dans un seul projet</span><span class="sxs-lookup"><span data-stu-id="e2c52-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="e2c52-384">Générer des contrôleurs MVC et les vues à partir de vos classes de modèle à l’aide de la génération de modèles automatique ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e2c52-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="e2c52-385">Générer des contrôleurs de l’API Web, qui utilisent des fonctionnalités telles que la programmation asynchrone et d’accès aux données via Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e2c52-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="e2c52-386">Générer automatiquement des Pages d’aide API Web pour vos contrôleurs</span><span class="sxs-lookup"><span data-stu-id="e2c52-386">Automatically generate Web API Help Pages for your controllers</span></span>
