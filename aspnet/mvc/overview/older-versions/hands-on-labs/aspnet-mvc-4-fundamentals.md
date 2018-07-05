---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Principes de base ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ce laboratoire pratique est basé sur le Store de musique MVC (Model View Controller), une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386573"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="8f582-103">Principes de base ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="8f582-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="8f582-104">par [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8f582-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="8f582-105">Télécharger le Kit de formation de Web Camps</span><span class="sxs-lookup"><span data-stu-id="8f582-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="8f582-106">Ce laboratoire pratique est basé sur le Store de musique MVC (Model View Controller), une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="8f582-107">Tout au long de l’atelier, vous allez découvrir la simplicité, encore alimentation de l’utilisation conjointe de ces technologies.</span><span class="sxs-lookup"><span data-stu-id="8f582-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="8f582-108">Vous démarrez avec une application simple et générez jusqu'à ce que vous avez une Application Web ASP.NET MVC 4 entièrement fonctionnelle.</span><span class="sxs-lookup"><span data-stu-id="8f582-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="8f582-109">Ce laboratoire fonctionne avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8f582-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="8f582-110">Si vous souhaitez Explorer la version d’ASP.NET MVC 3 de l’application du didacticiel, vous pouvez le trouver dans [MVC-musique-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="8f582-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="8f582-111">Ce laboratoire pratique suppose que le développeur a une expérience dans les technologies de développement Web, tels que HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8f582-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="8f582-112">Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="8f582-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="8f582-113">Le projet spécifique à ce laboratoire est disponible à l’adresse [principes de base ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="8f582-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="8f582-114">L’application Music Store</span><span class="sxs-lookup"><span data-stu-id="8f582-114">The Music Store application</span></span>

<span data-ttu-id="8f582-115">L’application web Store de musique qui est construite tout au long de ce laboratoire comprend trois parties principales : shopping, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="8f582-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="8f582-116">Visiteurs sera en mesure de parcourir des albums par genre, ajouter des albums à son panier, passez en revue leur sélection et enfin passer à l’extraction pour vous connecter et terminer la commande.</span><span class="sxs-lookup"><span data-stu-id="8f582-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="8f582-117">En outre, les administrateurs de magasin seront en mesure de gérer les albums disponibles, ainsi que leurs principales propriétés.</span><span class="sxs-lookup"><span data-stu-id="8f582-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="8f582-118">![Écrans de musique Store](aspnet-mvc-4-fundamentals/_static/image1.png "écrans Music Store")</span><span class="sxs-lookup"><span data-stu-id="8f582-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="8f582-119">*Écrans de musique Store*</span><span class="sxs-lookup"><span data-stu-id="8f582-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="8f582-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="8f582-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="8f582-121">Application de Store de musique est générée à l’aide de **contrôleur MVC (Model View)**, un modèle d’architecture qui sépare une application en trois composants principaux :</span><span class="sxs-lookup"><span data-stu-id="8f582-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="8f582-122">**Modèles**: objets de modèle sont les parties de l’application qui implémentent la logique de domaine.</span><span class="sxs-lookup"><span data-stu-id="8f582-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="8f582-123">Souvent, les objets de modèle également récupèrent et de stockent l’état de modèle dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="8f582-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="8f582-124">**Vues :** vues sont les composants qui affichent l’interface utilisateur de l’application (IU).</span><span class="sxs-lookup"><span data-stu-id="8f582-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="8f582-125">En règle générale, cette interface utilisateur est créée à partir des données de modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="8f582-126">Un exemple serait la vue edit d’Albums qui affiche les zones de texte et une liste déroulante, selon l’état actuel d’un objet de l’Album.</span><span class="sxs-lookup"><span data-stu-id="8f582-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="8f582-127">**Contrôleurs :** contrôleurs sont les composants qui gèrent l’interaction utilisateur, manipulent le modèle et finalement sélectionnent une vue à restituer l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f582-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="8f582-128">Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.</span><span class="sxs-lookup"><span data-stu-id="8f582-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="8f582-129">Le modèle MVC vous aide à créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique d’interface utilisateur), tout en assurant un couplage lâche entre ces éléments.</span><span class="sxs-lookup"><span data-stu-id="8f582-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="8f582-130">Cette séparation vous permet de gérer la complexité lorsque vous générez une application, car elle permet de vous concentrer sur l’un des aspects de l’implémentation à la fois.</span><span class="sxs-lookup"><span data-stu-id="8f582-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="8f582-131">En outre, le modèle MVC rend plus facile de tester des applications, encourage également l’utilisation du développement piloté par tests (TDD) pour créer des applications.</span><span class="sxs-lookup"><span data-stu-id="8f582-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="8f582-132">Le **ASP.NET MVC** framework offre une alternative au modèle Web Forms ASP.NET pour la création d’applications Web basées sur ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8f582-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="8f582-133">Le **ASP.NET MVC** est une infrastructure de présentation simple et facilement testable, qui (comme les applications Web basées sur des formulaires) est intégré avec les fonctionnalités ASP.NET existantes, telles que des pages maîtres et basée sur l’appartenance authentification et vous offre toute la puissance de .NET core.</span><span class="sxs-lookup"><span data-stu-id="8f582-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="8f582-134">Cela est utile si vous êtes déjà familiarisé avec ASP.NET Web Forms, car toutes les bibliothèques que vous utilisez déjà sont également disponibles dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8f582-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="8f582-135">En outre, le couplage lâche entre les trois principaux composants d’une application MVC favorise également développement en parallèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="8f582-136">Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur et un troisième peut se concentrer sur la logique métier dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="8f582-137">Objectifs</span><span class="sxs-lookup"><span data-stu-id="8f582-137">Objectives</span></span>

<span data-ttu-id="8f582-138">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="8f582-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="8f582-139">Créer une application ASP.NET MVC à partir de zéro, basée sur le didacticiel d’Application du Store musique</span><span class="sxs-lookup"><span data-stu-id="8f582-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="8f582-140">Ajouter des contrôleurs pour gérer des URL à la page d’accueil du site et pour ses principales fonctionnalités de navigation</span><span class="sxs-lookup"><span data-stu-id="8f582-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="8f582-141">Ajouter une vue pour personnaliser le contenu affiché, ainsi que son style</span><span class="sxs-lookup"><span data-stu-id="8f582-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="8f582-142">Ajouter des classes de modèle pour contenir et gérer la logique des données et de domaine</span><span class="sxs-lookup"><span data-stu-id="8f582-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="8f582-143">Utiliser un modèle de vue pour passer des informations à partir d’actions de contrôleur pour les modèles de vue</span><span class="sxs-lookup"><span data-stu-id="8f582-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="8f582-144">Explorez le nouveau modèle de ASP.NET MVC 4 pour les applications internet</span><span class="sxs-lookup"><span data-stu-id="8f582-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8f582-145">Prérequis</span><span class="sxs-lookup"><span data-stu-id="8f582-145">Prerequisites</span></span>

<span data-ttu-id="8f582-146">Vous devez disposer des éléments suivants pour terminer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="8f582-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="8f582-147">[Visual Studio 2012 Express pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer)</span><span class="sxs-lookup"><span data-stu-id="8f582-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="8f582-148">Installation</span><span class="sxs-lookup"><span data-stu-id="8f582-148">Setup</span></span>

<span data-ttu-id="8f582-149">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="8f582-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="8f582-150">Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="8f582-151">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="8f582-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="8f582-152">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe c :](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="8f582-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8f582-153">Exercices</span><span class="sxs-lookup"><span data-stu-id="8f582-153">Exercises</span></span>

<span data-ttu-id="8f582-154">Ce laboratoire pratique est constitué par les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="8f582-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="8f582-155">Exercice 1 : Création de projet d’Application Web du Store musique ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8f582-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="8f582-156">Exercice 2 : Création d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="8f582-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="8f582-157">Exercice 3 : Passage de paramètres à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="8f582-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="8f582-158">Exercice 4 : Création d’une vue</span><span class="sxs-lookup"><span data-stu-id="8f582-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="8f582-159">Exercice 5 : Création d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="8f582-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="8f582-160">Exercice 6 : Utilisation de paramètres dans la vue</span><span class="sxs-lookup"><span data-stu-id="8f582-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="8f582-161">Exercice 7 : Tour d’horizon nouveau modèle ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="8f582-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="8f582-162">Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="8f582-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="8f582-163">Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="8f582-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="8f582-164">Durée estimée pour effectuer ce laboratoire : **60 minutes**.</span><span class="sxs-lookup"><span data-stu-id="8f582-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="8f582-165">Exercice 1 : Création de projet d’Application Web du Store musique ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8f582-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="8f582-166">Dans cet exercice, vous allez apprendre à créer une application ASP.NET MVC dans Visual Studio 2012 Express pour Web, ainsi que son organisation des dossiers principaux.</span><span class="sxs-lookup"><span data-stu-id="8f582-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="8f582-167">En outre, vous allez apprendre à ajouter un nouveau contrôleur et les rendre afficher une chaîne simple dans la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="8f582-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="8f582-168">Tâche 1 : création du projet d’Application Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8f582-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="8f582-169">Dans cette tâche, vous allez créer un projet d’application ASP.NET MVC vide à l’aide du modèle MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="8f582-170">Démarrer **Visual Studio Express pour Web**.</span><span class="sxs-lookup"><span data-stu-id="8f582-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="8f582-171">Dans le menu **Fichier**, cliquez sur **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="8f582-172">Dans le **nouveau projet** boîte de dialogue Sélectionnez le **ASP.NET MVC 4 Web Application** projet type, situé sous **Visual c#,** **Web** modèle liste.</span><span class="sxs-lookup"><span data-stu-id="8f582-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="8f582-173">Modifier le **nom** à *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="8f582-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="8f582-174">Définir l’emplacement de la solution à l’intérieur d’un nouveau **commencer** dossier dans le dossier Source de cet exercice, par exemple **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="8f582-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="8f582-175">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f582-175">Click **OK**.</span></span>

    <span data-ttu-id="8f582-176">![Créer la boîte de dialogue Nouveau projet](aspnet-mvc-4-fundamentals/_static/image2.png "créer la boîte de dialogue Nouveau projet")</span><span class="sxs-lookup"><span data-stu-id="8f582-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="8f582-177">*Créer la boîte de dialogue Nouveau projet*</span><span class="sxs-lookup"><span data-stu-id="8f582-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="8f582-178">Dans le **nouveau projet ASP.NET MVC 4** sélectionnez boîte de dialogue le **base** modèle et vous assurer que le **moteur d’affichage** sélectionné est **Razor**.</span><span class="sxs-lookup"><span data-stu-id="8f582-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="8f582-179">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f582-179">Click **OK**.</span></span>

    <span data-ttu-id="8f582-180">![Nouvelle boîte de dialogue projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "nouvelle boîte de dialogue projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="8f582-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="8f582-181">*Nouvelle boîte de dialogue projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="8f582-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="8f582-182">Tâche 2 : Explorer la Structure de Solution</span><span class="sxs-lookup"><span data-stu-id="8f582-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="8f582-183">L’infrastructure ASP.NET MVC inclut un modèle de projet Visual Studio qui vous permet de créer des applications Web prenant en charge le modèle MVC.</span><span class="sxs-lookup"><span data-stu-id="8f582-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="8f582-184">Ce modèle crée une nouvelle application Web ASP.NET MVC avec les dossiers requis, les modèles d’élément et les entrées de fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="8f582-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="8f582-185">Dans cette tâche, vous allez examiner la structure de solution pour comprendre les éléments qui sont impliqués et leurs relations.</span><span class="sxs-lookup"><span data-stu-id="8f582-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="8f582-186">Les dossiers suivants sont inclus dans toute l’application ASP.NET MVC, car l’infrastructure ASP.NET MVC par défaut utilise un &quot;convention sur configuration&quot; approche et en fait des hypothèses par défaut en fonction de dossier d’affectation de noms conventions.</span><span class="sxs-lookup"><span data-stu-id="8f582-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="8f582-187">Une fois que le projet est créé, passez en revue la structure de dossiers a été créée dans l’Explorateur de solutions sur le côté droit :</span><span class="sxs-lookup"><span data-stu-id="8f582-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="8f582-188">![Structure de dossier de MVC ASP.NET dans l’Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image4.png "structure ASP.NET MVC dossier dans l’Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="8f582-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="8f582-189">*Structure de dossier de MVC ASP.NET dans l’Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="8f582-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="8f582-190">**Contrôleurs**.</span><span class="sxs-lookup"><span data-stu-id="8f582-190">**Controllers**.</span></span> <span data-ttu-id="8f582-191">Ce dossier contient les classes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8f582-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="8f582-192">Dans une application basée sur MVC, les contrôleurs sont chargés de gérer l’interaction de l’utilisateur final, manipuler le modèle et finalement en choisissant une vue à restituer l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f582-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="8f582-193">L’infrastructure MVC requiert les noms de tous les contrôleurs se terminent par &quot;contrôleur&quot;-par exemple, HomeController, LoginController ou ProductController.</span><span class="sxs-lookup"><span data-stu-id="8f582-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="8f582-194">**Modèles**.</span><span class="sxs-lookup"><span data-stu-id="8f582-194">**Models**.</span></span> <span data-ttu-id="8f582-195">Ce dossier est fourni pour les classes qui représentent le modèle d’application pour l’application Web MVC.</span><span class="sxs-lookup"><span data-stu-id="8f582-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="8f582-196">Cela inclut généralement le code qui définit les objets et la logique permettant d’interagir avec le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="8f582-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="8f582-197">En règle générale, les objets de modèle réels seront dans les bibliothèques de classes séparées.</span><span class="sxs-lookup"><span data-stu-id="8f582-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="8f582-198">Toutefois, lorsque vous créez une nouvelle application, vous pouvez inclure les classes et les déplacer dans les bibliothèques de classes séparées ultérieurement dans le cycle de développement.</span><span class="sxs-lookup"><span data-stu-id="8f582-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="8f582-199">**Vues**.</span><span class="sxs-lookup"><span data-stu-id="8f582-199">**Views**.</span></span> <span data-ttu-id="8f582-200">Ce dossier est l’emplacement recommandé pour les vues, les composants responsables de l’affichage de l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="8f582-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="8f582-201">Les vues utilisent des fichiers .aspx, .ascx, .cshtml et .master, en plus de tous les fichiers qui sont liées au rendu des vues.</span><span class="sxs-lookup"><span data-stu-id="8f582-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="8f582-202">Dossier Views contient un dossier pour chaque contrôleur ; le dossier est nommé avec le préfixe du nom de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8f582-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="8f582-203">Par exemple, si vous disposez d’un contrôleur nommé **HomeController**, le dossier Views contient un dossier appelé Home.</span><span class="sxs-lookup"><span data-stu-id="8f582-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="8f582-204">Par défaut, lorsque l’infrastructure ASP.NET MVC charge une vue, il recherche un fichier .aspx avec le nom de vue requis dans le dossier Views\controllerName (**vues [Nom_contrôleur] [Action] .aspx**) ou (**vues [Nom_contrôleur] [Action] .cshtml**) pour les vues Razor.</span><span class="sxs-lookup"><span data-stu-id="8f582-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8f582-205">Outre les dossiers indiqués précédemment, une application Web MVC utilise le **Global.asax** de fichier pour définir le routage d’URL globales par défaut et elle utilise le **Web.config** fichier pour configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="8f582-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="8f582-206">Tâche 3 : ajout d’un HomeController</span><span class="sxs-lookup"><span data-stu-id="8f582-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="8f582-207">Dans les applications ASP.NET qui n’utilisent pas l’infrastructure MVC, l’intervention de l’utilisateur est organisée autour des pages et déclenchement et la gestion des événements à partir de ces pages.</span><span class="sxs-lookup"><span data-stu-id="8f582-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="8f582-208">En revanche, l’interaction utilisateur avec les applications ASP.NET MVC est organisée autour des contrôleurs et leurs méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="8f582-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="8f582-209">En revanche, infrastructure ASP.NET MVC mappe des URL aux classes qui sont désignés comme contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="8f582-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="8f582-210">Contrôleurs de traiter les demandes entrantes, gèrent les entrées d’utilisateur et les interactions, exécuter la logique d’application appropriée et de déterminer la réponse à envoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers une URL différente, un etc.).</span><span class="sxs-lookup"><span data-stu-id="8f582-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="8f582-211">Dans le cas d’affichage HTML, une classe de contrôleur appelle généralement un composant de vue séparé afin de générer le balisage HTML pour la demande.</span><span class="sxs-lookup"><span data-stu-id="8f582-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="8f582-212">Dans une application MVC, la vue affiche uniquement des informations ; le contrôleur gère les entrées et interactions des utilisateurs, et y répond.</span><span class="sxs-lookup"><span data-stu-id="8f582-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="8f582-213">Dans cette tâche, vous allez ajouter une classe de contrôleur qui gérera les URL pour la page d’accueil du site Music Store.</span><span class="sxs-lookup"><span data-stu-id="8f582-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="8f582-214">Avec le bouton droit **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis **contrôleur** commande :</span><span class="sxs-lookup"><span data-stu-id="8f582-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="8f582-215">![Ajouter une commande de contrôleur](aspnet-mvc-4-fundamentals/_static/image5.png "ajouter une commande de contrôleur")</span><span class="sxs-lookup"><span data-stu-id="8f582-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="8f582-216">*Ajouter la commande de contrôleur*</span><span class="sxs-lookup"><span data-stu-id="8f582-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="8f582-217">Le **ajouter un contrôleur** boîte de dialogue apparaît.</span><span class="sxs-lookup"><span data-stu-id="8f582-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="8f582-218">Nommez le contrôleur *HomeController* et appuyez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="8f582-219">![Ajouter la boîte de dialogue contrôleur](aspnet-mvc-4-fundamentals/_static/image6.png "contrôleur la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="8f582-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="8f582-220">*Ajouter la boîte de dialogue contrôleur*</span><span class="sxs-lookup"><span data-stu-id="8f582-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="8f582-221">Le fichier **HomeController.cs** est créé dans le **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="8f582-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="8f582-222">Pour disposer le **HomeController** retourner une chaîne sur son action Index, remplacez le **Index** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="8f582-223">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="8f582-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="8f582-224">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="8f582-225">Dans cette tâche, vous allez essayer l’application dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="8f582-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="8f582-226">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="8f582-227">Le projet est compilé et démarre de serveur Web IIS local.</span><span class="sxs-lookup"><span data-stu-id="8f582-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="8f582-228">Local serveur Web IIS s’ouvre automatiquement un navigateur web pointant vers l’URL du serveur Web.</span><span class="sxs-lookup"><span data-stu-id="8f582-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="8f582-229">![Application qui s’exécute dans un navigateur web](aspnet-mvc-4-fundamentals/_static/image7.png "Application s’exécutant dans un navigateur web")</span><span class="sxs-lookup"><span data-stu-id="8f582-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="8f582-230">*Application qui s’exécute dans un navigateur web*</span><span class="sxs-lookup"><span data-stu-id="8f582-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-231">Le serveur Web IIS local s’exécutera le site Web sur un numéro de port libre aléatoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="8f582-232">Dans la figure ci-dessus, le site est en cours d’exécution à `http://localhost:50103/`, il utilise le port 50103.</span><span class="sxs-lookup"><span data-stu-id="8f582-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="8f582-233">Votre numéro de port peut varier.</span><span class="sxs-lookup"><span data-stu-id="8f582-233">Your port number may vary.</span></span>
2. <span data-ttu-id="8f582-234">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f582-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="8f582-235">Exercice 2 : Création d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="8f582-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="8f582-236">Dans cet exercice, vous allez apprendre à mettre à jour le contrôleur pour implémenter une fonctionnalité simple de l’application de Store de musique.</span><span class="sxs-lookup"><span data-stu-id="8f582-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="8f582-237">Ce contrôleur de définir les méthodes d’action pour gérer chacune des requêtes spécifiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="8f582-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="8f582-238">Une page de liste de genres de musique dans le Store musique</span><span class="sxs-lookup"><span data-stu-id="8f582-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="8f582-239">Une page de navigation qui répertorie tous les albums de musique pour un genre particulier</span><span class="sxs-lookup"><span data-stu-id="8f582-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="8f582-240">Une page de détails qui affiche des informations sur un album musical spécifique</span><span class="sxs-lookup"><span data-stu-id="8f582-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="8f582-241">Pour la portée de cet exercice, ces actions renvoie simplement une chaîne à ce stade.</span><span class="sxs-lookup"><span data-stu-id="8f582-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="8f582-242">Tâche 1 : ajout d’une nouvelle classe StoreController</span><span class="sxs-lookup"><span data-stu-id="8f582-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="8f582-243">Dans cette tâche, vous allez ajouter un nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8f582-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="8f582-244">Si elle est déjà ouverte, démarrez **Visual Studio Express 2012 pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="8f582-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="8f582-245">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="8f582-246">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex02-CreatingAController\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**.</span><span class="sxs-lookup"><span data-stu-id="8f582-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="8f582-247">Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8f582-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="8f582-248">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="8f582-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8f582-249">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8f582-250">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8f582-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8f582-251">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8f582-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8f582-252">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8f582-253">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8f582-254">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="8f582-255">Ajoutez le nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8f582-255">Add the new controller.</span></span> <span data-ttu-id="8f582-256">Pour ce faire, cliquez sur le **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis le **contrôleur** commande.</span><span class="sxs-lookup"><span data-stu-id="8f582-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="8f582-257">Modifier le **nom du contrôleur** à *StoreController*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="8f582-258">![Ajouter la boîte de dialogue contrôleur](aspnet-mvc-4-fundamentals/_static/image8.png "contrôleur la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="8f582-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="8f582-259">*Ajouter la boîte de dialogue contrôleur*</span><span class="sxs-lookup"><span data-stu-id="8f582-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="8f582-260">Tâche 2 : modification Actions de la StoreController</span><span class="sxs-lookup"><span data-stu-id="8f582-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="8f582-261">Dans cette tâche, vous allez modifier les méthodes de contrôleur sont appelées **actions**.</span><span class="sxs-lookup"><span data-stu-id="8f582-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="8f582-262">Actions sont responsables de la gestion des demandes d’URL et déterminer le contenu qui doit être envoyé au navigateur ou de l’utilisateur qui a appelé l’URL.</span><span class="sxs-lookup"><span data-stu-id="8f582-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="8f582-263">Le **StoreController** classe possède déjà un **Index** (méthode).</span><span class="sxs-lookup"><span data-stu-id="8f582-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="8f582-264">Vous l’utiliserez plus loin dans cet atelier pour implémenter la page qui répertorie tous les genres du magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="8f582-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="8f582-265">Pour l’instant, remplacez simplement le **Index** méthode avec le code suivant qui retourne une chaîne &quot;Hello from Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="8f582-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="8f582-266">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex2 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="8f582-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="8f582-267">Ajouter **Parcourir** et **détails** méthodes.</span><span class="sxs-lookup"><span data-stu-id="8f582-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="8f582-268">Pour ce faire, ajoutez le code suivant à la **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="8f582-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="8f582-269">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="8f582-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="8f582-270">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="8f582-271">Dans cette tâche, vous allez essayer l’application dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="8f582-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="8f582-272">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8f582-273">Le projet démarre dans le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="8f582-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="8f582-274">Modifier l’URL pour vérifier la mise en œuvre de chaque action.</span><span class="sxs-lookup"><span data-stu-id="8f582-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="8f582-275">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="8f582-275">**/Store**.</span></span> <span data-ttu-id="8f582-276">Vous verrez  **&quot;Hello from Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="8f582-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="8f582-277">**/ Store/Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="8f582-277">**/Store/Browse**.</span></span> <span data-ttu-id="8f582-278">Vous verrez  **&quot;Hello from Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="8f582-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="8f582-279">**/ Store/détails**.</span><span class="sxs-lookup"><span data-stu-id="8f582-279">**/Store/Details**.</span></span> <span data-ttu-id="8f582-280">Vous verrez  **&quot;Hello from Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="8f582-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="8f582-281">![Navigation StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de navigation")</span><span class="sxs-lookup"><span data-stu-id="8f582-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="8f582-282">*Navigation /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="8f582-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="8f582-283">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f582-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="8f582-284">Exercice 3 : Passage de paramètres à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="8f582-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="8f582-285">Jusqu'à présent, vous avez retournait les chaînes constantes à partir des contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="8f582-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="8f582-286">Dans cet exercice, vous allez apprendre à passer des paramètres à un contrôleur à l’aide de l’URL et la chaîne de requête, puis effectue les actions de la méthode répondre avec le texte dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f582-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="8f582-287">Tâche 1 - Ajout de paramètre de Genre à StoreController</span><span class="sxs-lookup"><span data-stu-id="8f582-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="8f582-288">Dans cette tâche, vous allez utiliser le **querystring** pour envoyer des paramètres pour le **Parcourir** méthode d’action dans le **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="8f582-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="8f582-289">Si elle est déjà ouverte, démarrez **Visual Studio Express pour Web**.</span><span class="sxs-lookup"><span data-stu-id="8f582-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="8f582-290">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="8f582-291">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex03-PassingParametersToAController\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**.</span><span class="sxs-lookup"><span data-stu-id="8f582-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="8f582-292">Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8f582-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="8f582-293">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="8f582-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8f582-294">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8f582-295">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8f582-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8f582-296">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8f582-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8f582-297">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8f582-298">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8f582-299">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="8f582-300">Ouvrez **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-300">Open **StoreController** class.</span></span> <span data-ttu-id="8f582-301">Pour ce faire, dans le **l’Explorateur de solutions**, développez le **contrôleurs** dossier et double-cliquez sur **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8f582-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="8f582-302">Modifier le **Parcourir** méthode, en ajoutant un paramètre de chaîne pour demander un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="8f582-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="8f582-303">ASP.NET MVC automatiquement passer toute chaîne de requête ou paramètres nommés de publication de formulaire **genre** à cette méthode d’action lorsqu’elle est appelée.</span><span class="sxs-lookup"><span data-stu-id="8f582-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="8f582-304">Pour ce faire, remplacez le **Parcourir** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="8f582-305">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="8f582-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="8f582-306">Vous utilisez le **HttpUtility.HtmlEncode** méthode utilitaire pour empêche les utilisateurs d’injecter du Javascript dans la vue avec un lien comme   **/Store/Parcourir ? Genre =&lt;script&gt;Window.location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="8f582-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="8f582-307">Pour en savoir plus, visitez [cet article msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="8f582-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="8f582-308">Tâche 2 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="8f582-309">Dans cette tâche, vous essayez de l’Application dans un navigateur web et utiliser le **genre** paramètre.</span><span class="sxs-lookup"><span data-stu-id="8f582-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="8f582-310">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8f582-311">Le projet démarre dans le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="8f582-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="8f582-312">Remplacez l’URL par   */Store/Parcourir ? Genre = Disco* pour vérifier que l’action reçoit le paramètre de genre.</span><span class="sxs-lookup"><span data-stu-id="8f582-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="8f582-313">![Navigation StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "navigation StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="8f582-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="8f582-314">*Navigation /Store/Browse ? Genre = Disco*</span><span class="sxs-lookup"><span data-stu-id="8f582-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="8f582-315">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f582-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="8f582-316">Tâche 3 : ajout d’un paramètre d’Id incorporé dans l’URL</span><span class="sxs-lookup"><span data-stu-id="8f582-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="8f582-317">Dans cette tâche, vous allez utiliser le **URL** à passer un **Id** paramètre à la **détails** méthode d’action de la **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="8f582-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="8f582-318">Convention de routage consiste à traiter le segment d’URL après le nom de la méthode d’action en tant que paramètre nommé de la valeur par défaut de ASP.NET MVC **Id**. Si votre méthode d’action a un paramètre nommé Id, puis ASP.NET MVC automatiquement passera le segment d’URL pour vous en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="8f582-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="8f582-319">Dans l’URL **détails/Store/5**, **Id** sera interprété comme **5**.</span><span class="sxs-lookup"><span data-stu-id="8f582-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="8f582-320">Modifier le **détails** méthode de la **StoreController**, l’ajout un **int** paramètre appelé **id**. Pour ce faire, remplacez le **détails** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="8f582-321">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="8f582-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="8f582-322">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="8f582-323">Dans cette tâche, vous essayez de l’Application dans un navigateur web et utiliser le **Id** paramètre.</span><span class="sxs-lookup"><span data-stu-id="8f582-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="8f582-324">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8f582-325">Le projet démarre dans le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="8f582-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="8f582-326">Remplacez l’URL par */Store/Details/5* pour vérifier que l’action reçoit le paramètre id.</span><span class="sxs-lookup"><span data-stu-id="8f582-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="8f582-327">![Navigation StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de navigation")</span><span class="sxs-lookup"><span data-stu-id="8f582-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="8f582-328">*Navigation /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="8f582-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="8f582-329">Exercice 4 : Création d’une vue</span><span class="sxs-lookup"><span data-stu-id="8f582-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="8f582-330">Jusqu'à présent, vous avez retournait chaînes à partir d’actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8f582-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="8f582-331">Bien que ce soit un moyen utile de comprendre le fonctionnement des contrôleurs, il n’est pas comment vos applications Web réelles sont générées.</span><span class="sxs-lookup"><span data-stu-id="8f582-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="8f582-332">Les vues sont des composants qui fournissent une meilleure approche pour générer le code HTML au navigateur à l’aide de fichiers de modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="8f582-333">Dans cet exercice, vous allez apprendre à ajouter une page maître de disposition pour configurer un modèle pour le contenu HTML commun, une feuille de style pour améliorer l’apparence du site et, enfin, un modèle de vue pour activer le HomeController renvoyer le HTML.</span><span class="sxs-lookup"><span data-stu-id="8f582-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="8f582-334">Tâche 1 : modification du fichier \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="8f582-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="8f582-335">Le fichier **~/Views/Shared/\_layout.cshtml** vous permet de configurer un modèle pour le code HTML commun à utiliser sur le site Web complet.</span><span class="sxs-lookup"><span data-stu-id="8f582-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="8f582-336">Dans cette tâche, vous allez ajouter une page maître de disposition avec un en-tête commun avec des liens vers la zone de page d’accueil et Store.</span><span class="sxs-lookup"><span data-stu-id="8f582-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="8f582-337">Si elle est déjà ouverte, démarrez **Visual Studio Express pour Web**.</span><span class="sxs-lookup"><span data-stu-id="8f582-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="8f582-338">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="8f582-339">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex04-CreatingAView\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**.</span><span class="sxs-lookup"><span data-stu-id="8f582-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="8f582-340">Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8f582-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="8f582-341">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="8f582-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8f582-342">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8f582-343">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8f582-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8f582-344">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8f582-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8f582-345">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8f582-346">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8f582-347">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="8f582-348">Le fichier  <strong>\_layout.cshtml</strong> contient la disposition du conteneur HTML pour toutes les pages sur le site.</span><span class="sxs-lookup"><span data-stu-id="8f582-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="8f582-349">Il inclut le <strong>&lt;html&gt;</strong> élément pour la réponse HTML, ainsi que le <strong>&lt;head&gt;</strong> et <strong>&lt;corps&gt;</strong> éléments.</span><span class="sxs-lookup"><span data-stu-id="8f582-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="8f582-350"><strong>@RenderBody()</strong> dans le code HTML corps identifier les zones cette vue modèles seront en mesure de le remplir avec le contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="8f582-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="8f582-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="8f582-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="8f582-352">Ajouter un en-tête commun avec des liens vers la zone de page d’accueil et Store sur toutes les pages du site.</span><span class="sxs-lookup"><span data-stu-id="8f582-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="8f582-353">Pour ce faire, ajoutez le code suivant ci-dessous &lt;corps&gt; instruction.</span><span class="sxs-lookup"><span data-stu-id="8f582-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="8f582-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="8f582-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="8f582-355">Inclure une balise div pour restituer la section de corps de chaque page.</span><span class="sxs-lookup"><span data-stu-id="8f582-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="8f582-356">Remplacez  <strong>@RenderBody()</strong> avec le code higlighted suivant : (c#)</span><span class="sxs-lookup"><span data-stu-id="8f582-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="8f582-357">Saviez-vous ?</span><span class="sxs-lookup"><span data-stu-id="8f582-357">Did you know?</span></span> <span data-ttu-id="8f582-358">Visual Studio 2012 offre des extraits de code qui la rendent facile d’ajouter le code utilisé couramment dans HTML, les fichiers de code et bien plus encore !</span><span class="sxs-lookup"><span data-stu-id="8f582-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="8f582-359">L’essayer en tapant **&lt;div&gt;** et en appuyant sur **onglet** à deux reprises pour insérer un complète **div** balise.</span><span class="sxs-lookup"><span data-stu-id="8f582-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="8f582-360">Tâche 2 - Ajout feuille de style CSS</span><span class="sxs-lookup"><span data-stu-id="8f582-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="8f582-361">Le modèle de projet vide inclut un fichier CSS très simplifié qui inclut uniquement les styles utilisés pour afficher les formes de base et les messages de validation.</span><span class="sxs-lookup"><span data-stu-id="8f582-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="8f582-362">Vous allez utiliser CSS et des images (potentiellement fournis par un concepteur) supplémentaires pour améliorer l’apparence du site.</span><span class="sxs-lookup"><span data-stu-id="8f582-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="8f582-363">Dans cette tâche, vous allez ajouter une feuille de style CSS pour définir les styles du site.</span><span class="sxs-lookup"><span data-stu-id="8f582-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="8f582-364">Le fichier CSS et les images sont inclus dans le **Source\Assets\Content** dossier de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="8f582-365">Afin de les ajouter à l’application, faites glisser leur contenu à partir d’un **Windows Explorer** fenêtre dans le **l’Explorateur de solutions** dans Visual Studio Express pour le Web, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8f582-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="8f582-366">![En faisant glisser le contenu de style](aspnet-mvc-4-fundamentals/_static/image12.png "en faisant glisser le contenu de style")</span><span class="sxs-lookup"><span data-stu-id="8f582-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="8f582-367">*En faisant glisser le contenu de style*</span><span class="sxs-lookup"><span data-stu-id="8f582-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="8f582-368">Une boîte de dialogue d’avertissement s’affiche, vous demandant de confirmer à remplacer **Site.css** fichier et certaines images existantes.</span><span class="sxs-lookup"><span data-stu-id="8f582-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="8f582-369">Vérifiez **appliquer à tous les éléments** et cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="8f582-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="8f582-370">Tâche 3 : ajout d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="8f582-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="8f582-371">Dans cette tâche, vous allez ajouter un modèle de vue pour générer la réponse HTML qui utilise la page maître de mise en page et ajouté des CSS dans cet exercice.</span><span class="sxs-lookup"><span data-stu-id="8f582-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="8f582-372">Pour utiliser un modèle de vue lorsque vous parcourez la page d’accueil du site, vous devez d’abord indiquer que, au lieu de renvoyer une chaîne, le **HomeController Index** méthode retournera un **vue**.</span><span class="sxs-lookup"><span data-stu-id="8f582-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="8f582-373">Ouvrez **HomeController** classe et modifier son **Index** méthode pour retourner un **ActionResult**, et qu’il renvoie **View()**.</span><span class="sxs-lookup"><span data-stu-id="8f582-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="8f582-374">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="8f582-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="8f582-375">Maintenant, vous devez ajouter un modèle de vue approprié.</span><span class="sxs-lookup"><span data-stu-id="8f582-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="8f582-376">Pour ce faire, **avec le bouton droit** à l’intérieur de la **Index** méthode d’action, puis **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8f582-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="8f582-377">Cela fera apparaître le **ajouter une vue** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="8f582-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="8f582-378">![Ajout d’une vue à partir de la méthode Index](aspnet-mvc-4-fundamentals/_static/image13.png "Ajout d’une vue à partir de la méthode Index")</span><span class="sxs-lookup"><span data-stu-id="8f582-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="8f582-379">*Ajout d’une vue à partir de la méthode Index*</span><span class="sxs-lookup"><span data-stu-id="8f582-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="8f582-380">Le **ajouter une vue** boîte de dialogue apparaît pour générer un fichier de modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="8f582-381">Par défaut, cette boîte de dialogue pré-remplit le nom du modèle de vue afin qu’elle corresponde à la méthode d’action qui va l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="8f582-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="8f582-382">Étant donné que vous avez utilisé le **ajouter une vue** menu contextuel dans le **Index** méthode d’action dans le HomeController, le **ajouter une vue** boîte de dialogue comporte des Index en tant que le nom de la vue par défaut.</span><span class="sxs-lookup"><span data-stu-id="8f582-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="8f582-383">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-383">Click **Add**.</span></span>

    <span data-ttu-id="8f582-384">![Afficher la boîte de dialogue Ajouter](aspnet-mvc-4-fundamentals/_static/image14.png "afficher la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="8f582-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="8f582-385">*Afficher la boîte de dialogue Ajouter*</span><span class="sxs-lookup"><span data-stu-id="8f582-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="8f582-386">Visual Studio génère un **Index.cshtml** afficher le modèle à l’intérieur de la **Views\Home** dossier, puis l’ouvre.</span><span class="sxs-lookup"><span data-stu-id="8f582-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="8f582-387">![Vue de l’Index créé d’accueil](aspnet-mvc-4-fundamentals/_static/image15.png "vue accueil Index créé")</span><span class="sxs-lookup"><span data-stu-id="8f582-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="8f582-388">*Vue d’accueil Index créé*</span><span class="sxs-lookup"><span data-stu-id="8f582-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-389">nom et l’emplacement de la **Index.cshtml** fichier concerne et suit les conventions de nommage d’ASP.NET MVC par défaut.</span><span class="sxs-lookup"><span data-stu-id="8f582-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="8f582-390">Le dossier \Views\**accueil** correspond au nom du contrôleur (**accueil** contrôleur).</span><span class="sxs-lookup"><span data-stu-id="8f582-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="8f582-391">Le nom de modèle de vue (**Index**), correspond à la méthode d’action de contrôleur qui affichent la vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="8f582-392">De cette façon, ASP.NET MVC évite d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lorsque vous utilisez cette convention de nommage pour retourner une vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="8f582-393">Le modèle de vue généré est basé sur le  **\_layout.cshtml** modèle précédemment défini.</span><span class="sxs-lookup"><span data-stu-id="8f582-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="8f582-394">Mettre à jour la propriété ViewBag.Title à **accueil**et modifier le contenu principal **c’est la Page d’accueil**, comme illustré dans le code ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8f582-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="8f582-395">Sélectionnez **MvcMusicStore** projet dans l’Explorateur de solutions et appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="8f582-396">Tâche 4 : vérification</span><span class="sxs-lookup"><span data-stu-id="8f582-396">Task 4: Verification</span></span>

<span data-ttu-id="8f582-397">Afin de vérifier que vous avez correctement effectué toutes les étapes dans l’exercice précédent, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="8f582-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="8f582-398">Avec l’application ouverte dans un navigateur, vous devez noter que :</span><span class="sxs-lookup"><span data-stu-id="8f582-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="8f582-399">Méthode d’action du HomeController Index trouvé et affiché le **\Views\Home\Index.cshtml** afficher le modèle, même si le code appelé **retourner View()**, car le modèle de vue suivi le convention d’affectation de noms standard.</span><span class="sxs-lookup"><span data-stu-id="8f582-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="8f582-400">La Page d’accueil affiche le message d’accueil défini dans le **\Views\Home\Index.cshtml** afficher le modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="8f582-401">À l’aide de la Page d’accueil est la  **\_layout.cshtml** modèle, et par conséquent, le message d’accueil est contenu dans la mise en page de site standard HTML.</span><span class="sxs-lookup"><span data-stu-id="8f582-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="8f582-402">![Vue d’Index à l’aide de la LayoutPage et le style défini d’accueil](aspnet-mvc-4-fundamentals/_static/image16.png "accueil vue de l’Index à l’aide de la LayoutPage et le style défini")</span><span class="sxs-lookup"><span data-stu-id="8f582-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="8f582-403">*Vue d’Index accueil à l’aide de la LayoutPage et le style défini*</span><span class="sxs-lookup"><span data-stu-id="8f582-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="8f582-404">Exercice 5 : Création d’un modèle de vue</span><span class="sxs-lookup"><span data-stu-id="8f582-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="8f582-405">Jusqu’ici, vous avez apportées à vos vues afficher codée en dur HTML, mais, pour créer des applications web dynamiques, le modèle de vue doit recevoir des informations à partir du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8f582-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="8f582-406">Est une technique courante pour être utilisée à cet effet la **ViewModel** modèle, ce qui permet à un contrôleur créer un package avec toutes les informations nécessaires pour générer la réponse HTML appropriée.</span><span class="sxs-lookup"><span data-stu-id="8f582-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="8f582-407">Dans cet exercice, vous allez apprendre à créer une classe ViewModel et ajoutez les propriétés requises : le nombre de genres dans le magasin et une liste de ces genres.</span><span class="sxs-lookup"><span data-stu-id="8f582-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="8f582-408">Vous met également à jour le StoreController pour utiliser le ViewModel créé, et enfin, vous allez créer un nouveau modèle de vue qui affiche les propriétés mentionnées dans la page.</span><span class="sxs-lookup"><span data-stu-id="8f582-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="8f582-409">Tâche 1 : création d’une classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="8f582-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="8f582-410">Dans cette tâche, vous allez créer une classe ViewModel qui implémentera le scénario de la liste de genre Store.</span><span class="sxs-lookup"><span data-stu-id="8f582-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="8f582-411">Si elle est déjà ouverte, démarrez **Visual Studio Express pour Web**.</span><span class="sxs-lookup"><span data-stu-id="8f582-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="8f582-412">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="8f582-413">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex05-CreatingAViewModel\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**.</span><span class="sxs-lookup"><span data-stu-id="8f582-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="8f582-414">Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8f582-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="8f582-415">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="8f582-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8f582-416">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8f582-417">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8f582-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8f582-418">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8f582-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8f582-419">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8f582-420">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8f582-421">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="8f582-422">Créer un **ViewModels** dossier pour contenir le ViewModel.</span><span class="sxs-lookup"><span data-stu-id="8f582-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="8f582-423">Pour ce faire, cliquez sur le niveau supérieur **MvcMusicStore** projet, sélectionnez **ajouter** , puis **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="8f582-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="8f582-424">![Ajout d’un nouveau dossier](aspnet-mvc-4-fundamentals/_static/image17.png "Ajout d’un nouveau dossier")</span><span class="sxs-lookup"><span data-stu-id="8f582-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="8f582-425">*Ajout d’un nouveau dossier*</span><span class="sxs-lookup"><span data-stu-id="8f582-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="8f582-426">Nommez le dossier *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="8f582-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="8f582-427">![Dossier ViewModels dans l’Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image18.png "dossier ViewModels dans l’Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="8f582-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="8f582-428">*Dossier ViewModels dans l’Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="8f582-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="8f582-429">Créer un **ViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="8f582-430">Pour ce faire, cliquez sur le **ViewModels** dossier créé récemment, sélectionnez **ajouter** , puis **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="8f582-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="8f582-431">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *StoreIndexViewModel.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="8f582-432">![Ajout d’une nouvelle classe](aspnet-mvc-4-fundamentals/_static/image19.png "ajoutant une nouvelle classe")</span><span class="sxs-lookup"><span data-stu-id="8f582-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="8f582-433">*Ajout d’une nouvelle classe*</span><span class="sxs-lookup"><span data-stu-id="8f582-433">*Adding a new Class*</span></span>

    <span data-ttu-id="8f582-434">![Création de classe de StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel de création de classe")</span><span class="sxs-lookup"><span data-stu-id="8f582-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="8f582-435">*Création de classe de StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="8f582-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="8f582-436">Tâche 2 : ajout de propriétés à la classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="8f582-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="8f582-437">Il existe deux paramètres à passer à partir de la StoreController pour le modèle de vue afin de générer la réponse HTML attendue : le nombre de genres dans le magasin et une liste de ces genres.</span><span class="sxs-lookup"><span data-stu-id="8f582-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="8f582-438">Dans cette tâche, vous allez ajouter ces 2 propriétés à la **StoreIndexViewModel** classe : **NumberOfGenres** (entier) et **Genres** (il s’agit d’une liste de chaînes).</span><span class="sxs-lookup"><span data-stu-id="8f582-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="8f582-439">Ajouter **NumberOfGenres** et **Genres** propriétés pour le **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="8f582-440">Pour ce faire, ajoutez les 2 lignes suivantes à la définition de classe :</span><span class="sxs-lookup"><span data-stu-id="8f582-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="8f582-441">(Code Snippet - *ASP.NET MVC 4 principes de base - propriétés de Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="8f582-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="8f582-442">Le **{get ; définir ;}**  notation utilise C# fonctionnalité des propriétés implémentées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="8f582-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="8f582-443">Il offre les avantages d’une propriété sans nécessiter de déclarer un champ de stockage.</span><span class="sxs-lookup"><span data-stu-id="8f582-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="8f582-444">Tâche 3 : mise à jour StoreController à utiliser le StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="8f582-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="8f582-445">Le **StoreIndexViewModel** classe encapsule les informations nécessaires pour passer de **StoreController**de **Index** à un modèle de vue afin de générer une réponse (méthode) .</span><span class="sxs-lookup"><span data-stu-id="8f582-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="8f582-446">Dans cette tâche, vous mettrez à jour la **StoreController** à utiliser le **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="8f582-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="8f582-447">Ouvrez **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="8f582-448">![Ouverture de la classe de StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "ouverture StoreController classe")</span><span class="sxs-lookup"><span data-stu-id="8f582-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="8f582-449">*Classe de StoreController ouverture*</span><span class="sxs-lookup"><span data-stu-id="8f582-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="8f582-450">Pour pouvoir utiliser le **StoreIndexViewModel** classe à partir de la **StoreController**, ajouter l’espace de noms suivante en haut de la **StoreController** code :</span><span class="sxs-lookup"><span data-stu-id="8f582-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="8f582-451">(Code Snippet - *ASP.NET MVC 4 principes de base - StoreIndexViewModel Ex5 à l’aide de ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="8f582-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="8f582-452">Modifier le **StoreController**de **Index** méthode d’action afin qu’elle crée et remplit un **StoreIndexViewModel** de l’objet et le transmet à un modèle de vue génère une réponse HTML avec lui.</span><span class="sxs-lookup"><span data-stu-id="8f582-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-453">Dans le laboratoire &quot;accès aux données et les modèles ASP.NET MVC&quot; vous écrivez du code qui Récupère la liste des genres de magasin à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="8f582-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="8f582-454">Dans le code suivant, vous allez créer un **liste** de genres de données factice qui rempliront le **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="8f582-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="8f582-455">Après la création et la configuration de la **StoreIndexViewModel** de l’objet, il sera passé en tant qu’argument à la **vue** (méthode).</span><span class="sxs-lookup"><span data-stu-id="8f582-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="8f582-456">Cela indique que le modèle de vue utilise cet objet pour générer une réponse HTML avec lui.</span><span class="sxs-lookup"><span data-stu-id="8f582-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="8f582-457">Remplacez le **Index** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="8f582-458">(Code Snippet - *ASP.NET MVC 4 principes de base - méthode Ex5 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="8f582-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="8f582-459">Si vous n’êtes pas familiarisé avec c#, vous pouvez supposer que l’utilisation **var** signifie que le **viewModel** variable est à liaison tardive.</span><span class="sxs-lookup"><span data-stu-id="8f582-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="8f582-460">Qui n’est pas correct - le compilateur c# à l’aide en fonction de ce que vous affectez à la variable d’inférence de type pour déterminer si **viewModel** est de type **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="8f582-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="8f582-461">Par ailleurs, de compilation local **viewModel** variable comme un **StoreIndexViewModel** vous tapez get vérification de la compilation et la prise en charge de Visual Studio-éditeur de code.</span><span class="sxs-lookup"><span data-stu-id="8f582-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="8f582-462">Tâche 4 : création d’un modèle de vue qui utilise StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="8f582-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="8f582-463">Dans cette tâche, vous allez créer un modèle de vue qui utilise un objet StoreIndexViewModel passé à partir du contrôleur pour afficher une liste de genres.</span><span class="sxs-lookup"><span data-stu-id="8f582-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="8f582-464">Avant de créer le nouveau modèle de vue, nous allons générer le projet afin que le **boîte de dialogue Vue ajouter** connaît le **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="8f582-465">Générez le projet en sélectionnant le **Build** élément de menu, puis **MvcMusicStore Build**.</span><span class="sxs-lookup"><span data-stu-id="8f582-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="8f582-466">![Génération du projet](aspnet-mvc-4-fundamentals/_static/image22.png "génération du projet")</span><span class="sxs-lookup"><span data-stu-id="8f582-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="8f582-467">*Génération du projet*</span><span class="sxs-lookup"><span data-stu-id="8f582-467">*Building the project*</span></span>
2. <span data-ttu-id="8f582-468">Créer un nouveau modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-468">Create a new View template.</span></span> <span data-ttu-id="8f582-469">Pour ce faire, le bouton droit dans le **Index** (méthode), puis sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8f582-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="8f582-470">![Ajout d’une vue](aspnet-mvc-4-fundamentals/_static/image23.png "Ajout d’une vue")</span><span class="sxs-lookup"><span data-stu-id="8f582-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="8f582-471">*Ajout d’une vue*</span><span class="sxs-lookup"><span data-stu-id="8f582-471">*Adding a View*</span></span>
3. <span data-ttu-id="8f582-472">Étant donné que le **boîte de dialogue Vue ajouter** a été appelée à partir de la **StoreController**, il ajoutera le modèle de vue par défaut dans un **\Views\Store\Index.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="8f582-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="8f582-473">Vérifier le **créer une fortement--vue typée** case à cocher, puis sélectionnez **StoreIndexViewModel** en tant que le **classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="8f582-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="8f582-474">En outre, assurez-vous que le moteur d’affichage sélectionné est **Razor**.</span><span class="sxs-lookup"><span data-stu-id="8f582-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="8f582-475">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-475">Click **Add**.</span></span>

    <span data-ttu-id="8f582-476">![Afficher la boîte de dialogue Ajouter](aspnet-mvc-4-fundamentals/_static/image24.png "afficher la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="8f582-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="8f582-477">*Afficher la boîte de dialogue Ajouter*</span><span class="sxs-lookup"><span data-stu-id="8f582-477">*Add View Dialog*</span></span>

    <span data-ttu-id="8f582-478">Le **\Views\Store\Index.cshtml** afficher le fichier de modèle est créé et ouvert.</span><span class="sxs-lookup"><span data-stu-id="8f582-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="8f582-479">Selon les informations fournies à la **ajouter une vue** boîte de dialogue de la dernière étape, la vue de modèle s’attend à recevoir un **StoreIndexViewModel** instance en tant que les données à utiliser pour générer une réponse HTML.</span><span class="sxs-lookup"><span data-stu-id="8f582-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="8f582-480">Vous remarquerez que le modèle hérite d’un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en c#.</span><span class="sxs-lookup"><span data-stu-id="8f582-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="8f582-481">Tâche 5 : mise à jour le modèle de vue</span><span class="sxs-lookup"><span data-stu-id="8f582-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="8f582-482">Dans cette tâche, vous mettrez à jour le modèle de vue créé dans la dernière tâche pour récupérer le nombre de genres et leurs noms dans la page.</span><span class="sxs-lookup"><span data-stu-id="8f582-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="8f582-483">Vous utiliserez @ syntaxe (souvent appelé &quot;pépites de code&quot;) pour exécuter du code dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="8f582-484">Dans le **Index.cshtml** de fichiers, dans le **Store** dossier, remplacez son code par le suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="8f582-485">Boucle sur la liste de genre dans **StoreIndexViewModel** et créer un élément HTML **&lt;ul&gt;** à l’aide de la liste un **foreach** boucle.</span><span class="sxs-lookup"><span data-stu-id="8f582-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="8f582-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="8f582-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="8f582-487">Appuyez sur **F5** pour exécuter l’Application et de parcourir **/Store**.</span><span class="sxs-lookup"><span data-stu-id="8f582-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="8f582-488">Vous verrez la liste des genres passé dans le **StoreIndexViewModel** de l’objet à partir de la **StoreController** pour le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="8f582-489">![Vue affichant une liste de genres](aspnet-mvc-4-fundamentals/_static/image26.png "vue affichant une liste de genres")</span><span class="sxs-lookup"><span data-stu-id="8f582-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="8f582-490">*Vue affichant une liste de genres*</span><span class="sxs-lookup"><span data-stu-id="8f582-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="8f582-491">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="8f582-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="8f582-492">Exercice 6 : Utilisation de paramètres dans la vue</span><span class="sxs-lookup"><span data-stu-id="8f582-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="8f582-493">Dans l’exercice 3, vous avez appris comment passer des paramètres au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8f582-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="8f582-494">Dans cet exercice, vous allez apprendre à utiliser ces paramètres dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="8f582-495">Pour ce faire, vous serez présentées tout d’abord aux classes de modèle qui vous aideront à gérer votre logique de données et de domaine.</span><span class="sxs-lookup"><span data-stu-id="8f582-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="8f582-496">En outre, vous allez apprendre à créer des liens vers les pages à l’intérieur de l’application ASP.NET MVC sans se préoccuper des éléments comme les chemins d’accès de l’URL d’encodage.</span><span class="sxs-lookup"><span data-stu-id="8f582-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="8f582-497">Tâche 1 : ajout de Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="8f582-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="8f582-498">Contrairement aux ViewModels, qui sont créés uniquement à transmettre des informations à partir du contrôleur à la vue, les classes de modèle sont conçues pour contenir et de gérer la logique des données et de domaine.</span><span class="sxs-lookup"><span data-stu-id="8f582-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="8f582-499">Dans cette tâche, vous allez ajouter deux classes de modèle pour représenter ces concepts : **Genre** et **Album**.</span><span class="sxs-lookup"><span data-stu-id="8f582-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="8f582-500">Si elle est déjà ouverte, démarrez **Visual Studio Express pour le Web**</span><span class="sxs-lookup"><span data-stu-id="8f582-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="8f582-501">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="8f582-502">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex06-UsingParametersInView\Begin**, sélectionnez **Begin.sln** et cliquez sur **Open**.</span><span class="sxs-lookup"><span data-stu-id="8f582-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="8f582-503">Ou bien, vous pouvez continuer avec la solution que vous avez obtenue à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8f582-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="8f582-504">Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="8f582-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8f582-505">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8f582-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8f582-506">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8f582-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8f582-507">Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8f582-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8f582-508">Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8f582-509">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8f582-510">C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="8f582-511">Ajouter un **Genre** classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="8f582-512">Pour ce faire, cliquez sur le **modèles** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option.</span><span class="sxs-lookup"><span data-stu-id="8f582-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="8f582-513">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *Genre.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="8f582-514">![Ajout d’une classe](aspnet-mvc-4-fundamentals/_static/image27.png "Ajout d’une classe")</span><span class="sxs-lookup"><span data-stu-id="8f582-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="8f582-515">*Ajout d’un nouvel élément*</span><span class="sxs-lookup"><span data-stu-id="8f582-515">*Adding a new item*</span></span>

    <span data-ttu-id="8f582-516">![Ajouter une classe de modèle de Genre](aspnet-mvc-4-fundamentals/_static/image28.png "ajouter une classe de modèle de Genre")</span><span class="sxs-lookup"><span data-stu-id="8f582-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="8f582-517">*Ajouter une classe de modèle de Genre*</span><span class="sxs-lookup"><span data-stu-id="8f582-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="8f582-518">Ajouter un **nom** propriété à la classe de Genre.</span><span class="sxs-lookup"><span data-stu-id="8f582-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="8f582-519">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-519">To do this, add the following code:</span></span>

    <span data-ttu-id="8f582-520">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="8f582-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="8f582-521">Suivant la même procédure que précédemment, ajoutez un **Album** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="8f582-522">Pour ce faire, cliquez sur le **modèles** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option.</span><span class="sxs-lookup"><span data-stu-id="8f582-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="8f582-523">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *Album.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="8f582-524">Ajouter deux propriétés à la classe Album : **Genre** et **titre**.</span><span class="sxs-lookup"><span data-stu-id="8f582-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="8f582-525">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-525">To do this, add the following code:</span></span>

    <span data-ttu-id="8f582-526">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="8f582-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="8f582-527">Tâche 2 : ajout d’un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="8f582-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="8f582-528">Un **StoreBrowseViewModel** servira dans cette tâche pour afficher les Albums qui correspondent à un Genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="8f582-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="8f582-529">Dans cette tâche, vous créez cette classe et puis ajoutez deux propriétés permettant de gérer le **Genre** et son **Album**de liste.</span><span class="sxs-lookup"><span data-stu-id="8f582-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="8f582-530">Ajouter un **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="8f582-531">Pour ce faire, cliquez sur le **ViewModels** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option.</span><span class="sxs-lookup"><span data-stu-id="8f582-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="8f582-532">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *StoreBrowseViewModel.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="8f582-533">Ajoutez une référence aux modèles dans **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="8f582-534">Pour ce faire, ajoutez le code suivant à l’aide d’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="8f582-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="8f582-535">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="8f582-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="8f582-536">Ajoutez deux propriétés à **StoreBrowseViewModel** classe : **Genre** et **Albums**.</span><span class="sxs-lookup"><span data-stu-id="8f582-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="8f582-537">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-537">To do this, add the following code:</span></span>

    <span data-ttu-id="8f582-538">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="8f582-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="8f582-539">What ' s **liste&lt;Album&gt;**  ? : à l’aide de cette définition de la **liste&lt;T&gt;**  type, où **T** contraint le type à des éléments de ce **liste** appartiennent dans ce cas, **Album** (ou un de ses descendants).</span><span class="sxs-lookup"><span data-stu-id="8f582-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="8f582-540">Cette capacité à concevoir des classes et méthodes qui diffèrent la spécification d’un ou plusieurs types jusqu'à ce que la classe ou la méthode est déclarée et instanciée par le code client est une fonctionnalité du langage c# appelé **génériques**.</span><span class="sxs-lookup"><span data-stu-id="8f582-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="8f582-541">**Liste&lt;T&gt;**  est l’équivalent générique de la **ArrayList** de type et est disponible dans le **System.Collections.Generic** espace de noms.</span><span class="sxs-lookup"><span data-stu-id="8f582-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="8f582-542">Un des avantages de l’utilisation de **génériques** est que dans la mesure où le type est spécifié, vous n’avez pas besoin prendre en charge de la vérification des opérations telles que le cast des éléments dans des types **Album** comme vous le feriez avec un **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="8f582-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="8f582-543">Tâche 3 : à l’aide de nouveau ViewModel dans le StoreController</span><span class="sxs-lookup"><span data-stu-id="8f582-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="8f582-544">Dans cette tâche, vous allez modifier le **StoreController**de **Parcourir** et **détails** méthodes d’action pour utiliser le nouveau **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="8f582-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="8f582-545">Ajouter une référence dans le dossier de modèles dans **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="8f582-546">Pour ce faire, développez le **contrôleurs** dossier dans le **l’Explorateur de solutions** et ouvrez le **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="8f582-547">Puis ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-547">Then add the following code:</span></span>

    <span data-ttu-id="8f582-548">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="8f582-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="8f582-549">Remplacez le **Parcourir** méthode d’action à utiliser le **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="8f582-550">Vous allez créer un Genre et deux nouveaux objets Albums avec données factices (dans l’atelier pratique suivant vous allez consommer des données réelles à partir d’une base de données).</span><span class="sxs-lookup"><span data-stu-id="8f582-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="8f582-551">Pour ce faire, remplacez le **Parcourir** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="8f582-552">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="8f582-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="8f582-553">Remplacez le **détails** méthode d’action à utiliser le **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="8f582-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="8f582-554">Vous allez créer un nouveau **Album** objet à retourner à la **vue**.</span><span class="sxs-lookup"><span data-stu-id="8f582-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="8f582-555">Pour ce faire, remplacez le **détails** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="8f582-556">(Code Snippet - *bases d’ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="8f582-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="8f582-557">Tâche 4 : ajout d’un modèle d’affichage Parcourir</span><span class="sxs-lookup"><span data-stu-id="8f582-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="8f582-558">Dans cette tâche, vous allez ajouter un **Parcourir** pour afficher des Albums trouvées pour un Genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="8f582-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="8f582-559">Avant de créer le nouveau modèle de vue, vous devez créer le projet afin que le **ajouter une vue** dialogue connaît le **ViewModel** classe à utiliser.</span><span class="sxs-lookup"><span data-stu-id="8f582-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="8f582-560">Générez le projet en sélectionnant le **Build** élément de menu, puis **MvcMusicStore Build**.</span><span class="sxs-lookup"><span data-stu-id="8f582-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="8f582-561">Ajouter un **Parcourir** vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-561">Add a **Browse** View.</span></span> <span data-ttu-id="8f582-562">Pour ce faire, cliquez sur le **Parcourir** méthode d’action de la **StoreController** et cliquez sur **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8f582-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="8f582-563">Dans le **ajouter une vue** boîte de dialogue, vérifiez que le nom de la vue est **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="8f582-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="8f582-564">Vérifier le **créer une vue fortement typée** case à cocher et sélectionnez **StoreBrowseViewModel** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8f582-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="8f582-565">Laissez les autres champs avec leur valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="8f582-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="8f582-566">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-566">Then click **Add**.</span></span>

    <span data-ttu-id="8f582-567">![Ajout d’une vue Parcourir](aspnet-mvc-4-fundamentals/_static/image29.png "Ajout d’une vue Parcourir")</span><span class="sxs-lookup"><span data-stu-id="8f582-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="8f582-568">*Ajout d’une vue Parcourir*</span><span class="sxs-lookup"><span data-stu-id="8f582-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="8f582-569">Modifier le **Browse.cshtml** pour afficher des informations du Genre l’accès à la **StoreBrowseViewModel** objet qui est passé au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="8f582-570">Pour ce faire, remplacez le contenu par le suivant : (c#)</span><span class="sxs-lookup"><span data-stu-id="8f582-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="8f582-571">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="8f582-572">Dans cette tâche, vous allez tester que le **Parcourir** méthode récupère les Albums à partir de la **Parcourir** action de la méthode.</span><span class="sxs-lookup"><span data-stu-id="8f582-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="8f582-573">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8f582-574">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8f582-574">The project starts in the Home page.</span></span> <span data-ttu-id="8f582-575">Remplacez l’URL par   **/Store/Parcourir ? Genre = Disco** pour vérifier que l’action retourne deux Albums.</span><span class="sxs-lookup"><span data-stu-id="8f582-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="8f582-576">![Exploration des Albums Disco Store](aspnet-mvc-4-fundamentals/_static/image30.png "navigation Albums Disco Store")</span><span class="sxs-lookup"><span data-stu-id="8f582-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="8f582-577">*Navigation Albums Disco Store*</span><span class="sxs-lookup"><span data-stu-id="8f582-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="8f582-578">Tâche 6 - Affichage des informations sur un Album spécifique</span><span class="sxs-lookup"><span data-stu-id="8f582-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="8f582-579">Dans cette tâche, vous allez implémenter le **Store/détails** pour afficher les informations sur un album spécifique.</span><span class="sxs-lookup"><span data-stu-id="8f582-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="8f582-580">Dans cet atelier pratique, tout ce que vous afficherez l’album sur la figure déjà dans le **vue** modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="8f582-581">C’est le cas, au lieu de créer un **StoreDetailsViewModel** (classe), vous allez utiliser actuel **StoreBrowseViewModel** modèle en passant l’Album à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="8f582-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="8f582-582">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="8f582-583">Ajouter un nouveau **détails** afficher pour le **StoreController**de **détails** méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="8f582-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="8f582-584">Pour ce faire, cliquez sur le **détails** méthode dans le **StoreController** classe et cliquez sur **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8f582-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="8f582-585">Dans le **ajouter une vue** boîte de dialogue, vérifiez que le **nom de la vue** est **détails**.</span><span class="sxs-lookup"><span data-stu-id="8f582-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="8f582-586">Vérifier le **créer une vue fortement typée** case à cocher et sélectionnez **Album** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8f582-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="8f582-587">Laissez les autres champs avec leur valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="8f582-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="8f582-588">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8f582-588">Then click **Add**.</span></span> <span data-ttu-id="8f582-589">Cela crée et ouvre un **\Views\Store\Details.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="8f582-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="8f582-590">![Ajout d’une vue de détails](aspnet-mvc-4-fundamentals/_static/image31.png "Ajout d’une vue de détails")</span><span class="sxs-lookup"><span data-stu-id="8f582-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="8f582-591">*Ajout d’une vue de détails*</span><span class="sxs-lookup"><span data-stu-id="8f582-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="8f582-592">Modifier le **Details.cshtml** fichier pour afficher des informations de l’Album, l’accès à la **Album** objet qui est passé au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="8f582-593">Pour ce faire, remplacez le contenu par le suivant : (c#)</span><span class="sxs-lookup"><span data-stu-id="8f582-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="8f582-594">Tâche 7 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="8f582-595">Dans cette tâche, vous allez tester que le **détails** View récupère les informations d’Album à partir de la **détaille action** (méthode).</span><span class="sxs-lookup"><span data-stu-id="8f582-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="8f582-596">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8f582-597">Le projet démarre dans le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="8f582-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="8f582-598">Remplacez l’URL par **/Store/Details/5** pour vérifier les informations de l’album.</span><span class="sxs-lookup"><span data-stu-id="8f582-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="8f582-599">![Exploration en détail les Albums](aspnet-mvc-4-fundamentals/_static/image32.png "exploration en détail les Albums")</span><span class="sxs-lookup"><span data-stu-id="8f582-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="8f582-600">*Exploration des détails de l’Album*</span><span class="sxs-lookup"><span data-stu-id="8f582-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="8f582-601">Tâche 8 : ajout de liens entre les Pages</span><span class="sxs-lookup"><span data-stu-id="8f582-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="8f582-602">Dans cette tâche, vous allez ajouter un lien dans la vue de Store pour avoir un lien dans le nom de chaque Genre appropriés **/Store/Parcourir** URL.</span><span class="sxs-lookup"><span data-stu-id="8f582-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="8f582-603">Ainsi, lorsque vous cliquez sur un Genre, par exemple **Disco**, il permet d’accéder à **/Store/Parcourir ? genre = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="8f582-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="8f582-604">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="8f582-605">Mise à jour le **Index** page pour ajouter un lien vers le **Parcourir** page.</span><span class="sxs-lookup"><span data-stu-id="8f582-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="8f582-606">Pour ce faire, dans le **l’Explorateur de solutions** développez la **vues** dossier, puis le **Store** dossier et double-cliquez sur le **Index.cshtml** page.</span><span class="sxs-lookup"><span data-stu-id="8f582-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="8f582-607">Ajouter un lien vers la vue Parcourir indiquant le genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="8f582-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="8f582-608">Pour ce faire, remplacez le code en surbrillance suivant au sein de la **&lt;li&gt;** balises : (c#)</span><span class="sxs-lookup"><span data-stu-id="8f582-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="8f582-609">une autre approche serait liaison directement à la page, avec un code semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="8f582-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="8f582-610">&lt;un href =&quot;/Store/Parcourir ? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="8f582-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="8f582-611">Bien que cette approche fonctionne, elle dépend d’une chaîne codée en dur.</span><span class="sxs-lookup"><span data-stu-id="8f582-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="8f582-612">Si vous renommez ultérieurement le contrôleur, vous devrez modifier cette instruction manuellement.</span><span class="sxs-lookup"><span data-stu-id="8f582-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="8f582-613">Une meilleure alternative consiste à utiliser un **programme d’assistance HTML** (méthode).</span><span class="sxs-lookup"><span data-stu-id="8f582-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="8f582-614">ASP.NET MVC inclut une méthode d’assistance HTML qui est disponible pour des tâches.</span><span class="sxs-lookup"><span data-stu-id="8f582-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="8f582-615">Le **Html.ActionLink()** méthode d’assistance permet de facilement générer HTML **&lt;un&gt;** liens, s’assurer que les chemins d’URL sont correctement codées URL.</span><span class="sxs-lookup"><span data-stu-id="8f582-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="8f582-616">Htlm.ActionLink a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="8f582-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="8f582-617">Dans cet exercice, vous utiliserez une fonction qui accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="8f582-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="8f582-618">Texte du lien, qui affiche le nom du Genre</span><span class="sxs-lookup"><span data-stu-id="8f582-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="8f582-619">Nom d’action de contrôleur (**Parcourir**)</span><span class="sxs-lookup"><span data-stu-id="8f582-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="8f582-620">Des valeurs de paramètre, en spécifiant le nom d’itinéraire (**Genre**) et la valeur (**nom du Genre**)</span><span class="sxs-lookup"><span data-stu-id="8f582-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="8f582-621">Tâche 9 - exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="8f582-622">Dans cette tâche, vous allez tester que chaque Genre s’affiche avec un lien vers le texte approprié **/Store/Parcourir** URL.</span><span class="sxs-lookup"><span data-stu-id="8f582-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="8f582-623">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8f582-624">Le projet démarre dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8f582-624">The project starts in the Home page.</span></span> <span data-ttu-id="8f582-625">Remplacez l’URL par **/Store** pour vérifier que chaque Genre des liens appropriés **/Store/Parcourir** URL.</span><span class="sxs-lookup"><span data-stu-id="8f582-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="8f582-626">![Genres de navigation avec des liens vers la page Parcourir](aspnet-mvc-4-fundamentals/_static/image33.png "Genres de navigation avec des liens vers la page Parcourir")</span><span class="sxs-lookup"><span data-stu-id="8f582-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="8f582-627">*Genres de navigation avec des liens vers la page Parcourir*</span><span class="sxs-lookup"><span data-stu-id="8f582-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="8f582-628">Tâche 10 : à l’aide de la Collection de ViewModel dynamique pour transmettre des valeurs</span><span class="sxs-lookup"><span data-stu-id="8f582-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="8f582-629">Dans cette tâche, vous allez apprendre à une méthode simple et efficace pour transmettre des valeurs entre le contrôleur et la vue sans apporter de modifications dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="8f582-630">ASP.NET MVC 4 fournit la collection &quot;ViewModel&quot;, ce qui peut être affecté à n’importe quelle valeur dynamique et accessible à l’intérieur des contrôleurs et les vues ainsi.</span><span class="sxs-lookup"><span data-stu-id="8f582-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="8f582-631">Vous allez maintenant utiliser le regroupement dynamique ViewBag pour passer d’une liste de &quot; **marqué d’une étoile genres** &quot; à partir du contrôleur à la vue.</span><span class="sxs-lookup"><span data-stu-id="8f582-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="8f582-632">La vue Index de Store accédera à **ViewModel** et afficher les informations.</span><span class="sxs-lookup"><span data-stu-id="8f582-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="8f582-633">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="8f582-634">Ouvrez **StoreController.cs** et modifier **Index** méthode pour créer une liste de marqué d’une étoile genres dans la collection de ViewModel :</span><span class="sxs-lookup"><span data-stu-id="8f582-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="8f582-635">Vous pouvez également utiliser la syntaxe **ViewBag [&quot;Starred&quot;]** pour accéder aux propriétés.</span><span class="sxs-lookup"><span data-stu-id="8f582-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="8f582-636">L’icône d’étoile **&quot;starred.png&quot;** est inclus dans le **Source\Assets\Images** dossier de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8f582-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="8f582-637">Pour l’ajouter à l’application, faites glisser leur contenu à partir d’un **Windows Explorer** fenêtre dans le **l’Explorateur de solutions** dans Visual Web Developer Express, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8f582-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="8f582-638">![Image étoile d’ajout à la solution](aspnet-mvc-4-fundamentals/_static/image34.png "image étoile d’ajout à la solution")</span><span class="sxs-lookup"><span data-stu-id="8f582-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="8f582-639">*Ajout de l’image en étoile à la solution*</span><span class="sxs-lookup"><span data-stu-id="8f582-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="8f582-640">Ouvrez la vue **Store/Index.cshtml** et modifier le contenu.</span><span class="sxs-lookup"><span data-stu-id="8f582-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="8f582-641">Vous allez lire les &quot;marqué d’une étoile&quot; propriété dans le **ViewBag** collection et demandez si le nom du genre en cours est dans la liste.</span><span class="sxs-lookup"><span data-stu-id="8f582-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="8f582-642">Dans ce cas, vous afficherez une icône représentant une étoile direct sur le lien de genre.</span><span class="sxs-lookup"><span data-stu-id="8f582-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="8f582-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="8f582-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="8f582-644">Tâche 11 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8f582-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="8f582-645">Dans cette tâche, vous allez tester que les genres avec une astérisque affichent une icône représentant une étoile.</span><span class="sxs-lookup"><span data-stu-id="8f582-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="8f582-646">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8f582-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8f582-647">Le projet démarre dans le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="8f582-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="8f582-648">Remplacez l’URL par **/Store** pour vérifier que chaque genre proposée est l’étiquette digne :</span><span class="sxs-lookup"><span data-stu-id="8f582-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="8f582-649">![Genres de navigation avec des éléments avec une astérisque](aspnet-mvc-4-fundamentals/_static/image35.png "Genres de navigation avec des éléments avec une astérisque")</span><span class="sxs-lookup"><span data-stu-id="8f582-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="8f582-650">*Genres de navigation avec des éléments avec une astérisque*</span><span class="sxs-lookup"><span data-stu-id="8f582-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="8f582-651">Exercice 7 : Tour d’horizon nouveau modèle d’ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="8f582-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="8f582-652">Dans cet exercice, vous allez explorer les améliorations dans les modèles de projet ASP.NET MVC 4, jeter un coup de œil des fonctionnalités les plus pertinentes du nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="8f582-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="8f582-653">Tâche 1 : Explorer le modèle d’Application ASP.NET MVC 4 Internet</span><span class="sxs-lookup"><span data-stu-id="8f582-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="8f582-654">Si elle est déjà ouverte, démarrez **Visual Studio Express pour le Web**</span><span class="sxs-lookup"><span data-stu-id="8f582-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="8f582-655">Sélectionnez le **fichier | Nouveau | Projet** commande de menu.</span><span class="sxs-lookup"><span data-stu-id="8f582-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="8f582-656">Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche arborescence, puis choisissez le **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="8f582-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="8f582-657">**Nom** le projet *MusicStore* et mettre à jour le **nom de la solution** à *commencer*, puis sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="8f582-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="8f582-658">![Création d’un nouveau projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "création d’un nouveau projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="8f582-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="8f582-659">*Création d’un nouveau projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="8f582-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="8f582-660">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Internet** modèle de projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f582-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="8f582-661">Notez que vous pouvez sélectionner Razor ou ASPX comme le moteur d’affichage.</span><span class="sxs-lookup"><span data-stu-id="8f582-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="8f582-662">![Création d’une Application ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "création d’une Application ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="8f582-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="8f582-663">*Création d’une Application ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="8f582-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-664">Syntaxe Razor a été introduite dans ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="8f582-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="8f582-665">Son objectif est de réduire le nombre de caractères et séquences de touches requis dans un fichier, en activant un rapide et le fluide de flux de travail de codage.</span><span class="sxs-lookup"><span data-stu-id="8f582-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="8f582-666">Razor utilise existant C# /Visual Basic (ou autre) compétences linguistiques et offre une syntaxe de balisage de modèle qui permet un flux de travail de construction HTML exceptionnel.</span><span class="sxs-lookup"><span data-stu-id="8f582-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="8f582-667">Appuyez sur **F5** pour exécuter la solution et voir le modèle renouvelé.</span><span class="sxs-lookup"><span data-stu-id="8f582-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="8f582-668">Vous pouvez consulter les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="8f582-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="8f582-669">**Modèles de moderne**</span><span class="sxs-lookup"><span data-stu-id="8f582-669">**Modern-style templates**</span></span>

        <span data-ttu-id="8f582-670">Les modèles ont été renouvelés, en fournissant plus de styles modernes.</span><span class="sxs-lookup"><span data-stu-id="8f582-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="8f582-671">![Modèles ASP.NET MVC 4 redessiné](aspnet-mvc-4-fundamentals/_static/image38.png "redessiné modèles ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="8f582-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="8f582-672">*Modèles ASP.NET MVC 4 redessiné*</span><span class="sxs-lookup"><span data-stu-id="8f582-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="8f582-673">**Rendu adaptatif**</span><span class="sxs-lookup"><span data-stu-id="8f582-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="8f582-674">Découvrez de redimensionnement de la fenêtre du navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre.</span><span class="sxs-lookup"><span data-stu-id="8f582-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="8f582-675">Ces modèles utilisent la technique de rendu adaptatif pour afficher correctement dans les plateformes de bureau et mobiles sans aucune personnalisation.</span><span class="sxs-lookup"><span data-stu-id="8f582-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="8f582-676">![Modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents](aspnet-mvc-4-fundamentals/_static/image39.png "le modèle de projet ASP.NET MVC 4 dans les tailles autre navigateur")</span><span class="sxs-lookup"><span data-stu-id="8f582-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="8f582-677">*Modèle de projet ASP.NET MVC 4 dans les tailles autre navigateur*</span><span class="sxs-lookup"><span data-stu-id="8f582-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="8f582-678">Fermez le navigateur pour arrêter le débogueur, revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="8f582-679">Vous êtes maintenant en mesure d’Explorer la solution et découvrez quelques-unes des nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="8f582-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="8f582-680">![ASP.NET MVC 4-internet-application--modèle de projet](aspnet-mvc-4-fundamentals/_static/image40.png "le modèle de projet Application Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="8f582-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="8f582-681">*Le modèle de projet Application Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="8f582-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="8f582-682">**Balisage HTML5**</span><span class="sxs-lookup"><span data-stu-id="8f582-682">**HTML5 markup**</span></span>

       <span data-ttu-id="8f582-683">Parcourir les vues de modèle pour rechercher le nouveau thème le balisage, par exemple ouvrir **About.cshtml** afficher au sein de **accueil** dossier.</span><span class="sxs-lookup"><span data-stu-id="8f582-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="8f582-684">![Nouveau modèle, en utilisant le balisage Razor et HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nouveau modèle, en utilisant le balisage Razor et HTML5")</span><span class="sxs-lookup"><span data-stu-id="8f582-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="8f582-685">*Nouveau modèle, en utilisant le balisage Razor et HTML5*</span><span class="sxs-lookup"><span data-stu-id="8f582-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="8f582-686">**Bibliothèques JavaScript inclus**</span><span class="sxs-lookup"><span data-stu-id="8f582-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="8f582-687">**jQuery**: jQuery simplifie la traversée de document HTML, la gestion des événements, l’animation et les interactions Ajax.</span><span class="sxs-lookup"><span data-stu-id="8f582-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="8f582-688">**jQuery UI**: cette bibliothèque fournit des abstractions d’interaction de bas niveau et d’avancée de l’animation, effets et de widgets de thème, reposant sur la bibliothèque JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="8f582-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="8f582-689">Vous pouvez en savoir plus sur jQuery et l’interface utilisateur jQuery dans [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="8f582-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="8f582-690">**KnockoutJS**: modèle par défaut de l’ASP.NET MVC 4 inclut désormais **KnockoutJS**, une infrastructure MVVM JavaScript qui vous permet de créer des applications web riches et hautement réactives à l’aide de JavaScript et HTML.</span><span class="sxs-lookup"><span data-stu-id="8f582-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="8f582-691">Comme dans ASP.NET MVC 3, jQuery et bibliothèques d’interface utilisateur jQuery sont également inclus dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8f582-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="8f582-692">Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="8f582-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="8f582-693">**Modernizr**: cette bibliothèque s’exécute automatiquement, rendre votre site compatible avec les navigateurs plus anciens lors de l’utilisation des technologies HTML5 et CSS3.</span><span class="sxs-lookup"><span data-stu-id="8f582-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="8f582-694">Vous pouvez obtenir plus d’informations sur la bibliothèque Modernizr dans ce lien : [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="8f582-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="8f582-695">**SimpleMembership inclus dans la solution**</span><span class="sxs-lookup"><span data-stu-id="8f582-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="8f582-696">SimpleMembership a été conçu comme un remplacement pour le système de fournisseur de rôle ASP.NET et l’appartenance au précédent.</span><span class="sxs-lookup"><span data-stu-id="8f582-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="8f582-697">Il possède de nombreuses nouvelles fonctionnalités qui facilitent pour les développeurs à sécuriser les pages web de manière plus flexible.</span><span class="sxs-lookup"><span data-stu-id="8f582-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="8f582-698">Le modèle Internet a déjà configuré quelques éléments à intégrer SimpleMembership, par exemple, le contrôle AccountController est prêt à utiliser la OAuthWebSecurity (pour l’inscription du compte OAuth, login, gestion, etc.) et la sécurité Web.</span><span class="sxs-lookup"><span data-stu-id="8f582-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="8f582-699">![SimpleMembership inclus dans la solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclus dans la solution")</span><span class="sxs-lookup"><span data-stu-id="8f582-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="8f582-700">*SimpleMembership inclus dans la solution*</span><span class="sxs-lookup"><span data-stu-id="8f582-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="8f582-701">Obtenir des informations supplémentaires [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) dans MSDN.</span><span class="sxs-lookup"><span data-stu-id="8f582-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="8f582-702">En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [publication annexe b : une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="8f582-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8f582-703">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="8f582-703">Summary</span></span>

<span data-ttu-id="8f582-704">En fin de cet atelier pratique, vous avez appris les notions de base d’ASP.NET MVC :</span><span class="sxs-lookup"><span data-stu-id="8f582-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="8f582-705">Les éléments principaux d’une application MVC et comment ils interagissent</span><span class="sxs-lookup"><span data-stu-id="8f582-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="8f582-706">Comment créer une Application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8f582-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="8f582-707">Comment ajouter et configurer des contrôleurs pour gérer les paramètres transmis à l’URL et la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="8f582-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="8f582-708">Comment ajouter une page maître de disposition pour configurer un modèle pour le contenu HTML commun, une feuille de style pour améliorer l’apparence et un modèle de vue pour afficher le contenu HTML</span><span class="sxs-lookup"><span data-stu-id="8f582-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="8f582-709">Comment utiliser le modèle ViewModel pour passer des propriétés pour le modèle de vue pour afficher des informations dynamiques</span><span class="sxs-lookup"><span data-stu-id="8f582-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="8f582-710">Comment utiliser les paramètres passés aux contrôleurs dans le modèle de vue</span><span class="sxs-lookup"><span data-stu-id="8f582-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="8f582-711">Comment ajouter des liens vers des pages à l’intérieur de l’application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8f582-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="8f582-712">Comment ajouter et utiliser des propriétés dynamiques dans une vue</span><span class="sxs-lookup"><span data-stu-id="8f582-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="8f582-713">Les améliorations dans les modèles de projet ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="8f582-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="8f582-714">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="8f582-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="8f582-715">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="8f582-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="8f582-716">Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="8f582-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="8f582-717">Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="8f582-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8f582-718">Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="8f582-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="8f582-719">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="8f582-719">Click on **Install Now**.</span></span> <span data-ttu-id="8f582-720">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.</span><span class="sxs-lookup"><span data-stu-id="8f582-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8f582-721">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="8f582-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8f582-722">![Installer Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="8f582-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="8f582-723">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="8f582-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="8f582-724">Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="8f582-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="8f582-726">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="8f582-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8f582-727">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="8f582-727">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="8f582-729">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="8f582-729">*Installation progress*</span></span>
6. <span data-ttu-id="8f582-730">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="8f582-730">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="8f582-732">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="8f582-732">*Installation completed*</span></span>
7. <span data-ttu-id="8f582-733">Cliquez sur **Exit** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="8f582-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="8f582-734">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="8f582-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour une vignette de Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="8f582-736">*VS Express pour une vignette de Web*</span><span class="sxs-lookup"><span data-stu-id="8f582-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8f582-737">Annexe b : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="8f582-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="8f582-738">Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et publiez l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8f582-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="8f582-739">Tâche 1 : création d’un nouveau Site Web à partir de Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8f582-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="8f582-740">Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="8f582-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-741">Avec Windows Azure, vous pouvez héberger 10 Sites Web ASP.NET gratuitement et faites ensuite évoluer que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="8f582-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="8f582-742">Vous pouvez vous inscrire [ici](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="8f582-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="8f582-743">![Ouvrez une session sur le portail Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "ouvrez une session sur le portail Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="8f582-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="8f582-744">*Ouvrez une session sur le portail de gestion Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="8f582-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="8f582-745">Cliquez sur **New** sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="8f582-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="8f582-746">![Création d’un Site Web](aspnet-mvc-4-fundamentals/_static/image49.png "création d’un Site Web")</span><span class="sxs-lookup"><span data-stu-id="8f582-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="8f582-747">*Création d’un Site Web*</span><span class="sxs-lookup"><span data-stu-id="8f582-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="8f582-748">Cliquez sur **calcul** | **Site Web**.</span><span class="sxs-lookup"><span data-stu-id="8f582-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="8f582-749">Puis sélectionnez **création rapide** option.</span><span class="sxs-lookup"><span data-stu-id="8f582-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="8f582-750">Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.</span><span class="sxs-lookup"><span data-stu-id="8f582-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-751">Un Site Web Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="8f582-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="8f582-752">L’option Création rapide vous permet de déployer une application web terminée pour le Site Web Windows Azure à partir en dehors du portail.</span><span class="sxs-lookup"><span data-stu-id="8f582-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="8f582-753">Il n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="8f582-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="8f582-754">![Création d’un Site Web à l’aide de la création rapide](aspnet-mvc-4-fundamentals/_static/image50.png "création d’un Site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="8f582-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="8f582-755">*Création d’un Site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="8f582-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="8f582-756">Attendez que la nouvelle **Site Web** est créé.</span><span class="sxs-lookup"><span data-stu-id="8f582-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="8f582-757">Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="8f582-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="8f582-758">Vérifiez que le nouveau Site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="8f582-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="8f582-759">![Navigation vers le nouveau site web](aspnet-mvc-4-fundamentals/_static/image51.png "exploration vers le nouveau site web")</span><span class="sxs-lookup"><span data-stu-id="8f582-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="8f582-760">*Navigation vers le nouveau site web*</span><span class="sxs-lookup"><span data-stu-id="8f582-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="8f582-761">![Site Web en cours d’exécution](aspnet-mvc-4-fundamentals/_static/image52.png "site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="8f582-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="8f582-762">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="8f582-762">*Web site running*</span></span>
6. <span data-ttu-id="8f582-763">Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="8f582-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="8f582-764">![Ouvrir les pages de gestion de site web](aspnet-mvc-4-fundamentals/_static/image53.png "ouvrir les pages de gestion de site web")</span><span class="sxs-lookup"><span data-stu-id="8f582-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="8f582-765">*Ouvrir les pages de gestion de Site Web*</span><span class="sxs-lookup"><span data-stu-id="8f582-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="8f582-766">Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.</span><span class="sxs-lookup"><span data-stu-id="8f582-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-767">Le *le profil de publication* contient toutes les informations requises pour publier une application web à un site Web Windows Azure pour chaque méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="8f582-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="8f582-768">Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="8f582-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="8f582-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8f582-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="8f582-770">![Téléchargement du site web le profil de publication](aspnet-mvc-4-fundamentals/_static/image54.png "téléchargement du site web le profil de publication")</span><span class="sxs-lookup"><span data-stu-id="8f582-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="8f582-771">*Téléchargement du Site Web le profil de publication*</span><span class="sxs-lookup"><span data-stu-id="8f582-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="8f582-772">Télécharger le fichier de profil de publication dans un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="8f582-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="8f582-773">Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8f582-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="8f582-774">![L’enregistrement du fichier de profil de publication](aspnet-mvc-4-fundamentals/_static/image55.png "l’enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="8f582-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="8f582-775">*L’enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="8f582-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="8f582-776">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="8f582-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="8f582-777">Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données.</span><span class="sxs-lookup"><span data-stu-id="8f582-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="8f582-778">Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="8f582-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="8f582-779">Vous devez un serveur de base de données SQL pour stocker la base de données d’application.</span><span class="sxs-lookup"><span data-stu-id="8f582-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="8f582-780">Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à l’adresse **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**.</span><span class="sxs-lookup"><span data-stu-id="8f582-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="8f582-781">Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="8f582-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="8f582-782">Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="8f582-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="8f582-783">Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="8f582-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="8f582-784">![Tableau de bord de serveur SQL Database](aspnet-mvc-4-fundamentals/_static/image56.png "tableau de bord serveur de base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="8f582-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="8f582-785">*Tableau de bord serveur de base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="8f582-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="8f582-786">Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**.</span><span class="sxs-lookup"><span data-stu-id="8f582-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="8f582-787">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) bouton.</span><span class="sxs-lookup"><span data-stu-id="8f582-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Ajout d’adresse IP du Client](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="8f582-789">*Ajout d’adresse IP du Client*</span><span class="sxs-lookup"><span data-stu-id="8f582-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="8f582-790">Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="8f582-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="8f582-792">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="8f582-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8f582-793">Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="8f582-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="8f582-794">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8f582-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="8f582-795">Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="8f582-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="8f582-796">![Publication de l’Application](aspnet-mvc-4-fundamentals/_static/image60.png "publication de l’Application")</span><span class="sxs-lookup"><span data-stu-id="8f582-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="8f582-797">*Publier le site web*</span><span class="sxs-lookup"><span data-stu-id="8f582-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="8f582-798">Importer le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="8f582-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="8f582-799">![L’importation du profil de publication](aspnet-mvc-4-fundamentals/_static/image61.png "l’importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="8f582-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="8f582-800">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="8f582-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="8f582-801">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="8f582-801">Click **Validate Connection**.</span></span> <span data-ttu-id="8f582-802">Une fois la Validation terminée. Cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="8f582-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8f582-803">La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="8f582-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="8f582-804">![Validation de la connexion](aspnet-mvc-4-fundamentals/_static/image62.png "validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="8f582-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="8f582-805">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="8f582-805">*Validating connection*</span></span>
4. <span data-ttu-id="8f582-806">Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="8f582-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="8f582-807">![Configuration de déploiement Web](aspnet-mvc-4-fundamentals/_static/image63.png "configuration de déploiement Web")</span><span class="sxs-lookup"><span data-stu-id="8f582-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="8f582-808">*Configuration de déploiement Web*</span><span class="sxs-lookup"><span data-stu-id="8f582-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="8f582-809">Configurez la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="8f582-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="8f582-810">Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.</span><span class="sxs-lookup"><span data-stu-id="8f582-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="8f582-811">Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="8f582-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="8f582-812">Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="8f582-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="8f582-813">Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="8f582-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="8f582-814">![Configuration de chaîne de connexion de destination](aspnet-mvc-4-fundamentals/_static/image64.png "configuration de chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="8f582-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="8f582-815">*Configuration de chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="8f582-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="8f582-816">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f582-816">Then click **OK**.</span></span> <span data-ttu-id="8f582-817">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="8f582-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="8f582-818">![Création de la base de données](aspnet-mvc-4-fundamentals/_static/image65.png "création de la chaîne de la base de données")</span><span class="sxs-lookup"><span data-stu-id="8f582-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="8f582-819">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="8f582-819">*Creating the database*</span></span>
7. <span data-ttu-id="8f582-820">La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte de la connexion par défaut.</span><span class="sxs-lookup"><span data-stu-id="8f582-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="8f582-821">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="8f582-821">Then click **Next**.</span></span>

    <span data-ttu-id="8f582-822">![Chaîne de connexion pointant vers la base de données SQL](aspnet-mvc-4-fundamentals/_static/image66.png "chaîne de connexion pointant vers la base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="8f582-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="8f582-823">*Chaîne de connexion pointant vers la base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="8f582-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="8f582-824">Dans le **aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="8f582-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="8f582-825">![Publication de l’application web](aspnet-mvc-4-fundamentals/_static/image67.png "publication de l’application web")</span><span class="sxs-lookup"><span data-stu-id="8f582-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="8f582-826">*Publication de l’application web*</span><span class="sxs-lookup"><span data-stu-id="8f582-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="8f582-827">Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.</span><span class="sxs-lookup"><span data-stu-id="8f582-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="8f582-828">![Application publiée dans Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application publiée dans Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="8f582-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="8f582-829">*Application soit publiée dans Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="8f582-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="8f582-830">Annexe c : l’utilisation des extraits de Code</span><span class="sxs-lookup"><span data-stu-id="8f582-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="8f582-831">Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="8f582-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="8f582-832">Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="8f582-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="8f582-833">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-fundamentals/_static/image69.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="8f582-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="8f582-834">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="8f582-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="8f582-835">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="8f582-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="8f582-836">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="8f582-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="8f582-837">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="8f582-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="8f582-838">Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="8f582-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="8f582-839">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).</span><span class="sxs-lookup"><span data-stu-id="8f582-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="8f582-840">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="8f582-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="8f582-841">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-fundamentals/_static/image70.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="8f582-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="8f582-842">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="8f582-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="8f582-843">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-fundamentals/_static/image71.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="8f582-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="8f582-844">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="8f582-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="8f582-845">![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-fundamentals/_static/image72.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="8f582-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="8f582-846">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="8f582-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="8f582-847">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="8f582-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="8f582-848">Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="8f582-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="8f582-849">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="8f582-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="8f582-850">Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="8f582-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="8f582-851">![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-fundamentals/_static/image73.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="8f582-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="8f582-852">*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="8f582-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="8f582-853">![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-fundamentals/_static/image74.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="8f582-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="8f582-854">*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="8f582-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
