---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Notions de base ASP.NET MVC 4 | Documents Microsoft
author: rick-anderson
description: Cet atelier pratique est basé sur le magasin de musique MVC (Model View Controller), une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MV...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: a0dd32280321938aba84a2aed5273d80750ed774
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="b3e8a-103">Notions de base ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b3e8a-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="b3e8a-104">Par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b3e8a-105">Télécharger Camps Web Kit de formation</span><span class="sxs-lookup"><span data-stu-id="b3e8a-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b3e8a-106">Cet atelier pratique est basé sur le magasin de musique MVC (Model View Controller), une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="b3e8a-107">Tout au long de l’atelier, vous allez apprendre la simplicité, encore d’alimentation de l’utilisation conjointe de ces technologies.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="b3e8a-108">Vous démarrez avec une application simple et générerez jusqu'à ce que vous avez une Application Web ASP.NET MVC 4 entièrement fonctionnelle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="b3e8a-109">Ce laboratoire fonctionne avec ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="b3e8a-110">Si vous souhaitez Explorer la version d’ASP.NET MVC 3 de l’application du didacticiel, vous pouvez le trouver dans [magasin de musique MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="b3e8a-111">Cet atelier pratique suppose que le développeur a une expérience dans les technologies de développement Web, tels que HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="b3e8a-112">Tous les exemples de code et des extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [Microsoft-Web/WebCampTrainingKit versions](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b3e8a-113">Le projet spécifique pour ce laboratoire est disponible à l’adresse [notions de base ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="b3e8a-114">L’application de magasin de musique</span><span class="sxs-lookup"><span data-stu-id="b3e8a-114">The Music Store application</span></span>

<span data-ttu-id="b3e8a-115">L’application web de magasin de musique qui est construite tout au long de cet atelier comprend trois parties principales : achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="b3e8a-116">Visiteurs sera en mesure de parcourir des albums par genre, ajouter des albums dans leur panier d’achat, passez en revue leur sélection et enfin passer à l’extraction pour se connecter et terminer la commande.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="b3e8a-117">En outre, les administrateurs de magasin seront en mesure de gérer les albums disponibles, ainsi que leurs propriétés principales.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="b3e8a-118">![Les écrans de magasin de musique](aspnet-mvc-4-fundamentals/_static/image1.png "écrans de magasin de musique")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="b3e8a-119">*Écrans de magasin de musique*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="b3e8a-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="b3e8a-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="b3e8a-121">Application de magasin de musique est générée à l’aide de **contrôleur MVC (Model View)**, un modèle d’architecture qui sépare une application en trois composants principaux :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="b3e8a-122">**Modèles**: objets de modèle sont les parties de l’application qui implémentent la logique de domaine.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="b3e8a-123">Souvent, les objets de modèle également récupèrent et de stocker l’état de modèle dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="b3e8a-124">**Affichages :** vues sont les composants qui affichent l’interface utilisateur de l’application (IU).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="b3e8a-125">En règle générale, cette interface utilisateur est créée à partir des données de modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="b3e8a-126">Un exemple serait la vue de modifier des Albums qui affiche des zones de texte et une liste déroulante, en fonction de l’état actuel d’un objet de l’Album.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="b3e8a-127">**Contrôleurs :** contrôleurs sont les composants qui gèrent l’interaction de l’utilisateur, manipulent le modèle et finalement sélectionnent une vue à restituer l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="b3e8a-128">Dans une application MVC, la vue affiche uniquement les informations ; le contrôleur gère et répond à la saisie de l’utilisateur et à l’interaction.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="b3e8a-129">Le modèle MVC vous aide à créer des applications qui séparent les différents aspects de l’application (logique d’entrée, logique métier et logique de l’interface utilisateur), tout en assurant un couplage lâche entre ces éléments.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="b3e8a-130">Cette séparation vous permet de gérer la complexité lorsque vous générez une application, car elle permet de vous concentrer sur l’un des aspects de l’implémentation à la fois.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="b3e8a-131">En outre, le modèle MVC facilite tester des applications, encourage également l’utilisation du développement piloté par test (TDD) pour créer des applications.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="b3e8a-132">Le **ASP.NET MVC** framework offre une alternative au modèle Web Forms ASP.NET pour la création d’applications Web basées sur ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="b3e8a-133">Le **ASP.NET MVC** est une infrastructure de présentation simple et facilement testable, qui (comme avec les applications Web forms) est intégrée aux fonctionnalités ASP.NET existantes, telles que les pages maîtres et basée sur l’appartenance authentification et vous offre toute la puissance de .NET core.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="b3e8a-134">Cela est utile si vous êtes déjà familiarisé avec ASP.NET Web Forms, car toutes les bibliothèques que vous utilisez déjà sont également disponibles dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="b3e8a-135">En outre, le couplage lâche entre les trois principaux composants d’une application MVC favorise également le développement parallèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="b3e8a-136">Par exemple, un développeur peut travailler sur la vue, un deuxième développeur peut travailler sur la logique du contrôleur et un troisième peut se concentrer sur la logique métier dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b3e8a-137">Objectifs</span><span class="sxs-lookup"><span data-stu-id="b3e8a-137">Objectives</span></span>

<span data-ttu-id="b3e8a-138">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b3e8a-139">Créer une application ASP.NET MVC à partir de rien en fonction du didacticiel de l’Application de magasin de musique</span><span class="sxs-lookup"><span data-stu-id="b3e8a-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="b3e8a-140">Ajouter des contrôleurs pour gérer des URL à la page d’accueil du site et ses principales fonctionnalités de navigation</span><span class="sxs-lookup"><span data-stu-id="b3e8a-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="b3e8a-141">Ajouter un affichage pour personnaliser le contenu affiché ainsi que son style</span><span class="sxs-lookup"><span data-stu-id="b3e8a-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="b3e8a-142">Ajouter des classes de modèle pour contenir et gérer la logique des données et de domaine</span><span class="sxs-lookup"><span data-stu-id="b3e8a-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="b3e8a-143">Utiliser un modèle de vue pour passer des informations à partir d’actions de contrôleur pour les modèles d’affichage</span><span class="sxs-lookup"><span data-stu-id="b3e8a-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="b3e8a-144">Explorer le nouveau modèle de ASP.NET MVC 4 pour les applications internet</span><span class="sxs-lookup"><span data-stu-id="b3e8a-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b3e8a-145">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b3e8a-145">Prerequisites</span></span>

<span data-ttu-id="b3e8a-146">Vous devez disposer des éléments suivants pour effectuer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b3e8a-147">[Visual Studio 2012 Express pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lecture [annexe A](#AppendixA) pour obtenir des instructions sur l’installation)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b3e8a-148">Installation</span><span class="sxs-lookup"><span data-stu-id="b3e8a-148">Setup</span></span>

<span data-ttu-id="b3e8a-149">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="b3e8a-150">Pour plus de commodité, une grande partie du code que vous gérez le long de cet atelier est disponible sous les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b3e8a-151">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b3e8a-152">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et vous souhaitez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide de l’annexe c :](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b3e8a-153">Exercices</span><span class="sxs-lookup"><span data-stu-id="b3e8a-153">Exercises</span></span>

<span data-ttu-id="b3e8a-154">Cet atelier pratique est constitué par les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b3e8a-155">Exercice 1 : Création de projet d’Application Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b3e8a-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="b3e8a-156">Exercice 2 : Création d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="b3e8a-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="b3e8a-157">Exercice 3 : Passer des paramètres à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="b3e8a-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="b3e8a-158">Exercice 4 : Création d’une vue</span><span class="sxs-lookup"><span data-stu-id="b3e8a-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="b3e8a-159">Exercice 5 : Création d’un modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="b3e8a-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="b3e8a-160">Exercice 6 : Utilisation de paramètres dans la vue</span><span class="sxs-lookup"><span data-stu-id="b3e8a-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="b3e8a-161">Exercice 7 : Une visite guidée nouveau modèle ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b3e8a-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="b3e8a-162">Chaque exercice est accompagné par un **fin** dossier qui contient la solution résultante, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b3e8a-163">Si vous avez besoin d’aide fonctionne via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="b3e8a-164">Durée estimée pour effectuer ce laboratoire : **60 minutes**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="b3e8a-165">Exercice 1 : Création de projet d’Application Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b3e8a-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="b3e8a-166">Dans cet exercice, vous allez apprendre comment créer une application ASP.NET MVC dans Visual Studio 2012 Express pour Web ainsi que son organisation dossier principal.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="b3e8a-167">En outre, vous allez apprendre à ajouter un nouveau contrôleur et le rendre afficher une chaîne simple dans la page d’accueil de l’application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="b3e8a-168">Tâche 1 - Création de projet d’Application Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b3e8a-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="b3e8a-169">Dans cette tâche, vous allez créer un projet d’application ASP.NET MVC vide à l’aide du modèle MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="b3e8a-170">Démarrer **Visual Studio Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b3e8a-171">Dans le menu **Fichier**, cliquez sur **Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="b3e8a-172">Dans le **nouveau projet** boîte de dialogue Sélectionnez le **ASP.NET MVC 4 Web Application** projet type, situé sous **Visual c#,** **Web** modèle liste.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="b3e8a-173">Modifier la **nom** à *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="b3e8a-174">Définir l’emplacement de la solution à l’intérieur d’un nouveau **commencer** dossier dans le dossier Source de cet exercice, par exemple **[votre-HOL-chemin d’accès] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="b3e8a-175">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-175">Click **OK**.</span></span>

    <span data-ttu-id="b3e8a-176">![Créer la boîte de dialogue Nouveau projet](aspnet-mvc-4-fundamentals/_static/image2.png "créer la boîte de dialogue Nouveau projet")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="b3e8a-177">*Créer la boîte de dialogue Nouveau projet*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="b3e8a-178">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue Sélectionnez le **base** modèle et assurez-vous que le **moteur d’affichage** sélectionné est **Razor**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="b3e8a-179">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-179">Click **OK**.</span></span>

    <span data-ttu-id="b3e8a-180">![Nouvelle boîte de dialogue projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "nouvelle boîte de dialogue projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="b3e8a-181">*Nouvelle boîte de dialogue projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="b3e8a-182">Tâche 2 : Explorer la Structure de la Solution</span><span class="sxs-lookup"><span data-stu-id="b3e8a-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="b3e8a-183">L’infrastructure ASP.NET MVC inclut un modèle de projet Visual Studio qui vous permet de créer des applications Web prenant en charge le modèle MVC.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="b3e8a-184">Ce modèle crée une nouvelle application Web ASP.NET MVC avec les dossiers requis, les modèles d’élément et les entrées de fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="b3e8a-185">Dans cette tâche, vous allez examiner la structure de la solution pour comprendre les éléments qui sont impliqués et leurs relations.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="b3e8a-186">Les dossiers suivants sont inclus dans toute l’application ASP.NET MVC, car l’infrastructure ASP.NET MVC par défaut utilise un &quot;convention avant la configuration&quot; approche et en fait des hypothèses par défaut basés sur le dossier d’affectation de noms conventions.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="b3e8a-187">Une fois que le projet est créé, passez en revue la structure de dossier qui a été créée dans l’Explorateur de solutions sur le côté droit :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="b3e8a-188">![Structure des dossiers de MVC ASP.NET dans l’Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC arborescence dans l’Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="b3e8a-189">*Structure des dossiers de MVC ASP.NET dans l’Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="b3e8a-190">**Contrôleurs**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-190">**Controllers**.</span></span> <span data-ttu-id="b3e8a-191">Ce dossier contient les classes de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="b3e8a-192">Dans une application en fonction de MVC, les contrôleurs sont responsables de la gestion des interactions de l’utilisateur final, manipuler le modèle et finalement en choisissant une vue à restituer l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="b3e8a-193">L’infrastructure MVC requiert les noms de tous les contrôleurs se terminent par &quot;contrôleur&quot;-par exemple, HomeController, LoginController ou ProductController.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="b3e8a-194">**Modèles**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-194">**Models**.</span></span> <span data-ttu-id="b3e8a-195">Ce dossier est fourni pour les classes qui représentent le modèle d’application pour l’application Web MVC.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="b3e8a-196">Cela inclut généralement le code qui définit les objets et la logique d’interaction avec le magasin de données.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="b3e8a-197">En règle générale, les objets de modèle réels seront dans les bibliothèques de classes séparées.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="b3e8a-198">Toutefois, lorsque vous créez une nouvelle application, vous pouvez inclure des classes et les déplacer dans les bibliothèques de classes séparées ultérieurement dans le cycle de développement.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="b3e8a-199">**Vues**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-199">**Views**.</span></span> <span data-ttu-id="b3e8a-200">Ce dossier est l’emplacement recommandé pour les vues, les composants responsables de l’affichage de l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="b3e8a-201">Vues utilisent des fichiers .aspx, .ascx, .cshtml et .master, en plus de tous les autres fichiers liés au rendu des vues.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="b3e8a-202">Dossier Views contient un dossier pour chaque contrôleur ; le dossier est nommé avec le préfixe du nom de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="b3e8a-203">Par exemple, si vous disposez d’un contrôleur nommé **HomeController**, le dossier Views contient un dossier appelé Home.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="b3e8a-204">Par défaut, lorsque l’infrastructure ASP.NET MVC charge une vue, il recherche un fichier .aspx avec le nom de vue requis dans le dossier Views\controllerName (**[nom du contrôleur] [Action] des vues .aspx**) ou (**vues [ControllerName] [Action] .cshtml**) pour les vues Razor.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b3e8a-205">Outre les dossiers indiqués précédemment, une application Web MVC utilise le **Global.asax** fichier pour définir le routage d’URL globales par défaut et elle utilise le **Web.config** fichier pour configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="b3e8a-206">Tâche 3 : ajout d’un HomeController</span><span class="sxs-lookup"><span data-stu-id="b3e8a-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="b3e8a-207">Dans les applications ASP.NET qui n’utilisent pas l’infrastructure MVC, l’intervention de l’utilisateur est organisée autour des pages et de déclenchement et la gestion des événements à partir de ces pages.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="b3e8a-208">En revanche, l’interaction utilisateur avec les applications ASP.NET MVC est organisée autour des contrôleurs et leurs méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="b3e8a-209">En revanche, infrastructure ASP.NET MVC mappe des URL aux classes qui sont référencés en tant que contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="b3e8a-210">Contrôleurs de traiter les demandes entrantes, gèrent les entrées d’utilisateur et les interactions, exécutent la logique d’application appropriée et de déterminer la réponse à envoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers une autre URL, un etc.).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="b3e8a-211">Dans le cas d’affichage HTML, une classe de contrôleur appelle généralement un composant de vue séparé afin de générer le balisage HTML de la demande.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="b3e8a-212">Dans une application MVC, la vue affiche uniquement les informations ; le contrôleur gère et répond à la saisie de l’utilisateur et à l’interaction.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="b3e8a-213">Dans cette tâche, vous allez ajouter une classe de contrôleur qui gérera les URL pour la page d’accueil du site de magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="b3e8a-214">Avec le bouton droit **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis **contrôleur** commande :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="b3e8a-215">![Ajouter une commande de contrôleur](aspnet-mvc-4-fundamentals/_static/image5.png "ajouter une commande de contrôleur")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="b3e8a-216">*Ajouter des commandes de contrôleur*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="b3e8a-217">Le **ajouter un contrôleur** boîte de dialogue s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="b3e8a-218">Nommez le contrôleur *HomeController* et appuyez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="b3e8a-219">![Ajouter la boîte de dialogue contrôleur](aspnet-mvc-4-fundamentals/_static/image6.png "contrôleur la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="b3e8a-220">*Boîte de dialogue contrôleur ajouter*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="b3e8a-221">Le fichier **HomeController.cs** est créé dans le **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="b3e8a-222">Pour disposer le **HomeController** retourner une chaîne sur son action d’Index, remplacez le **Index** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="b3e8a-223">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex1 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]
~~~

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b3e8a-224">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="b3e8a-225">Dans cette tâche, vous allez essayer de l’application dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="b3e8a-226">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="b3e8a-227">Le projet est compilé et démarre de serveur Web IIS local.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="b3e8a-228">Local IIS Web Server s’ouvre automatiquement un navigateur web pointant vers l’URL du serveur Web.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="b3e8a-229">![Application qui s’exécute dans un navigateur web](aspnet-mvc-4-fundamentals/_static/image7.png "Application qui s’exécute dans un navigateur web")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="b3e8a-230">*Application qui s’exécute dans un navigateur web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-231">Le serveur Web IIS local s’exécute le site Web sur un numéro de port libre aléatoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="b3e8a-232">Dans la figure ci-dessus, le site est en cours d’exécution à `http://localhost:50103/`, afin qu’il utilise le port 50103.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="b3e8a-233">Votre numéro de port peut varier.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-233">Your port number may vary.</span></span>
2. <span data-ttu-id="b3e8a-234">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="b3e8a-235">Exercice 2 : Création d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="b3e8a-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="b3e8a-236">Dans cet exercice, vous allez apprendre comment mettre à jour le contrôleur pour implémenter une fonctionnalité simple de l’application de magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="b3e8a-237">Ce contrôleur définit des méthodes d’action pour gérer chacun des requêtes spécifiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="b3e8a-238">Une page de liste des genres de musique dans le magasin de musique</span><span class="sxs-lookup"><span data-stu-id="b3e8a-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="b3e8a-239">Une page de navigation qui répertorie tous les albums de musique pour un genre particulier</span><span class="sxs-lookup"><span data-stu-id="b3e8a-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="b3e8a-240">Une page de détails qui affiche des informations sur un album de musique spécifique</span><span class="sxs-lookup"><span data-stu-id="b3e8a-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="b3e8a-241">Pour l’étendue de cet exercice, ces actions retourne simplement une chaîne à ce stade.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="b3e8a-242">Tâche 1 : ajout d’une nouvelle classe StoreController</span><span class="sxs-lookup"><span data-stu-id="b3e8a-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="b3e8a-243">Dans cette tâche, vous allez ajouter un nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="b3e8a-244">S’il est déjà ouvert, démarrez **Visual Studio Express pour Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="b3e8a-245">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b3e8a-246">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex02-CreatingAController\Begin**, sélectionnez **Begin.sln** et cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b3e8a-247">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b3e8a-248">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3e8a-249">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b3e8a-250">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b3e8a-251">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b3e8a-252">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3e8a-253">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3e8a-254">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b3e8a-255">Ajoutez le nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-255">Add the new controller.</span></span> <span data-ttu-id="b3e8a-256">Pour ce faire, cliquez sur le **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis le **contrôleur** commande.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="b3e8a-257">Modifier la **nom du contrôleur** à *StoreController*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="b3e8a-258">![Ajouter la boîte de dialogue contrôleur](aspnet-mvc-4-fundamentals/_static/image8.png "contrôleur la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="b3e8a-259">*Boîte de dialogue contrôleur ajouter*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="b3e8a-260">Tâche 2 : modification Actions de la StoreController</span><span class="sxs-lookup"><span data-stu-id="b3e8a-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="b3e8a-261">Dans cette tâche, vous allez modifier les méthodes de contrôleur sont appelés **actions**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="b3e8a-262">Actions sont responsables de la gestion des demandes d’URL et de déterminer le contenu qui doit être envoyé vers le navigateur ou l’utilisateur qui a appelé l’URL.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="b3e8a-263">Le **StoreController** classe a déjà un **Index** (méthode).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="b3e8a-264">Vous allez l’utiliser ultérieurement dans ce laboratoire pour implémenter la page qui répertorie tous les genres de la banque de musique.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="b3e8a-265">Pour l’instant, remplacez simplement la **Index** méthode avec le code suivant qui retourne une chaîne &quot;Hello de Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="b3e8a-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="b3e8a-266">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex2 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
~~~
2. <span data-ttu-id="b3e8a-267">Ajouter **Parcourir** et **détails** méthodes.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="b3e8a-268">Pour ce faire, ajoutez le code suivant à la **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="b3e8a-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="b3e8a-269">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b3e8a-270">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="b3e8a-271">Dans cette tâche, vous allez essayer de l’application dans un navigateur web.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="b3e8a-272">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3e8a-273">Le projet de démarrage le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="b3e8a-274">Modifiez l’URL pour vérifier la mise en œuvre de chaque action.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="b3e8a-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-275">**/Store**.</span></span> <span data-ttu-id="b3e8a-276">Vous verrez  **&quot;Hello de Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="b3e8a-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-277">**/Store/Browse**.</span></span> <span data-ttu-id="b3e8a-278">Vous verrez  **&quot;Hello de Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="b3e8a-279">**/ / Détails du magasin**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-279">**/Store/Details**.</span></span> <span data-ttu-id="b3e8a-280">Vous verrez  **&quot;Hello de Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="b3e8a-281">![Navigation StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de navigation")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="b3e8a-282">*Navigation /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="b3e8a-283">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="b3e8a-284">Exercice 3 : Passer des paramètres à un contrôleur</span><span class="sxs-lookup"><span data-stu-id="b3e8a-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="b3e8a-285">Jusqu'à présent, vous avez été retourner des chaînes de constantes à partir des contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="b3e8a-286">Dans cet exercice, vous allez apprendre à passer des paramètres à un contrôleur à l’aide de l’URL et la chaîne de requête, puis effectuez les actions de la méthode répondre avec du texte dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="b3e8a-287">Tâche 1 - Ajouter paramètre Genre StoreController</span><span class="sxs-lookup"><span data-stu-id="b3e8a-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="b3e8a-288">Dans cette tâche, vous allez utiliser le **querystring** pour envoyer des paramètres à la **Parcourir** méthode d’action dans le **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="b3e8a-289">S’il est déjà ouvert, démarrez **Visual Studio Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b3e8a-290">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b3e8a-291">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex03-PassingParametersToAController\Begin**, sélectionnez **Begin.sln** et cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b3e8a-292">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b3e8a-293">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3e8a-294">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b3e8a-295">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b3e8a-296">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b3e8a-297">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3e8a-298">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3e8a-299">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b3e8a-300">Ouvrez **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-300">Open **StoreController** class.</span></span> <span data-ttu-id="b3e8a-301">Pour ce faire, dans le **l’Explorateur de solutions**, développez le **contrôleurs** et double-cliquez sur **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="b3e8a-302">Modifier la **Parcourir** méthode, en ajoutant un paramètre de chaîne pour demander un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="b3e8a-303">ASP.NET MVC automatiquement passer toute chaîne de requête ou paramètres nommés de publication de formulaire **genre** à cette méthode d’action lorsqu’elle est appelée.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="b3e8a-304">Pour ce faire, remplacez le **Parcourir** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="b3e8a-305">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="b3e8a-306">Tâche 2 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-306">Task 2 - Running the Application</span></span>

<span data-ttu-id="b3e8a-307">Dans cette tâche, vous essayez l’Application dans un navigateur web et utiliser le **genre** paramètre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-307">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="b3e8a-308">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-308">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3e8a-309">Le projet de démarrage le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-309">The project starts in the **Home** page.</span></span> <span data-ttu-id="b3e8a-310">Modifier l’URL pour *, stocker, parcourir ? Genre = Disco* pour vérifier que l’action reçoit le paramètre de genre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-310">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="b3e8a-311">![Navigation StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "navigation StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="b3e8a-312">*Browsing /Store/Browse?Genre=Disco*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-312">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="b3e8a-313">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-313">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="b3e8a-314">Tâche 3 : ajout d’un paramètre d’Id incorporé dans l’URL</span><span class="sxs-lookup"><span data-stu-id="b3e8a-314">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="b3e8a-315">Dans cette tâche, vous allez utiliser le **URL** pour passer un **Id** paramètre à la **détails** méthode d’action de la **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-315">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="b3e8a-316">Convention de routage consiste à traiter le segment d’URL après le nom de la méthode d’action comme un paramètre nommé de la valeur par défaut de ASP.NET MVC **Id**. Si votre méthode d’action a un paramètre nommé Id, puis ASP.NET MVC sera automatiquement passer le segment d’URL pour vous en tant que paramètre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-316">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="b3e8a-317">Dans l’URL **magasin/détails/5**, **Id** sera interprété comme **5**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-317">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="b3e8a-318">Modifier la **détails** méthode de la **StoreController**, ajout un **int** paramètre appelé **id**. Pour ce faire, remplacez le **détails** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-318">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="b3e8a-319">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-319">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b3e8a-320">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-320">Task 4 - Running the Application</span></span>

<span data-ttu-id="b3e8a-321">Dans cette tâche, vous essayez l’Application dans un navigateur web et utiliser le **Id** paramètre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-321">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="b3e8a-322">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-322">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3e8a-323">Le projet de démarrage le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-323">The project starts in the **Home** page.</span></span> <span data-ttu-id="b3e8a-324">Modifier l’URL pour */Store/Details/5* pour vérifier que l’action reçoit le paramètre id.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-324">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="b3e8a-325">![Navigation StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de navigation")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-325">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="b3e8a-326">*Navigation /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-326">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="b3e8a-327">Exercice 4 : Création d’une vue</span><span class="sxs-lookup"><span data-stu-id="b3e8a-327">Exercise 4: Creating a View</span></span>

<span data-ttu-id="b3e8a-328">Jusqu'à présent vous avez été retournant des chaînes à partir d’actions de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-328">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="b3e8a-329">C’est un moyen utile de comprendre le fonctionnement des contrôleurs, mais il n'est pas comment vos applications Web réelles sont générées.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-329">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="b3e8a-330">Les vues sont des composants qui fournissent une meilleure approche pour générer le code HTML au navigateur avec l’utilisation de fichiers de modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-330">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="b3e8a-331">Dans cet exercice, vous allez apprendre à ajouter une page maître de mise en page pour configurer un modèle pour le contenu HTML commun, une feuille de style pour améliorer l’apparence du site et, enfin, un modèle d’affichage pour permettre le HomeController retourner le code HTML.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-331">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="b3e8a-332">Tâche 1 : modification du fichier \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="b3e8a-332">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="b3e8a-333">Le fichier **~/Views/Shared/\_layout.cshtml** vous permet de configurer un modèle pour le code HTML commun à utiliser dans tout le site Web.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-333">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="b3e8a-334">Dans cette tâche, vous allez ajouter une page maître de mise en page avec un en-tête commun avec des liens vers la zone de page d’accueil et le magasin.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-334">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="b3e8a-335">S’il est déjà ouvert, démarrez **Visual Studio Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-335">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b3e8a-336">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-336">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b3e8a-337">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex04-CreatingAView\Begin**, sélectionnez **Begin.sln** et cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-337">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b3e8a-338">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-338">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b3e8a-339">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3e8a-340">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b3e8a-341">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b3e8a-342">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b3e8a-343">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3e8a-344">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3e8a-345">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b3e8a-346">Le fichier  <strong>\_layout.cshtml</strong> contient la structure de conteneur HTML pour toutes les pages sur le site.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-346">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="b3e8a-347">Il inclut le <strong>&lt;html&gt;</strong> , élément pour la réponse HTML, ainsi que le <strong>&lt;head&gt;</strong> et <strong>&lt;corps&gt;</strong> éléments.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-347">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="b3e8a-348"><strong>@RenderBody()</strong> dans le code HTML corps identifier les zones cette vue modèles seront en mesure de remplir avec le contenu dynamique.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-348"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="b3e8a-349">(C#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-349">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="b3e8a-350">Ajouter un en-tête commun avec des liens vers la zone de page d’accueil et magasin sur toutes les pages du site.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-350">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="b3e8a-351">Pour ce faire, ajoutez le code suivant ci-dessous &lt;corps&gt; instruction.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-351">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="b3e8a-352">(C#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-352">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="b3e8a-353">Inclure un élément div pour restituer la section de corps de chaque page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-353">Include a div to render the body section of each page.</span></span> <span data-ttu-id="b3e8a-354">Remplacez  <strong>@RenderBody()</strong> avec le code higlighted suivant : (c#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-354">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="b3e8a-355">Saviez-vous ?</span><span class="sxs-lookup"><span data-stu-id="b3e8a-355">Did you know?</span></span> <span data-ttu-id="b3e8a-356">Visual Studio 2012 offre des extraits de code qui la rendent facile d’ajouter du code utilisé couramment dans HTML, les fichiers de code et bien plus encore !</span><span class="sxs-lookup"><span data-stu-id="b3e8a-356">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="b3e8a-357">Essayez en tapant **&lt;div&gt;** et en appuyant sur **onglet** à deux reprises pour insérer un **div** balise.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-357">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="b3e8a-358">Tâche 2 - ajouter feuille de style CSS</span><span class="sxs-lookup"><span data-stu-id="b3e8a-358">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="b3e8a-359">Le modèle de projet vide inclut un fichier CSS très simplifié qui inclut uniquement les styles utilisés pour afficher des formulaires de base et les messages de validation.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-359">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="b3e8a-360">Vous allez utiliser CSS et des images (potentiellement fournis par un concepteur) supplémentaires afin d’améliorer l’apparence du site.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-360">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="b3e8a-361">Dans cette tâche, vous allez ajouter une feuille de style CSS pour définir les styles du site.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-361">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="b3e8a-362">Le fichier CSS et les images sont inclus dans le **Source\Assets\Content** dossier de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-362">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="b3e8a-363">Afin de les ajouter à l’application, faites glisser son contenu à partir d’un **l’Explorateur Windows** fenêtre dans le **l’Explorateur de solutions** dans Visual Studio Express pour le Web, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-363">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="b3e8a-364">![En faisant glisser le contenu de style](aspnet-mvc-4-fundamentals/_static/image12.png "en faisant glisser le contenu de style")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-364">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="b3e8a-365">*En faisant glisser le contenu de style*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-365">*Dragging style contents*</span></span>
2. <span data-ttu-id="b3e8a-366">Une boîte de dialogue d’avertissement s’affiche, vous demandant de confirmer le remplacement **Site.css** fichier et des images existantes.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-366">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="b3e8a-367">Vérifiez **appliquer à tous les éléments** et cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-367">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="b3e8a-368">Tâche 3 : ajout d’un modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="b3e8a-368">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="b3e8a-369">Dans cette tâche, vous allez ajouter un modèle d’affichage pour générer la réponse HTML qui utilise la page maître de mise en page et CSS ajoutés dans cet exercice.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-369">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="b3e8a-370">Pour utiliser un modèle d’affichage lorsque vous parcourez la page d’accueil du site, vous devez d’abord indiquer qu’au lieu de retourner une chaîne, le **HomeController Index** méthode retournera un **vue**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-370">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="b3e8a-371">Ouvrez **HomeController** classe et modifier son **Index** méthode pour retourner un **ActionResult**, et qu’il retourne **View()**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-371">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="b3e8a-372">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex4 HomeController Index*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-372">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
~~~
2. <span data-ttu-id="b3e8a-373">Maintenant, vous devez ajouter un modèle d’affichage approprié.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-373">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="b3e8a-374">Pour ce faire, **avec le bouton droit** à l’intérieur de la **Index** méthode d’action et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-374">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="b3e8a-375">Cela affiche la **ajouter une vue** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-375">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="b3e8a-376">![Ajout d’une vue à partir de la méthode Index](aspnet-mvc-4-fundamentals/_static/image13.png "Ajout d’une vue à partir de l’Index (méthode)")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-376">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="b3e8a-377">*Ajout d’une vue à partir de l’Index (méthode)*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-377">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="b3e8a-378">Le **ajouter une vue** boîte de dialogue apparaît pour générer un fichier de modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-378">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="b3e8a-379">Par défaut, cette boîte de dialogue remplit au préalable le nom du modèle d’affichage afin qu’elle corresponde à la méthode d’action qui l’utilise.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-379">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="b3e8a-380">Étant donné que vous avez utilisé le **ajouter une vue** menu contextuel dans le **Index** méthode d’action dans le HomeController, le **ajouter une vue** boîte de dialogue comporte des Index en tant que le nom de la vue par défaut.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-380">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="b3e8a-381">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-381">Click **Add**.</span></span>

    <span data-ttu-id="b3e8a-382">![Afficher la boîte de dialogue Ajouter](aspnet-mvc-4-fundamentals/_static/image14.png "afficher la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-382">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="b3e8a-383">*Afficher la boîte de dialogue Ajouter*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-383">*Add View Dialog*</span></span>
4. <span data-ttu-id="b3e8a-384">Visual Studio génère une **Index.cshtml** afficher le modèle à l’intérieur de la **Views\Home** dossier, puis l’ouvre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-384">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="b3e8a-385">![Vue d’Index créé d’accueil](aspnet-mvc-4-fundamentals/_static/image15.png "vue accueil Index créé")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-385">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="b3e8a-386">*Accueil vue Index créé*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-386">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-387">nom et l’emplacement de la **Index.cshtml** fichier s’applique et suit les conventions d’affectation de noms par défaut ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-387">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="b3e8a-388">Le dossier \Views\**accueil** correspond au nom du contrôleur (**accueil** contrôleur).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-388">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="b3e8a-389">Le nom de modèle de vue (**Index**), correspond à la méthode d’action de contrôleur qui affichera la vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-389">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="b3e8a-390">De cette manière, ASP.NET MVC évite d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle d’affichage lors de l’utilisation de cette convention d’affectation de noms pour retourner une vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-390">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="b3e8a-391">Le modèle d’affichage généré est basé sur le  **\_layout.cshtml** modèle défini précédemment.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-391">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="b3e8a-392">Mettre à jour la propriété ViewBag.Title **accueil**et modifier le contenu principal à **la page d’accueil**, comme illustré dans le code ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-392">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


~~~
[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
~~~
6. <span data-ttu-id="b3e8a-393">Sélectionnez **MvcMusicStore** projet dans l’Explorateur de solutions et appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-393">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="b3e8a-394">Tâche 4 : vérification</span><span class="sxs-lookup"><span data-stu-id="b3e8a-394">Task 4: Verification</span></span>

<span data-ttu-id="b3e8a-395">Afin de vérifier que vous avez exécuté correctement toutes les étapes décrites dans l’exercice précédent, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-395">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="b3e8a-396">Avec l’application s’ouvre dans un navigateur, notez que :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-396">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="b3e8a-397">Méthode d’action du HomeController Index trouvée et affiche le **\Views\Home\Index.cshtml** afficher le modèle, même si le code appelé **retourner View()**, car le modèle d’affichage suivi le convention d’affectation de noms standard.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-397">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="b3e8a-398">La Page d’accueil affiche le message d’accueil défini dans le **\Views\Home\Index.cshtml** afficher le modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-398">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="b3e8a-399">À l’aide de la Page d’accueil est la  **\_layout.cshtml** modèle, et par conséquent, le message d’accueil est contenu dans la disposition de HTML du site standard.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-399">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="b3e8a-400">![Vue d’Index à l’aide de la LayoutPage défini et le style d’accueil](aspnet-mvc-4-fundamentals/_static/image16.png "accueil vue d’Index à l’aide de la LayoutPage défini et le style")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-400">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="b3e8a-401">*Accueil vue d’Index à l’aide de la LayoutPage défini et le style*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-401">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="b3e8a-402">Exercice 5 : Création d’un modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="b3e8a-402">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="b3e8a-403">Jusqu'à présent, vous avez apportées à vos vues afficher codé en dur HTML, mais, pour créer des applications web dynamiques, le modèle d’affichage doit recevoir des informations à partir du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-403">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="b3e8a-404">Une technique courante pour être utilisée à cet effet est le **ViewModel** modèle, qui permet à un contrôleur créer un package de toutes les informations nécessaires pour générer la réponse HTML appropriée.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-404">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="b3e8a-405">Dans cet exercice, vous allez apprendre à créer une classe ViewModel et ajoutez les propriétés requises : le nombre de genres dans le magasin et une liste de ces genres.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-405">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="b3e8a-406">Vous met également à jour le StoreController pour utiliser le ViewModel créé, et enfin, vous allez créer un nouveau modèle de vue qui affiche les propriétés mentionnées dans la page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-406">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="b3e8a-407">Tâche 1 - Création d’une classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="b3e8a-407">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="b3e8a-408">Dans cette tâche, vous allez créer une classe ViewModel chargé d’implémenter le scénario de liste de genre de magasin.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-408">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="b3e8a-409">S’il est déjà ouvert, démarrez **Visual Studio Express pour le Web**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-409">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b3e8a-410">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-410">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b3e8a-411">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex05-CreatingAViewModel\Begin**, sélectionnez **Begin.sln** et cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-411">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b3e8a-412">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-412">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b3e8a-413">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-413">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3e8a-414">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-414">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b3e8a-415">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-415">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b3e8a-416">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-416">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b3e8a-417">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-417">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3e8a-418">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-418">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3e8a-419">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-419">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b3e8a-420">Créer un **ViewModel** dossier où stocker le ViewModel.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-420">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="b3e8a-421">Pour ce faire, cliquez sur le niveau supérieur **MvcMusicStore** projet, sélectionnez **ajouter** , puis **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-421">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="b3e8a-422">![Ajout d’un nouveau dossier](aspnet-mvc-4-fundamentals/_static/image17.png "Ajout d’un nouveau dossier")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-422">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="b3e8a-423">*Ajout d’un nouveau dossier*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-423">*Adding a new folder*</span></span>
4. <span data-ttu-id="b3e8a-424">Nommez le dossier *ViewModel*.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-424">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="b3e8a-425">![Dossier ViewModel dans l’Explorateur de solutions](aspnet-mvc-4-fundamentals/_static/image18.png "dossier ViewModel dans l’Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-425">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="b3e8a-426">*Dossier ViewModel dans l’Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-426">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="b3e8a-427">Créer un **ViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-427">Create a **ViewModel** class.</span></span> <span data-ttu-id="b3e8a-428">Pour ce faire, cliquez sur le **ViewModel** dossier créé récemment, sélectionnez **ajouter** , puis **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-428">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="b3e8a-429">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *StoreIndexViewModel.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-429">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="b3e8a-430">![Ajout d’une nouvelle classe](aspnet-mvc-4-fundamentals/_static/image19.png "ajoutant une nouvelle classe")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-430">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="b3e8a-431">*Ajout d’une nouvelle classe*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-431">*Adding a new Class*</span></span>

    <span data-ttu-id="b3e8a-432">![Création de classe de StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel de création de classe")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-432">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="b3e8a-433">*Création de classe de StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-433">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="b3e8a-434">Tâche 2 : ajout de propriétés à la classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="b3e8a-434">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="b3e8a-435">Il existe deux paramètres à passer à partir de la StoreController pour le modèle d’affichage afin de générer la réponse HTML attendue : le nombre de genres dans le magasin et une liste de ces genres.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-435">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="b3e8a-436">Dans cette tâche, vous allez ajouter ces 2 propriétés à la **StoreIndexViewModel** classe : **NumberOfGenres** (entier) et **Genres** (il s’agit d’une liste de chaînes).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-436">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="b3e8a-437">Ajouter **NumberOfGenres** et **Genres** propriétés pour le **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-437">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="b3e8a-438">Pour ce faire, ajoutez les 2 lignes suivantes à la définition de classe :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-438">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="b3e8a-439">(Code d’extrait de code - *ASP.NET MVC 4 notions de base - Ex5 StoreIndexViewModel propriétés*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-439">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature. It provides the benefits of a property without requiring us to declare a backing field.
~~~

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="b3e8a-440">Tâche 3 - StoreController mise à jour pour utiliser le StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="b3e8a-440">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="b3e8a-441">Le **StoreIndexViewModel** classe encapsule les informations nécessaires pour passer de **StoreController**de **Index** à un modèle d’affichage afin de générer une réponse (méthode) .</span><span class="sxs-lookup"><span data-stu-id="b3e8a-441">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="b3e8a-442">Dans cette tâche, vous mettrez à jour la **StoreController** à utiliser le **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-442">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="b3e8a-443">Ouvrez **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-443">Open **StoreController** class.</span></span>

    <span data-ttu-id="b3e8a-444">![Ouverture de la classe de StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "ouverture StoreController classe")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-444">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="b3e8a-445">*Classe de StoreController ouverture*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-445">*Opening StoreController class*</span></span>
2. <span data-ttu-id="b3e8a-446">Pour pouvoir utiliser le **StoreIndexViewModel** classe à partir de la **StoreController**, ajouter l’espace de noms suivante en haut de la **StoreController** code :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-446">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="b3e8a-447">(Code d’extrait de code - *ASP.NET MVC 4 notions de base - StoreIndexViewModel Ex5 à l’aide de ViewModel*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-447">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
~~~
3. <span data-ttu-id="b3e8a-448">Modifier la **StoreController**de **Index** méthode d’action afin qu’il crée et remplit un **StoreIndexViewModel** de l’objet et le transmet à un modèle d’affichage pour génère une réponse HTML avec lui.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-448">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-449">Dans le laboratoire &quot;accès aux données et les modèles ASP.NET MVC&quot; vous allez écrire du code qui Récupère la liste des genres de magasin à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-449">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="b3e8a-450">Dans le code suivant, vous allez créer un **liste** de genres de données fictives rempliront la **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-450">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="b3e8a-451">Après la création et la configuration de la **StoreIndexViewModel** de l’objet, il sera passé comme argument à la **vue** (méthode).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-451">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="b3e8a-452">Cela indique que le modèle d’affichage utilisera cet objet pour générer une réponse HTML avec lui.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-452">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="b3e8a-453">Remplacez le **Index** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-453">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="b3e8a-454">(Code d’extrait de code - *ASP.NET MVC 4 notions de base - méthode Ex5 StoreController Index*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-454">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound. That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**. Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.
~~~

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="b3e8a-455">Tâche 4 : création d’un modèle de vue qui utilise StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="b3e8a-455">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="b3e8a-456">Dans cette tâche, vous allez créer un modèle d’affichage qui utilise un objet StoreIndexViewModel passé à partir du contrôleur pour afficher la liste des styles.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-456">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="b3e8a-457">Avant de créer le nouveau modèle d’affichage, nous allons générer le projet afin que les **boîte de dialogue Vue ajouter** connaît le **StoreIndexViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-457">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="b3e8a-458">Générez le projet en sélectionnant le **générer** élément de menu, puis **MvcMusicStore de Build**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-458">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="b3e8a-459">![Génération du projet](aspnet-mvc-4-fundamentals/_static/image22.png "génération du projet")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-459">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="b3e8a-460">*Génération du projet*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-460">*Building the project*</span></span>
2. <span data-ttu-id="b3e8a-461">Créer un nouveau modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-461">Create a new View template.</span></span> <span data-ttu-id="b3e8a-462">Pour ce faire, cliquez à l’intérieur de la **Index** (méthode) et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-462">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="b3e8a-463">![Ajout d’une vue](aspnet-mvc-4-fundamentals/_static/image23.png "Ajout d’une vue")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-463">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="b3e8a-464">*Ajout d’une vue*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-464">*Adding a View*</span></span>
3. <span data-ttu-id="b3e8a-465">Étant donné que la **boîte de dialogue Vue ajouter** a été appelée à partir de la **StoreController**, il ajoute le modèle d’affichage par défaut dans un **\Views\Store\Index.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-465">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="b3e8a-466">Vérifiez le **créer une fortement--vue typée** case à cocher, puis sélectionnez **StoreIndexViewModel** comme le **classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-466">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="b3e8a-467">En outre, assurez-vous que le moteur d’affichage sélectionné est **Razor**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-467">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="b3e8a-468">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-468">Click **Add**.</span></span>

    <span data-ttu-id="b3e8a-469">![Afficher la boîte de dialogue Ajouter](aspnet-mvc-4-fundamentals/_static/image24.png "afficher la boîte de dialogue Ajouter")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-469">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="b3e8a-470">*Afficher la boîte de dialogue Ajouter*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-470">*Add View Dialog*</span></span>

    <span data-ttu-id="b3e8a-471">Le **\Views\Store\Index.cshtml** afficher le fichier de modèle est créé et ouvert.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-471">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="b3e8a-472">Selon les informations fournies pour le **ajouter une vue** boîte de dialogue de la dernière étape, la vue de modèle s’attend à recevoir un **StoreIndexViewModel** instance en tant que les données à utiliser pour générer une réponse HTML.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-472">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="b3e8a-473">Vous remarquerez que le modèle hérite d’un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en c#.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-473">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="b3e8a-474">Tâche 5 : mise à jour le modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="b3e8a-474">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="b3e8a-475">Dans cette tâche, vous mettrez à jour le modèle d’affichage créé dans la dernière tâche pour récupérer le nombre de genres et leurs noms dans la page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-475">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="b3e8a-476">Vous allez utiliser @ syntaxe (souvent appelé &quot;pépites de code&quot;) pour exécuter du code dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-476">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="b3e8a-477">Dans le **Index.cshtml** de fichiers, dans le **magasin** dossier, remplacez son code par les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-477">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


~~~
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
~~~
2. <span data-ttu-id="b3e8a-478">Boucle sur la liste genre **StoreIndexViewModel** et créer un élément HTML **&lt;ul&gt;** à l’aide de la liste un **foreach** boucle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-478">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="b3e8a-479">(C#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-479">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="b3e8a-480">Appuyez sur **F5** pour exécuter l’Application et de parcourir **/stockages**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-480">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="b3e8a-481">Vous verrez la liste des genres passé dans le **StoreIndexViewModel** de l’objet à partir de la **StoreController** pour le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-481">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="b3e8a-482">![Vue qui affiche la liste des genres](aspnet-mvc-4-fundamentals/_static/image26.png "affichant une liste de genres")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-482">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="b3e8a-483">*Affiche la liste des genres de vue*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-483">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="b3e8a-484">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-484">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="b3e8a-485">Exercice 6 : Utilisation de paramètres dans la vue</span><span class="sxs-lookup"><span data-stu-id="b3e8a-485">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="b3e8a-486">Dans l’exercice 3, vous avez appris comment passer des paramètres pour le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-486">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="b3e8a-487">Dans cet exercice, vous allez apprendre comment utiliser ces paramètres dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-487">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="b3e8a-488">Pour cela, vous seront introduites tout d’abord aux classes de modèle qui vous aideront à gérer votre logique de données et le domaine.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-488">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="b3e8a-489">En outre, vous allez apprendre comment créer des liens vers les pages à l’intérieur de l’application ASP.NET MVC préoccuper des éléments comme les chemins d’accès de l’URL d’encodage.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-489">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="b3e8a-490">Tâche 1 : ajout de Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="b3e8a-490">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="b3e8a-491">Contrairement aux ViewModel, qui est créés uniquement pour passer des informations à partir du contrôleur à la vue, les classes de modèle sont créées pour contenir et gérer la logique des données et de domaine.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-491">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="b3e8a-492">Dans cette tâche, vous allez ajouter deux classes de modèle pour représenter ces concepts : **Genre** et **Album**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-492">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="b3e8a-493">S’il est déjà ouvert, démarrez **Visual Studio Express pour le Web**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-493">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="b3e8a-494">Dans le **fichier** menu, choisissez **ouvrir le projet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-494">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b3e8a-495">Dans la boîte de dialogue Ouvrir un projet, accédez à **Source\Ex06-UsingParametersInView\Begin**, sélectionnez **Begin.sln** et cliquez sur **ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-495">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b3e8a-496">Ou bien, vous pouvez continuer avec la solution que vous avez obtenu à l’issue de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-496">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b3e8a-497">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-497">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b3e8a-498">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-498">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b3e8a-499">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-499">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b3e8a-500">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-500">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b3e8a-501">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-501">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b3e8a-502">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-502">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b3e8a-503">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-503">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b3e8a-504">Ajouter un **Genre** classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-504">Add a **Genre** Model class.</span></span> <span data-ttu-id="b3e8a-505">Pour ce faire, cliquez sur le **modèles** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-505">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="b3e8a-506">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *Genre.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-506">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="b3e8a-507">![Ajout d’une classe](aspnet-mvc-4-fundamentals/_static/image27.png "Ajout d’une classe")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-507">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="b3e8a-508">*Ajouter un nouvel élément*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-508">*Adding a new item*</span></span>

    <span data-ttu-id="b3e8a-509">![Ajouter une classe de modèle de Genre](aspnet-mvc-4-fundamentals/_static/image28.png "ajouter une classe de modèle de Genre")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-509">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="b3e8a-510">*Ajouter une classe de modèle de Genre*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-510">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="b3e8a-511">Ajouter un **nom** propriété à la classe de Genre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-511">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="b3e8a-512">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-512">To do this, add the following code:</span></span>

    <span data-ttu-id="b3e8a-513">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-513">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
~~~
5. <span data-ttu-id="b3e8a-514">Suivant la même procédure que précédemment, ajoutez un **Album** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-514">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="b3e8a-515">Pour ce faire, cliquez sur le **modèles** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-515">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="b3e8a-516">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *Album.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-516">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="b3e8a-517">Ajouter deux propriétés à la classe Album : **Genre** et **titre**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-517">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="b3e8a-518">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-518">To do this, add the following code:</span></span>

    <span data-ttu-id="b3e8a-519">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-519">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]
~~~

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="b3e8a-520">Tâche 2 : ajout d’un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="b3e8a-520">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="b3e8a-521">A **StoreBrowseViewModel** sera utilisé dans cette tâche pour afficher les Albums qui correspondent à un Genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-521">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="b3e8a-522">Dans cette tâche, vous créez cette classe et puis ajoutez les deux propriétés pour gérer la **Genre** et son **Album**de liste.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-522">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="b3e8a-523">Ajouter un **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-523">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="b3e8a-524">Pour ce faire, cliquez sur le **ViewModel** dossier dans le **l’Explorateur de solutions**, sélectionnez **ajouter** , puis le **un nouvel élément** option.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-524">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="b3e8a-525">Sous **Code**, choisissez le **classe** d’élément et nommez le fichier *StoreBrowseViewModel.cs*, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-525">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="b3e8a-526">Ajoutez une référence aux modèles de **StoreBrowseViewModel** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-526">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="b3e8a-527">Pour ce faire, ajoutez le code suivant à l’aide d’espace de noms :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-527">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="b3e8a-528">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-528">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
~~~
3. <span data-ttu-id="b3e8a-529">Ajoutez les deux propriétés à **StoreBrowseViewModel** classe : **Genre** et **Albums**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-529">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="b3e8a-530">Pour ce faire, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-530">To do this, add the following code:</span></span>

    <span data-ttu-id="b3e8a-531">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-531">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).
> 
> This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.
> 
> **List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace. One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.
~~~

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="b3e8a-532">Tâche 3 : à l’aide de la nouvelle ViewModel dans le StoreController</span><span class="sxs-lookup"><span data-stu-id="b3e8a-532">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="b3e8a-533">Dans cette tâche, vous allez modifier le **StoreController**de **Parcourir** et **détails** méthodes d’action pour utiliser le nouveau **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="b3e8a-533">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="b3e8a-534">Ajoutez une référence dans le dossier de modèles dans **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-534">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="b3e8a-535">Pour ce faire, développez le **contrôleurs** dossier dans le **l’Explorateur de solutions** et ouvrez le **StoreController** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-535">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="b3e8a-536">Ensuite, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-536">Then add the following code:</span></span>

    <span data-ttu-id="b3e8a-537">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-537">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
~~~
2. <span data-ttu-id="b3e8a-538">Remplacez le **Parcourir** méthode d’action à utiliser le **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-538">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="b3e8a-539">Vous allez créer un Genre et deux nouveaux objets Albums avec des données factices (dans l’atelier pratique suivant vous consommera données réelles provenant d’une base de données).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-539">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="b3e8a-540">Pour ce faire, remplacez le **Parcourir** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-540">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="b3e8a-541">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-541">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
~~~
3. <span data-ttu-id="b3e8a-542">Remplacez le **détails** méthode d’action à utiliser le **StoreViewBrowseController** classe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-542">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="b3e8a-543">Vous allez créer un nouveau **Album** objet à retourner à la **vue**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-543">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="b3e8a-544">Pour ce faire, remplacez le **détails** méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-544">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="b3e8a-545">(Code d’extrait de code - *notions de base ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-545">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]
~~~

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="b3e8a-546">Tâche 4 : ajout d’un modèle de vue Parcourir</span><span class="sxs-lookup"><span data-stu-id="b3e8a-546">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="b3e8a-547">Dans cette tâche, vous allez ajouter un **Parcourir** pour afficher les Albums trouvées pour un Genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-547">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="b3e8a-548">Avant de créer le nouveau modèle d’affichage, vous devez générer le projet afin que les **ajouter une vue** boîte de dialogue connaît le **ViewModel** classe à utiliser.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-548">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="b3e8a-549">Générez le projet en sélectionnant le **générer** élément de menu, puis **MvcMusicStore de Build**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-549">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="b3e8a-550">Ajouter un **Parcourir** vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-550">Add a **Browse** View.</span></span> <span data-ttu-id="b3e8a-551">Pour ce faire, cliquez dans le **Parcourir** méthode d’action de la **StoreController** et cliquez sur **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-551">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="b3e8a-552">Dans le **ajouter une vue** boîte de dialogue, vérifiez que le nom de la vue est **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-552">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="b3e8a-553">Vérifiez le **créer une vue fortement typée** case à cocher et sélectionnez **StoreBrowseViewModel** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-553">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="b3e8a-554">Laissez les autres champs avec leur valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-554">Leave the other fields with their default value.</span></span> <span data-ttu-id="b3e8a-555">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-555">Then click **Add**.</span></span>

    <span data-ttu-id="b3e8a-556">![Ajout d’une vue Parcourir](aspnet-mvc-4-fundamentals/_static/image29.png "Ajout d’une vue Parcourir")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-556">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="b3e8a-557">*Ajout d’une vue Parcourir*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-557">*Adding a Browse View*</span></span>
4. <span data-ttu-id="b3e8a-558">Modifier la **Browse.cshtml** pour afficher des informations du Genre l’accès à la **StoreBrowseViewModel** objet passé au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-558">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="b3e8a-559">Pour ce faire, remplacez le contenu par les éléments suivants : (c#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-559">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b3e8a-560">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-560">Task 5 - Running the Application</span></span>

<span data-ttu-id="b3e8a-561">Dans cette tâche, vous allez tester que le **Parcourir** méthode récupère des Albums à partir de la **Parcourir** action de méthode.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-561">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="b3e8a-562">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-562">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3e8a-563">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-563">The project starts in the Home page.</span></span> <span data-ttu-id="b3e8a-564">Modifier l’URL pour **, stocker, parcourir ? Genre = Disco** pour vérifier que l’action retourne deux Albums.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-564">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="b3e8a-565">![Exploration des Albums Disco de magasin](aspnet-mvc-4-fundamentals/_static/image30.png "navigation Albums Disco de magasin")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-565">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="b3e8a-566">*Exploration des Albums Disco de magasin*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-566">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="b3e8a-567">Tâche 6 - Affichage des informations sur un Album spécifique</span><span class="sxs-lookup"><span data-stu-id="b3e8a-567">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="b3e8a-568">Dans cette tâche, vous allez implémenter le **magasin/détails** pour afficher des informations sur un album spécifique.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-568">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="b3e8a-569">Dans cet atelier pratique, tout ce dont vous allez afficher à propos de l’album est déjà contenu dans le **vue** modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-569">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="b3e8a-570">C’est le cas, au lieu de créer un **StoreDetailsViewModel** (classe), vous allez utiliser actuel **StoreBrowseViewModel** modèle en passant l’Album à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-570">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="b3e8a-571">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-571">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b3e8a-572">Ajouter un nouveau **détails** afficher pour le **StoreController**de **détails** méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-572">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="b3e8a-573">Pour ce faire, cliquez sur le **détails** méthode dans le **StoreController** de classe, puis cliquez sur **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-573">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="b3e8a-574">Dans le **ajouter une vue** boîte de dialogue, vérifiez que le **nom de la vue** est **détails**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-574">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="b3e8a-575">Vérifiez le **créer une vue fortement typée** case à cocher et sélectionnez **Album** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-575">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="b3e8a-576">Laissez les autres champs avec leur valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-576">Leave the other fields with their default value.</span></span> <span data-ttu-id="b3e8a-577">Cliquez ensuite sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-577">Then click **Add**.</span></span> <span data-ttu-id="b3e8a-578">Cela crée et ouvre un **\Views\Store\Details.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-578">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="b3e8a-579">![Ajout d’une vue détails](aspnet-mvc-4-fundamentals/_static/image31.png "Ajout d’une vue Détails")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-579">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="b3e8a-580">*Ajout d’une vue Détails*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-580">*Adding a Details View*</span></span>
3. <span data-ttu-id="b3e8a-581">Modifier la **Details.cshtml** fichier pour afficher des informations de l’Album, l’accès à la **Album** objet passé au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-581">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="b3e8a-582">Pour ce faire, remplacez le contenu par les éléments suivants : (c#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-582">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="b3e8a-583">Tâche 7 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-583">Task 7 - Running the Application</span></span>

<span data-ttu-id="b3e8a-584">Dans cette tâche, vous allez tester que le **détails** vue récupère les informations de l’Album à partir de la **détails action** (méthode).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-584">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="b3e8a-585">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-585">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3e8a-586">Le projet de démarrage le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-586">The project starts in the **Home** page.</span></span> <span data-ttu-id="b3e8a-587">Modifier l’URL pour **/Store/Details/5** pour vérifier les informations de l’album.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-587">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="b3e8a-588">![Exploration des détails des Albums](aspnet-mvc-4-fundamentals/_static/image32.png "navigation Albums détail")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-588">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="b3e8a-589">*Exploration des détails de l’Album*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-589">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="b3e8a-590">Tâche 8 : ajout de liens entre les Pages</span><span class="sxs-lookup"><span data-stu-id="b3e8a-590">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="b3e8a-591">Dans cette tâche, vous allez ajouter un lien dans la vue de magasin pour disposer d’un lien dans le nom de chaque Genre approprié **/magasin/Parcourir** URL.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-591">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="b3e8a-592">Ainsi, lorsque vous cliquez sur un Genre, par exemple **Disco**, il permet d’accéder à **/magasin/Parcourir ? genre = Disco** URL.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-592">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="b3e8a-593">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-593">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b3e8a-594">Mise à jour la **Index** page pour ajouter un lien vers le **Parcourir** page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-594">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="b3e8a-595">Pour ce faire, dans le **l’Explorateur de solutions** développez le **vues** dossier, puis le **magasin** et double-cliquez sur le **Index.cshtml** page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-595">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="b3e8a-596">Ajouter un lien vers le mode de navigation indiquant le genre sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-596">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="b3e8a-597">Pour ce faire, remplacez le code en surbrillance suivant dans le **&lt;li&gt;** balises : (c#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-597">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="b3e8a-598">une autre approche serait liaison directement à la page, avec un code semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-598">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="b3e8a-599">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="b3e8a-599">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="b3e8a-600">Bien que cette approche fonctionne, il dépend d’une chaîne codée en dur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-600">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="b3e8a-601">Si vous renommez ultérieurement le contrôleur, vous devez modifier cette instruction manuellement.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-601">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="b3e8a-602">Une meilleure solution consiste à utiliser un **programme d’assistance HTML** (méthode).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-602">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="b3e8a-603">ASP.NET MVC inclut une méthode de programme d’assistance HTML qui est disponible pour ces tâches.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-603">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="b3e8a-604">Le **Html.ActionLink()** méthode d’assistance permet de facilement générer HTML **&lt;un&gt;** liens, s’assurer que les chemins d’accès d’URL sont correctement encodé en URL.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-604">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="b3e8a-605">Htlm.ActionLink a plusieurs surcharges.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-605">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="b3e8a-606">Dans cet exercice, vous allez utiliser un qui accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-606">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="b3e8a-607">Texte du lien, qui affiche le nom du Genre</span><span class="sxs-lookup"><span data-stu-id="b3e8a-607">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="b3e8a-608">Nom d’action de contrôleur (**Parcourir**)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-608">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="b3e8a-609">Des valeurs de paramètre, en spécifiant le nom d’itinéraire (**Genre**) et la valeur (**nom du Genre**)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-609">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="b3e8a-610">Tâche 9 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-610">Task 9 - Running the Application</span></span>

<span data-ttu-id="b3e8a-611">Dans cette tâche, vous allez tester que chaque Genre est affiché avec un lien vers le **/magasin/Parcourir** URL.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-611">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="b3e8a-612">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-612">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3e8a-613">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-613">The project starts in the Home page.</span></span> <span data-ttu-id="b3e8a-614">Modifier l’URL pour **/stockages** pour vérifier que chaque Genre liens appropriés **/magasin/Parcourir** URL.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-614">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="b3e8a-615">![Exploration des Genres avec des liens vers la page Parcourir](aspnet-mvc-4-fundamentals/_static/image33.png "Genres de navigation avec des liens vers la page Parcourir")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-615">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="b3e8a-616">*Genres de navigation avec des liens vers la page Parcourir*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-616">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="b3e8a-617">Tâche 10 : à l’aide de Collection ViewModel dynamique pour transmettre des valeurs</span><span class="sxs-lookup"><span data-stu-id="b3e8a-617">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="b3e8a-618">Dans cette tâche, vous allez apprendre à une méthode simple et efficace pour transmettre les valeurs entre le contrôleur et la vue sans apporter de modifications dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-618">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="b3e8a-619">ASP.NET MVC 4 fournit la collection &quot;ViewModel&quot;, qui peut être assigné à n’importe quelle valeur dynamique et accessible à l’intérieur des contrôleurs et vues.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-619">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="b3e8a-620">Vous allez maintenant utiliser le regroupement dynamique ViewBag pour passer d’une liste de &quot; **en étoile genres** &quot; à partir du contrôleur à la vue.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-620">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="b3e8a-621">La vue de l’Index de magasin accédera à **ViewModel** et afficher les informations.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-621">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="b3e8a-622">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-622">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b3e8a-623">Ouvrez **StoreController.cs** et modifier **Index** méthode pour créer une liste de mieux genres dans ViewModel collection :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-623">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

> [!NOTE]
> You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.
~~~
2. <span data-ttu-id="b3e8a-624">L’icône représentant une étoile **&quot;starred.png&quot;** est inclus dans le **Source\Assets\Images** dossier de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-624">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="b3e8a-625">Pour l’ajouter à l’application, faites glisser son contenu à partir d’un **l’Explorateur Windows** fenêtre dans le **l’Explorateur de solutions** dans Visual Web Developer Express, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-625">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="b3e8a-626">![Image étoile d’ajout à la solution](aspnet-mvc-4-fundamentals/_static/image34.png "image étoile d’ajout à la solution")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-626">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="b3e8a-627">*Ajout d’une image en étoile à la solution*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-627">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="b3e8a-628">Ouvrez l’affichage **Store/Index.cshtml** et modifier le contenu.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-628">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="b3e8a-629">Vous lira le &quot;en étoile&quot; propriété dans le **ViewBag** collection et demandez si le nom du genre actuel est dans la liste.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-629">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="b3e8a-630">Dans ce cas, vous afficherez une icône représentant une étoile à droite vers le lien de genre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-630">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="b3e8a-631">(C#)</span><span class="sxs-lookup"><span data-stu-id="b3e8a-631">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="b3e8a-632">Tâche 11 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="b3e8a-632">Task 11 - Running the Application</span></span>

<span data-ttu-id="b3e8a-633">Dans cette tâche, vous allez tester que les genres indiqué par une étoile affichent une icône représentant une étoile.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-633">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="b3e8a-634">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-634">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b3e8a-635">Le projet de démarrage le **accueil** page.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-635">The project starts in the **Home** page.</span></span> <span data-ttu-id="b3e8a-636">Modifier l’URL pour **/stockages** pour vérifier que chaque genre proposée est l’étiquette respect :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-636">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="b3e8a-637">![Exploration des Genres avec éléments indiqué par une étoile](aspnet-mvc-4-fundamentals/_static/image35.png "Genres de navigation avec des éléments indiqué par une étoile")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-637">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="b3e8a-638">*Genres de navigation avec des éléments indiqué par une étoile*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-638">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="b3e8a-639">Exercice 7 : Une visite guidée de modèle ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b3e8a-639">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="b3e8a-640">Dans cet exercice, vous découvrirez les améliorations dans les modèles de projet ASP.NET MVC 4, en un coup de œil des fonctions les plus pertinentes du nouveau modèle.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-640">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="b3e8a-641">Tâche 1 : Explorer le modèle d’Application ASP.NET MVC 4 Internet</span><span class="sxs-lookup"><span data-stu-id="b3e8a-641">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="b3e8a-642">S’il est déjà ouvert, démarrez **Visual Studio Express pour le Web**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-642">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="b3e8a-643">Sélectionnez le **fichier | Nouveau | Projet** commande de menu.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-643">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="b3e8a-644">Dans le **nouveau projet** boîte de dialogue, sélectionnez le **Visual C# | Web** modèle dans le volet gauche arborescence, puis choisissez le **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-644">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="b3e8a-645">**Nom** le projet *MusicStore* et mettre à jour le **nom de la solution** à *commencer*, puis sélectionnez un emplacement (ou laissez la valeur par défaut) et cliquez sur **OK** .</span><span class="sxs-lookup"><span data-stu-id="b3e8a-645">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="b3e8a-646">![Création d’un nouveau projet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "création d’un nouveau projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-646">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="b3e8a-647">*Création d’un nouveau projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-647">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="b3e8a-648">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue, sélectionnez le **Application Internet** modèle de projet et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-648">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="b3e8a-649">Notez que vous pouvez sélectionner Razor ou ASPX en tant que le moteur d’affichage.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-649">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="b3e8a-650">![Création d’une Application ASP.NET MVC 4 Internet](aspnet-mvc-4-fundamentals/_static/image37.png "création d’une Application ASP.NET MVC 4 Internet")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-650">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="b3e8a-651">*Création d’une Application ASP.NET MVC 4 Internet*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-651">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-652">La syntaxe Razor a été introduite dans ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-652">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="b3e8a-653">Son objectif est de réduire le nombre de caractères et séquences de touches requis dans un fichier, en activant un rapide et le fluide de flux de travail de codage.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-653">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="b3e8a-654">Tire parti de Razor existant C# /Visual Basic (ou autres) compétences linguistiques et fournit une syntaxe de balisage de modèle qui permet à un flux de travail de construction HTML impressionnant.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-654">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="b3e8a-655">Appuyez sur **F5** pour exécuter la solution et consultez le modèle renouvelé.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-655">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="b3e8a-656">Vous pouvez récupérer les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-656">You can check out the following features:</span></span>

    1. <span data-ttu-id="b3e8a-657">**Modèles moderne**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-657">**Modern-style templates**</span></span>

        <span data-ttu-id="b3e8a-658">Les modèles ont été renouvelées, en fournissant plusieurs styles aspect moderne.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-658">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="b3e8a-659">![Les modèles ASP.NET MVC 4 adaptée](aspnet-mvc-4-fundamentals/_static/image38.png "adaptée modèles ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-659">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="b3e8a-660">*Modèles ASP.NET MVC 4 adaptée*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-660">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="b3e8a-661">**Rendu adaptable**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-661">**Adaptive Rendering**</span></span>

        <span data-ttu-id="b3e8a-662">Extraire le redimensionnement de la fenêtre de navigateur et notez la façon dont la mise en page s’adapte dynamiquement à la nouvelle taille de fenêtre.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-662">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="b3e8a-663">Ces modèles utilisent la technique de rendu adaptable pour afficher correctement dans les plateformes de bureau et mobiles sans aucune personnalisation.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-663">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="b3e8a-664">![Modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents](aspnet-mvc-4-fundamentals/_static/image39.png "le modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-664">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="b3e8a-665">*Modèle de projet ASP.NET MVC 4 dans les tailles de navigateur différents*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-665">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="b3e8a-666">Fermez le navigateur pour arrêter le débogueur, revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-666">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="b3e8a-667">Vous êtes en mesure d’Explorer la solution et passent en revue quelques-unes des nouvelles fonctionnalités introduites par ASP.NET MVC 4 dans le modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-667">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="b3e8a-668">![ASP.NET MVC 4-internet-à-modèle de projet application](aspnet-mvc-4-fundamentals/_static/image40.png "le modèle de projet ASP.NET MVC 4 Internet Application")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-668">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="b3e8a-669">*Le modèle de projet ASP.NET MVC 4 Internet Application*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-669">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="b3e8a-670">**Balisage de HTML5**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-670">**HTML5 markup**</span></span>

       <span data-ttu-id="b3e8a-671">Parcourir les vues de modèle pour rechercher le nouveau thème le balisage, par exemple ouvrir **About.cshtml** afficher au sein de **accueil** dossier.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-671">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="b3e8a-672">![Nouveau modèle, à l’aide d’un balisage Razor et HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nouveau modèle, à l’aide d’un balisage Razor et HTML5")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-672">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="b3e8a-673">*Nouveau modèle, à l’aide d’un balisage Razor et HTML5*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-673">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="b3e8a-674">**Bibliothèques JavaScript inclus**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-674">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="b3e8a-675">**jQuery**: jQuery simplifie le parcours de document HTML, la gestion des événements, l’animation et les interactions Ajax.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-675">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="b3e8a-676">**l’interface utilisateur jQuery**: cette bibliothèque fournit des abstractions pour une interaction de bas niveau et avancée de l’animation, effets et thème widgets, reposant sur la bibliothèque JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-676">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="b3e8a-677">Vous pouvez en savoir plus sur jQuery et jQuery UI dans [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-677">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="b3e8a-678">**KnockoutJS**: modèle par défaut de l’ASP.NET MVC 4 inclut désormais **KnockoutJS**, une infrastructure JavaScript MVVM qui vous permet de créer des applications web enrichies et très réactif à l’aide de JavaScript et HTML.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-678">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="b3e8a-679">Comme dans ASP.NET MVC 3, jQuery et jQuery bibliothèques d’interface utilisateur sont également inclus dans ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-679">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="b3e8a-680">Vous pouvez obtenir plus d’informations sur la bibliothèque KnockOutJS dans ce lien : [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-680">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="b3e8a-681">**Modernizr**: cette bibliothèque s’exécute automatiquement, rendre votre site compatible avec les navigateurs plus anciens lors de l’utilisation de technologies HTML5 et CSS3.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-681">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="b3e8a-682">Vous pouvez obtenir plus d’informations sur la bibliothèque Modernizr dans ce lien : [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-682">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="b3e8a-683">**SimpleMembership inclus dans la solution**</span><span class="sxs-lookup"><span data-stu-id="b3e8a-683">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="b3e8a-684">SimpleMembership a été conçu comme un remplacement pour le système de fournisseur de rôle ASP.NET et l’appartenance au précédent.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-684">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="b3e8a-685">Il possède plusieurs nouvelles fonctionnalités qui facilitent aux développeurs de pages web sécurisées dans une plus grande souplesse.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-685">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="b3e8a-686">Le modèle Internet a déjà configuré quelques éléments à intégrer SimpleMembership, par exemple, le AccountController est prêt à utiliser la OAuthWebSecurity (pour l’enregistrement du compte OAuth, connexion, gestion, etc.) et la sécurité du Web.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-686">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="b3e8a-687">![SimpleMembership inclus dans la solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership inclus dans la solution")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-687">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="b3e8a-688">*SimpleMembership inclus dans la solution*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-688">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="b3e8a-689">Obtenir des informations supplémentaires [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) dans MSDN.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-689">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="b3e8a-690">En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [annexe b : publication une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-690">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b3e8a-691">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="b3e8a-691">Summary</span></span>

<span data-ttu-id="b3e8a-692">À la fin de cet atelier pratique, vous avez appris les notions de base d’ASP.NET MVC :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-692">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="b3e8a-693">Les éléments fondamentaux d’une application MVC et comment ils interagissent</span><span class="sxs-lookup"><span data-stu-id="b3e8a-693">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="b3e8a-694">Comment créer une Application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b3e8a-694">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="b3e8a-695">Comment ajouter et configurer les contrôleurs pour gérer les paramètres passés par l’intermédiaire de l’URL et la chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="b3e8a-695">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="b3e8a-696">Comment ajouter une page maître de mise en page pour configurer un modèle pour le contenu HTML commun, une feuille de style pour améliorer l’apparence et un modèle d’affichage pour afficher le contenu HTML</span><span class="sxs-lookup"><span data-stu-id="b3e8a-696">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="b3e8a-697">Comment utiliser le modèle ViewModel pour passer des propriétés pour le modèle d’affichage pour afficher des informations dynamiques</span><span class="sxs-lookup"><span data-stu-id="b3e8a-697">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="b3e8a-698">Comment utiliser les paramètres transmis aux contrôleurs dans le modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="b3e8a-698">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="b3e8a-699">Comment ajouter des liens vers les pages à l’intérieur de l’application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b3e8a-699">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="b3e8a-700">Comment ajouter et utiliser des propriétés dynamiques dans une vue</span><span class="sxs-lookup"><span data-stu-id="b3e8a-700">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="b3e8a-701">Les améliorations dans les modèles de projet ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b3e8a-701">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b3e8a-702">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="b3e8a-702">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b3e8a-703">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-703">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b3e8a-704">Les instructions suivantes vous guident à travers les étapes requises pour installer *Visual studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-704">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b3e8a-705">Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-705">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b3e8a-706">Sinon, si vous avez déjà installé Web Platform Installer, vous pouvez ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-706">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b3e8a-707">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-707">Click on **Install Now**.</span></span> <span data-ttu-id="b3e8a-708">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-708">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b3e8a-709">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-709">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b3e8a-710">![Installer Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-710">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b3e8a-711">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-711">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b3e8a-712">Lisez les termes et les licences de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-712">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="b3e8a-714">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-714">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b3e8a-715">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-715">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="b3e8a-717">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-717">*Installation progress*</span></span>
6. <span data-ttu-id="b3e8a-718">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-718">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="b3e8a-720">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-720">*Installation completed*</span></span>
7. <span data-ttu-id="b3e8a-721">Cliquez sur **Exit** fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-721">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b3e8a-722">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-722">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour la vignette du Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="b3e8a-724">*VS Express pour la vignette du Web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-724">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b3e8a-725">Annexe b : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b3e8a-725">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b3e8a-726">Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et de publier l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-726">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="b3e8a-727">Tâche 1 - Création d’un Site Web à partir de Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b3e8a-727">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="b3e8a-728">Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-728">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-729">Avec Windows Azure, vous pouvez héberger des Sites Web ASP.NET 10 gratuitement et puis faire évoluer à mesure que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-729">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b3e8a-730">Vous pouvez vous inscrire [ici](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-730">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b3e8a-731">![Ouvrez une session sur le portail Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "connectez-vous au portail Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-731">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b3e8a-732">*Ouvrez une session sur le portail de gestion Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-732">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="b3e8a-733">Cliquez sur **nouveau** sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-733">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b3e8a-734">![Création d’un Site Web](aspnet-mvc-4-fundamentals/_static/image49.png "création d’un Site Web")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-734">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b3e8a-735">*Création d’un Site Web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-735">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b3e8a-736">Cliquez sur **de calcul** | **Site Web**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-736">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b3e8a-737">Puis sélectionnez **création rapide** option.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-737">Then select **Quick Create** option.</span></span> <span data-ttu-id="b3e8a-738">Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-738">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-739">Un Site Web de Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-739">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b3e8a-740">L’option Création rapide vous permet de déployer une application web complète pour le Site Web Windows Azure à partir d’à l’extérieur du portail.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-740">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="b3e8a-741">Il n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-741">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b3e8a-742">![Création d’un Site Web à l’aide de la création rapide](aspnet-mvc-4-fundamentals/_static/image50.png "création d’un Site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-742">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b3e8a-743">*Création d’un Site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-743">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b3e8a-744">Attendez que la nouvelle **Site Web** est créé.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-744">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b3e8a-745">Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-745">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b3e8a-746">Vérifiez que le nouveau Site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-746">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b3e8a-747">![Navigation vers le nouveau site web](aspnet-mvc-4-fundamentals/_static/image51.png "exploration vers le nouveau site web")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-747">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b3e8a-748">*Navigation vers le nouveau site web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-748">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b3e8a-749">![Site Web en cours d’exécution](aspnet-mvc-4-fundamentals/_static/image52.png "site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-749">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="b3e8a-750">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-750">*Web site running*</span></span>
6. <span data-ttu-id="b3e8a-751">Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-751">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b3e8a-752">![Ouvrir les pages de gestion du site web](aspnet-mvc-4-fundamentals/_static/image53.png "ouvrir les pages de gestion de site web")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-752">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b3e8a-753">*Ouvrir les pages de gestion de Site Web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-753">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b3e8a-754">Dans le **tableau de bord** sous le **coup de œil rapide** , cliquez sur le **télécharger le profil de publication** lien.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-754">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-755">Le *le profil de publication* contient toutes les informations nécessaires pour publier une application web sur un site Web de Windows Azure pour chaque méthode de publication activée.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-755">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="b3e8a-756">Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour se connecter à et de s’authentifier auprès de chacun des points de terminaison pour laquelle une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-756">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b3e8a-757">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge la lecture de publier les profils pour automatiser la configuration de ces programmes pour publication d’applications web pour les sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-757">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="b3e8a-758">![Profil de publication de téléchargement du site web](aspnet-mvc-4-fundamentals/_static/image54.png "profil de publication de téléchargement du site web")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-758">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b3e8a-759">*Profil de publication de téléchargement du Site Web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-759">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b3e8a-760">Téléchargez le fichier de profil de publication à un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-760">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b3e8a-761">Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-761">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="b3e8a-762">![L’enregistrement du fichier de profil de publication](aspnet-mvc-4-fundamentals/_static/image55.png "l’enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-762">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b3e8a-763">*L’enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-763">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b3e8a-764">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="b3e8a-764">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b3e8a-765">Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-765">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b3e8a-766">Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-766">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b3e8a-767">Vous devez un serveur de base de données SQL pour stocker la base de données de l’application.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-767">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b3e8a-768">Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-768">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b3e8a-769">Si vous ne disposez pas d’un serveur créé, vous pouvez créer un à l’aide de la **ajouter** bouton sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-769">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b3e8a-770">Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et un mot de passe**, comme vous allez l’utiliser dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-770">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b3e8a-771">Ne créez pas encore, la base de données telle qu’elle sera créée dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-771">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b3e8a-772">![Tableau de bord de serveur SQL de base de données](aspnet-mvc-4-fundamentals/_static/image56.png "tableau de bord de serveur SQL de base de données")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-772">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b3e8a-773">*Tableau de bord de serveur SQL de base de données*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-773">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b3e8a-774">Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-774">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b3e8a-775">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP du Client actuel** et le coller dans le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) bouton.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-775">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Ajout d’adresse IP du Client](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="b3e8a-777">*Ajout d’adresse IP du Client*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-777">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b3e8a-778">Une fois la **adresse IP du Client** est ajouté aux adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-778">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="b3e8a-780">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-780">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b3e8a-781">Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b3e8a-781">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b3e8a-782">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-782">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b3e8a-783">Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-783">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b3e8a-784">![Publication de l’Application](aspnet-mvc-4-fundamentals/_static/image60.png "publication de l’Application")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-784">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="b3e8a-785">*Publier le site web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-785">*Publishing the web site*</span></span>
2. <span data-ttu-id="b3e8a-786">Importer le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-786">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b3e8a-787">![L’importation du profil de publication](aspnet-mvc-4-fundamentals/_static/image61.png "l’importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-787">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b3e8a-788">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-788">*Importing publish profile*</span></span>
3. <span data-ttu-id="b3e8a-789">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-789">Click **Validate Connection**.</span></span> <span data-ttu-id="b3e8a-790">Une fois la Validation terminée. Cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-790">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b3e8a-791">La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-791">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b3e8a-792">![Validation de la connexion](aspnet-mvc-4-fundamentals/_static/image62.png "validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-792">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="b3e8a-793">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-793">*Validating connection*</span></span>
4. <span data-ttu-id="b3e8a-794">Dans le **paramètres** sous le **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-794">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b3e8a-795">![Configuration de déploiement Web](aspnet-mvc-4-fundamentals/_static/image63.png "configuration de déploiement Web")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-795">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b3e8a-796">*Configuration de déploiement Web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-796">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b3e8a-797">Configurer la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="b3e8a-797">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="b3e8a-798">Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-798">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="b3e8a-799">Dans **nom d’utilisateur** tapez le nom de connexion de votre administrateur de serveur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-799">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="b3e8a-800">Dans **mot de passe** votre mot de passe du compte de connexion administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-800">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="b3e8a-801">Tapez un nouveau nom de base de données, par exemple : *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-801">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="b3e8a-802">![Configuration de chaîne de connexion de destination](aspnet-mvc-4-fundamentals/_static/image64.png "configuration de chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-802">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="b3e8a-803">*Configuration de chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-803">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b3e8a-804">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-804">Then click **OK**.</span></span> <span data-ttu-id="b3e8a-805">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-805">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b3e8a-806">![Création de la base de données](aspnet-mvc-4-fundamentals/_static/image65.png "création de la chaîne de la base de données")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-806">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="b3e8a-807">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-807">*Creating the database*</span></span>
7. <span data-ttu-id="b3e8a-808">La chaîne de connexion que vous allez utiliser pour se connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte par défaut de connexion.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-808">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b3e8a-809">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-809">Then click **Next**.</span></span>

    <span data-ttu-id="b3e8a-810">![Chaîne de connexion pointant vers la base de données SQL](aspnet-mvc-4-fundamentals/_static/image66.png "chaîne de connexion pointant vers la base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-810">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b3e8a-811">*Chaîne de connexion pointant vers la base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-811">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b3e8a-812">Dans le **aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-812">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b3e8a-813">![Publication de l’application web](aspnet-mvc-4-fundamentals/_static/image67.png "publication de l’application web")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-813">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="b3e8a-814">*Publication de l’application web*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-814">*Publishing the web application*</span></span>
9. <span data-ttu-id="b3e8a-815">Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-815">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="b3e8a-816">![Publication d’application sur Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application publiée dans Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-816">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="b3e8a-817">*Application de publication sur Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-817">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="b3e8a-818">Annexe c : à l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="b3e8a-818">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="b3e8a-819">Avec des extraits de code, vous avez tout le code que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-819">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b3e8a-820">Le document lab vous indique exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-820">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b3e8a-821">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-fundamentals/_static/image69.png "des extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-821">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b3e8a-822">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-822">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b3e8a-823">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="b3e8a-823">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b3e8a-824">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-824">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b3e8a-825">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-825">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b3e8a-826">Observez comment IntelliSense affiche les noms des extraits de code de mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-826">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b3e8a-827">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entier est sélectionné).</span><span class="sxs-lookup"><span data-stu-id="b3e8a-827">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b3e8a-828">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-828">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b3e8a-829">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-fundamentals/_static/image70.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-829">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b3e8a-830">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-830">*Start typing the snippet name*</span></span>

<span data-ttu-id="b3e8a-831">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-fundamentals/_static/image71.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-831">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b3e8a-832">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-832">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b3e8a-833">![Appuyez sur Tab à nouveau et l’extrait de code sont développés](aspnet-mvc-4-fundamentals/_static/image72.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-833">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b3e8a-834">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-834">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b3e8a-835">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-835">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b3e8a-836">Clic droit où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-836">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b3e8a-837">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-837">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b3e8a-838">Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="b3e8a-838">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b3e8a-839">![Avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-fundamentals/_static/image73.png "avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-839">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b3e8a-840">*Avec le bouton droit sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-840">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b3e8a-841">![Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-fundamentals/_static/image74.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="b3e8a-841">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b3e8a-842">*Sélectionnez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="b3e8a-842">*Pick the relevant snippet from the list, by clicking on it*</span></span>
