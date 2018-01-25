---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "Programmes d’assistance ASP.NET MVC 4, formulaires et la Validation | Documents Microsoft"
author: rick-anderson
description: "Dans ASP.NET MVC 4 modèles et les ateliers pratiques de données Access, vous avez été le chargement et affichage des données à partir de la base de données. Dans cet atelier pratique, vous allez ajouter à la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 925d659f42496045089ba056e194ac977c37a8de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="8579c-104">Validation, les formulaires et les programmes d’assistance ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="8579c-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>
====================
<span data-ttu-id="8579c-105">par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8579c-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="8579c-106">Dans **accès aux données et les modèles ASP.NET MVC 4** atelier pratique, vous avez été le chargement et l’affichage des données à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-106">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="8579c-107">Dans cet atelier pratique, vous allez ajouter à la **magasin de musique** application la possibilité de modifier ces données.</span><span class="sxs-lookup"><span data-stu-id="8579c-107">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>
> 
> <span data-ttu-id="8579c-108">Objectifs à l’esprit, vous allez d’abord créer le contrôleur qui prend en charge les actions de création, lecture, mise à jour et supprimer (CRUD) des albums.</span><span class="sxs-lookup"><span data-stu-id="8579c-108">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="8579c-109">Vous allez générer un modèle de vue d’Index en tirant parti de la fonctionnalité de génération de modèles automatique de ASP.NET MVC pour afficher les propriétés des albums dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="8579c-109">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="8579c-110">Pour des raisons de cette vue, vous allez ajouter un programme d’assistance HTML personnalisé qui va tronquer les longues descriptions.</span><span class="sxs-lookup"><span data-stu-id="8579c-110">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>
> 
> <span data-ttu-id="8579c-111">Vous ajouterez par la suite, le modifier et créer des vues qui permettent de modifier les albums dans la base de données, à l’aide des éléments de formulaire tels que des listes déroulantes.</span><span class="sxs-lookup"><span data-stu-id="8579c-111">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>
> 
> <span data-ttu-id="8579c-112">Enfin, vous permet aux utilisateurs de supprimer un album et vous serez également les empêcher d’entrer des données erronées en validant leur entrée.</span><span class="sxs-lookup"><span data-stu-id="8579c-112">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="8579c-113">Cet atelier pratique suppose que vous avez une connaissance élémentaire des **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="8579c-113">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="8579c-114">Si vous n’avez pas utilisé **ASP.NET MVC** auparavant, nous vous recommandons de dépasser **notions de base ASP.NET MVC** atelier pratique.</span><span class="sxs-lookup"><span data-stu-id="8579c-114">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>
> 
> 
> <span data-ttu-id="8579c-115">Ce laboratoire présente les améliorations et nouvelles fonctionnalités décrites précédemment en appliquant les modifications mineures à un exemple d’application Web dans le dossier Source.</span><span class="sxs-lookup"><span data-stu-id="8579c-115">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="8579c-116">Tous les exemples de code et des extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="8579c-116">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="8579c-117">Objectifs</span><span class="sxs-lookup"><span data-stu-id="8579c-117">Objectives</span></span>

<span data-ttu-id="8579c-118">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="8579c-118">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="8579c-119">Créer un contrôleur pour prendre en charge les opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="8579c-119">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="8579c-120">Générer une vue d’Index pour afficher les propriétés de l’entité dans une table HTML</span><span class="sxs-lookup"><span data-stu-id="8579c-120">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="8579c-121">Ajouter un programme d’assistance HTML personnalisé</span><span class="sxs-lookup"><span data-stu-id="8579c-121">Add a custom HTML helper</span></span>
- <span data-ttu-id="8579c-122">Créer et personnaliser une vue à modifier</span><span class="sxs-lookup"><span data-stu-id="8579c-122">Create and customize an Edit View</span></span>
- <span data-ttu-id="8579c-123">Différence entre les méthodes d’action qui réagissent appels HTTP-POST ou HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="8579c-123">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="8579c-124">Ajouter et personnaliser une instruction Create View</span><span class="sxs-lookup"><span data-stu-id="8579c-124">Add and customize a Create View</span></span>
- <span data-ttu-id="8579c-125">Handle de la suppression d’une entité</span><span class="sxs-lookup"><span data-stu-id="8579c-125">Handle the deletion of an entity</span></span>
- <span data-ttu-id="8579c-126">Valider l’entrée d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="8579c-126">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8579c-127">Prérequis</span><span class="sxs-lookup"><span data-stu-id="8579c-127">Prerequisites</span></span>

<span data-ttu-id="8579c-128">Vous devez disposer des éléments suivants pour effectuer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="8579c-128">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="8579c-129">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lecture [annexe A](#AppendixA) pour obtenir des instructions sur l’installation).</span><span class="sxs-lookup"><span data-stu-id="8579c-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="8579c-130">Installation</span><span class="sxs-lookup"><span data-stu-id="8579c-130">Setup</span></span>

<span data-ttu-id="8579c-131">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="8579c-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="8579c-132">Pour plus de commodité, une grande partie du code que vous gérez le long de cet atelier est disponible sous les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8579c-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="8579c-133">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="8579c-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="8579c-134">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et vous souhaitez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide de l’annexe b :](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="8579c-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8579c-135">Exercices</span><span class="sxs-lookup"><span data-stu-id="8579c-135">Exercises</span></span>

<span data-ttu-id="8579c-136">Les exercices suivants composent cet atelier pratique :</span><span class="sxs-lookup"><span data-stu-id="8579c-136">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="8579c-137">Création du contrôleur du Gestionnaire de magasin et sa vue de l’Index</span><span class="sxs-lookup"><span data-stu-id="8579c-137">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="8579c-138">Ajout d’une application d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="8579c-138">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="8579c-139">Création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="8579c-139">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="8579c-140">Ajout d’une vue de créer</span><span class="sxs-lookup"><span data-stu-id="8579c-140">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="8579c-141">Suppression de la gestion</span><span class="sxs-lookup"><span data-stu-id="8579c-141">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="8579c-142">Ajout de la validation</span><span class="sxs-lookup"><span data-stu-id="8579c-142">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="8579c-143">À l’aide de jQuery non obstructive côté Client</span><span class="sxs-lookup"><span data-stu-id="8579c-143">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="8579c-144">Chaque exercice est accompagné par un **fin** dossier qui contient la solution résultante, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="8579c-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="8579c-145">Si vous avez besoin d’aide fonctionne via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="8579c-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="8579c-146">Durée estimée pour effectuer ce laboratoire : **60 minutes**</span><span class="sxs-lookup"><span data-stu-id="8579c-146">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="8579c-147">Exercice 1 : Création du contrôleur du Gestionnaire de magasin et sa vue de l’Index</span><span class="sxs-lookup"><span data-stu-id="8579c-147">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="8579c-148">Dans cet exercice, vous allez apprendre à créer un nouveau contrôleur pour prendre en charge les opérations CRUD, personnaliser sa méthode d’action Index pour retourner une liste d’albums à partir de la base de données et la génération d’un modèle de vue d’Index en tirant parti de la génération de modèles automatique de ASP.NET MVC enfin fonction pour afficher les propriétés des albums dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="8579c-148">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="8579c-149">Tâche 1 : création de la StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="8579c-149">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="8579c-150">Dans cette tâche, vous allez créer un nouveau contrôleur appelé **StoreManagerController** pour prendre en charge les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="8579c-150">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="8579c-151">Ouvrez le **commencer** solution situé dans **début/CreatingTheStoreManagerController-Ex1/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-151">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="8579c-152">Vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="8579c-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8579c-153">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8579c-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8579c-154">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8579c-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8579c-155">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8579c-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-156">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8579c-157">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8579c-158">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8579c-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8579c-159">Ajoutez un nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8579c-159">Add a new controller.</span></span> <span data-ttu-id="8579c-160">Pour ce faire, cliquez sur le **contrôleurs** dossier dans l’Explorateur de solutions, sélectionnez **ajouter** , puis le **contrôleur** commande.</span><span class="sxs-lookup"><span data-stu-id="8579c-160">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="8579c-161">Modifier la **contrôleur** **nom** à **StoreManagerController** et assurez-vous que l’option **contrôleur MVC avec des actions de lecture/écriture vides**est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="8579c-161">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="8579c-162">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8579c-162">Click **Add**.</span></span>

    <span data-ttu-id="8579c-163">![Boîte de dialogue Ajouter contrôleur](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "boîte de dialogue Ajouter contrôleur")</span><span class="sxs-lookup"><span data-stu-id="8579c-163">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="8579c-164">*Boîte de dialogue contrôleur ajouter*</span><span class="sxs-lookup"><span data-stu-id="8579c-164">*Add Controller Dialog*</span></span>

    <span data-ttu-id="8579c-165">Une nouvelle classe de contrôleur est générée.</span><span class="sxs-lookup"><span data-stu-id="8579c-165">A new Controller class is generated.</span></span> <span data-ttu-id="8579c-166">Étant donné que vous avez indiquée pour ajouter des actions en lecture/écriture, les méthodes stub, des actions CRUD courantes sont créées avec des commentaires TODO renseignés, vous invitant à inclure la logique d’application spécifique.</span><span class="sxs-lookup"><span data-stu-id="8579c-166">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="8579c-167">Tâche 2 : personnalisation de l’Index StoreManager</span><span class="sxs-lookup"><span data-stu-id="8579c-167">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="8579c-168">Dans cette tâche, vous allez personnaliser la méthode d’action StoreManager Index pour retourner une vue avec la liste des albums à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-168">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="8579c-169">Dans la classe StoreManagerController, ajoutez le code suivant *à l’aide de* directives.</span><span class="sxs-lookup"><span data-stu-id="8579c-169">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="8579c-170">(Code d’extrait de code - *Validation - Ex1, les formulaires et les programmes d’assistance de ASP.NET MVC 4 à l’aide de MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="8579c-170">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="8579c-171">Ajoutez un champ à la **StoreManagerController** pour stocker une instance de **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="8579c-171">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="8579c-172">(Code d’extrait de code - *d’applications auxiliaires ASP.NET MVC 4 et de formulaires et de Validation - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="8579c-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="8579c-173">Implémentation de l’action de StoreManagerController Index pour retourner une vue avec la liste des albums.</span><span class="sxs-lookup"><span data-stu-id="8579c-173">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="8579c-174">La logique d’action de contrôleur sera très similaire à l’action d’Index de la StoreController écrite précédemment.</span><span class="sxs-lookup"><span data-stu-id="8579c-174">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="8579c-175">Utilisez LINQ pour récupérer tous les albums, y compris les informations de Genre et artiste pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="8579c-175">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="8579c-176">(Code d’extrait de code - *Validation - Ex1 StoreManagerController Index, les formulaires et les programmes d’assistance ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="8579c-176">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="8579c-177">Tâche 3 : création de la vue d’Index</span><span class="sxs-lookup"><span data-stu-id="8579c-177">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="8579c-178">Dans cette tâche, vous allez créer le modèle d’affichage de l’Index pour afficher la liste des albums retourné par le **StoreManager** contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8579c-178">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="8579c-179">Avant de créer le nouveau modèle d’affichage, vous devez générer le projet afin que les **boîte de dialogue Vue ajouter** connaît le **Album** classe à utiliser.</span><span class="sxs-lookup"><span data-stu-id="8579c-179">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="8579c-180">Sélectionnez **générer | Build MvcMusicStore** pour générer le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-180">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="8579c-181">Avec le bouton droit à l’intérieur de la **Index** méthode d’action et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8579c-181">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="8579c-182">Cela affiche la **ajouter une vue** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="8579c-182">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="8579c-183">![Ajouter une vue](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "ajouter une vue")</span><span class="sxs-lookup"><span data-stu-id="8579c-183">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="8579c-184">*Ajout d’une vue à partir de l’Index (méthode)*</span><span class="sxs-lookup"><span data-stu-id="8579c-184">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="8579c-185">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **Index**.</span><span class="sxs-lookup"><span data-stu-id="8579c-185">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="8579c-186">Sélectionnez le **créer une vue fortement typée** option, puis sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-186">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="8579c-187">Sélectionnez **liste** à partir de la **modèle de vue de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-187">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="8579c-188">Laissez le **moteur d’affichage** à **Razor** et les autres champs leur valeur par défaut de valeur, puis **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8579c-188">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="8579c-189">![Ajout d’une vue d’index](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Ajout d’une vue d’index")</span><span class="sxs-lookup"><span data-stu-id="8579c-189">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="8579c-190">*Ajout d’une vue d’Index*</span><span class="sxs-lookup"><span data-stu-id="8579c-190">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="8579c-191">Tâche 4 : personnalisation de la vue de la structure de la vue d’Index</span><span class="sxs-lookup"><span data-stu-id="8579c-191">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="8579c-192">Dans cette tâche, vous pouvez ajuster le modèle d’affichage simple créé avec la fonctionnalité de génération de modèles automatique ASP.NET MVC pour afficher les champs que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="8579c-192">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="8579c-193">Le **la structure** prise en charge dans ASP.NET MVC génère un modèle d’affichage simple qui répertorie tous les champs dans le modèle de l’Album.</span><span class="sxs-lookup"><span data-stu-id="8579c-193">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="8579c-194">**Génération de modèles automatique** fournit un moyen rapide de commencer à utiliser une vue fortement typée : au lieu de devoir écrire le modèle d’affichage manuellement, la génération de modèles automatique rapidement génère un modèle par défaut et vous pouvez ensuite modifier le code généré.</span><span class="sxs-lookup"><span data-stu-id="8579c-194">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="8579c-195">Passez en revue le code créé.</span><span class="sxs-lookup"><span data-stu-id="8579c-195">Review the code created.</span></span> <span data-ttu-id="8579c-196">La liste de champs générées feront partie des éléments suivants de table HTML qui **la structure** utilise pour afficher les données tabulaires.</span><span class="sxs-lookup"><span data-stu-id="8579c-196">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="8579c-197">Remplacez le  **&lt;table&gt;**  code avec le code suivant pour afficher uniquement les **Genre**, **artiste**, **titre d’Album**, et **prix** champs.</span><span class="sxs-lookup"><span data-stu-id="8579c-197">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="8579c-198">Cette opération supprime le **AlbumId** et **Album Art URL** colonnes.</span><span class="sxs-lookup"><span data-stu-id="8579c-198">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="8579c-199">En outre, il modifie les colonnes GenreId et ArtistId pour afficher leurs propriétés de classe lié de **Artist.Name** et **Genre.Name**et supprime la **détails** lien.</span><span class="sxs-lookup"><span data-stu-id="8579c-199">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="8579c-200">Modifier les descriptions suivantes.</span><span class="sxs-lookup"><span data-stu-id="8579c-200">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="8579c-201">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-201">Task 5 - Running the Application</span></span>

<span data-ttu-id="8579c-202">Dans cette tâche, vous allez tester que le **StoreManager** **Index** modèle d’affichage affiche la liste des albums en fonction de la conception des étapes précédentes.</span><span class="sxs-lookup"><span data-stu-id="8579c-202">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="8579c-203">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-203">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-204">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-204">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-205">Modifier l’URL pour **/StoreManager** pour vérifier qu’une liste d’albums s’affiche, indiquant leur **titre**, **artiste** et **Genre**.</span><span class="sxs-lookup"><span data-stu-id="8579c-205">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="8579c-206">![Parcourir la liste des albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "parcourir la liste des albums")</span><span class="sxs-lookup"><span data-stu-id="8579c-206">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="8579c-207">*Parcourir la liste des albums*</span><span class="sxs-lookup"><span data-stu-id="8579c-207">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="8579c-208">Exercice 2 : Ajout d’une application d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="8579c-208">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="8579c-209">La page d’Index de StoreManager a un problème potentiel : propriétés du titre et le nom de l’auteur peuvent tous deux être suffisamment longue pour décaler la mise en forme de table.</span><span class="sxs-lookup"><span data-stu-id="8579c-209">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="8579c-210">Dans cet exercice, vous allez apprendre à ajouter un programme d’assistance HTML personnalisé pour tronquer ce texte.</span><span class="sxs-lookup"><span data-stu-id="8579c-210">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="8579c-211">Dans l’illustration suivante, vous pouvez voir la manière dont le format est modifié en raison de la longueur du texte lorsque vous utilisez une taille de navigateur petit.</span><span class="sxs-lookup"><span data-stu-id="8579c-211">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="8579c-212">![Parcourir la liste des Albums avec pas tronqué texte](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "parcourir la liste des Albums avec ne tronqué pas de texte")</span><span class="sxs-lookup"><span data-stu-id="8579c-212">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="8579c-213">*Parcourir la liste des Albums avec ne tronqué pas de texte*</span><span class="sxs-lookup"><span data-stu-id="8579c-213">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="8579c-214">Tâche 1 - extension de l’application d’assistance HTML</span><span class="sxs-lookup"><span data-stu-id="8579c-214">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="8579c-215">Dans cette tâche, vous allez ajouter une nouvelle méthode **Truncate** à la **HTML** objet exposé dans les vues MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8579c-215">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="8579c-216">Pour ce faire, vous allez implémenter une **méthode d’extension** à la fonction intégrée **System.Web.Mvc.HtmlHelper** classe fournie par ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8579c-216">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="8579c-217">Pour en savoir plus sur **les méthodes d’Extension**, consultez cet article msdn.</span><span class="sxs-lookup"><span data-stu-id="8579c-217">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="8579c-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="8579c-218">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="8579c-219">Ouvrez le **commencer** solution situé dans **début/AddingAnHTMLHelper-Ex2/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-219">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="8579c-220">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8579c-220">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8579c-221">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="8579c-221">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8579c-222">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8579c-222">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8579c-223">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8579c-223">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8579c-224">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8579c-224">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-225">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-225">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8579c-226">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-226">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8579c-227">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8579c-227">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8579c-228">Index de StoreManager afficher.</span><span class="sxs-lookup"><span data-stu-id="8579c-228">Open StoreManager's Index View.</span></span> <span data-ttu-id="8579c-229">Pour ce faire, dans l’Explorateur de solutions, développez le **vues** dossier, puis le **StoreManager** et ouvrez le **Index.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="8579c-229">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="8579c-230">Ajoutez le code suivant ci-dessous le  **@model**  la directive pour définir le **Truncate** méthode d’assistance.</span><span class="sxs-lookup"><span data-stu-id="8579c-230">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="8579c-231">Tâche 2 : troncation de texte dans la Page</span><span class="sxs-lookup"><span data-stu-id="8579c-231">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="8579c-232">Dans cette tâche, vous allez utiliser le **Truncate** méthode tronquer le texte dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="8579c-232">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="8579c-233">Index de StoreManager afficher.</span><span class="sxs-lookup"><span data-stu-id="8579c-233">Open StoreManager's Index View.</span></span> <span data-ttu-id="8579c-234">Pour ce faire, dans l’Explorateur de solutions, développez le **vues** dossier, puis le **StoreManager** et ouvrez le **Index.cshtml** fichier.</span><span class="sxs-lookup"><span data-stu-id="8579c-234">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="8579c-235">Remplacez les lignes qui indiquent la **nom artiste** et Album **titre**.</span><span class="sxs-lookup"><span data-stu-id="8579c-235">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="8579c-236">Pour ce faire, remplacez les lignes suivantes.</span><span class="sxs-lookup"><span data-stu-id="8579c-236">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="8579c-237">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-237">Task 3 - Running the Application</span></span>

<span data-ttu-id="8579c-238">Dans cette tâche, vous allez tester que le **StoreManager** **Index** afficher le modèle tronque le titre et nom de l’auteur de l’Album.</span><span class="sxs-lookup"><span data-stu-id="8579c-238">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="8579c-239">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-239">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-240">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-240">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-241">Modifier l’URL pour **/StoreManager** pour vérifier ce long textes en le **titre** et **artiste** colonne sont tronqués.</span><span class="sxs-lookup"><span data-stu-id="8579c-241">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="8579c-242">![Tronqué noms titres et les artistes](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "tronqué noms artistes et de titres")</span><span class="sxs-lookup"><span data-stu-id="8579c-242">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="8579c-243">*Les noms d’artiste et les titres tronquées*</span><span class="sxs-lookup"><span data-stu-id="8579c-243">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="8579c-244">Exercice 3 : Création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="8579c-244">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="8579c-245">Dans cet exercice, vous allez apprendre à créer un formulaire pour permettre aux responsables de magasin modifier un Album.</span><span class="sxs-lookup"><span data-stu-id="8579c-245">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="8579c-246">Ils seront parcourir le **/StoreManager/Edit/id** URL (**id** en cours de l’id unique de l’album pour modifier), créant ainsi un appel HTTP-GET sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="8579c-246">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="8579c-247">La méthode d’action contrôleur modifier se récupérer l’Album approprié à partir de la base de données, créez un **StoreManagerViewModel** objet à encapsuler (ainsi que d’une liste d’artistes et Genres), puis transmettre à un modèle d’affichage afficher la page HTML à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8579c-247">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="8579c-248">Cette page contient un  **&lt;formulaire&gt;**  élément avec les zones de texte et les listes déroulantes pour modifier les propriétés de l’Album.</span><span class="sxs-lookup"><span data-stu-id="8579c-248">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="8579c-249">Une fois que l’utilisateur met à jour les valeurs de formulaire Album et clique sur le **enregistrer** bouton, les modifications sont envoyées une requête HTTP-POST rappeler **/StoreManager/Edit/id**. Bien que l’URL reste la même que dans le dernier appel, ASP.NET MVC qui identifie ce temps est une requête HTTP-POST et par conséquent exécute une autre méthode d’action de modification (un décorée avec **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="8579c-249">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="8579c-250">Tâche 1 : implémentation de la méthode d’Action HTTP-GET Edition</span><span class="sxs-lookup"><span data-stu-id="8579c-250">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="8579c-251">Dans cette tâche, vous allez implémenter la version HTTP-GET de la méthode d’action de modification pour récupérer l’Album approprié à partir de la base de données, ainsi qu’une liste de tous les Genres et artistes.</span><span class="sxs-lookup"><span data-stu-id="8579c-251">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="8579c-252">Il sera empaqueter ces données dans le **StoreManagerViewModel** objet défini dans la dernière étape, qui est ensuite transmise à un modèle d’affichage pour afficher la réponse avec.</span><span class="sxs-lookup"><span data-stu-id="8579c-252">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="8579c-253">Ouvrez le **commencer** solution situé dans **début/CreatingTheEditView-Ex3/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-253">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="8579c-254">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8579c-254">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8579c-255">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="8579c-255">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8579c-256">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8579c-256">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8579c-257">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8579c-257">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8579c-258">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8579c-258">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-259">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-259">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8579c-260">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-260">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8579c-261">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8579c-261">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8579c-262">Ouvrez le **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="8579c-262">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="8579c-263">Pour ce faire, développez le **contrôleurs** et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8579c-263">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="8579c-264">Remplacez le **HTTP-GET modifier** méthode d’action avec le code suivant pour récupérer les **Album** , ainsi que les **Genres** et **artistes**répertorie.</span><span class="sxs-lookup"><span data-stu-id="8579c-264">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="8579c-265">(Code d’extrait de code - *Validation - Ex3 StoreManagerController HTTP-GET modifier l’action et des formulaires et des programmes d’assistance de ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="8579c-265">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="8579c-266">Vous utilisez **System.Web.Mvc** **SelectList** pour artistes et Genres au lieu du **System.Collections.Generic** liste.</span><span class="sxs-lookup"><span data-stu-id="8579c-266">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="8579c-267">**SelectList** est un moyen plus NET pour remplir des listes déroulantes HTML et de gérer des éléments comme la sélection actuelle.</span><span class="sxs-lookup"><span data-stu-id="8579c-267">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="8579c-268">L’instanciation et la configuration ultérieure de ces objets ViewModel dans l’action du contrôleur rend le scénario d’écran de modifier plus claire.</span><span class="sxs-lookup"><span data-stu-id="8579c-268">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="8579c-269">Tâche 2 : création de la vue de modification</span><span class="sxs-lookup"><span data-stu-id="8579c-269">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="8579c-270">Dans cette tâche, vous allez créer un modèle de modifier la vue qui affiche les propriétés de l’album ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="8579c-270">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="8579c-271">Créer le mode édition.</span><span class="sxs-lookup"><span data-stu-id="8579c-271">Create the Edit View.</span></span> <span data-ttu-id="8579c-272">Pour ce faire, cliquez à l’intérieur de la **modifier** méthode d’action et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8579c-272">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="8579c-273">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **modifier**.</span><span class="sxs-lookup"><span data-stu-id="8579c-273">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="8579c-274">Vérifiez le **créer une vue fortement typée** case à cocher et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **afficher la classe de données** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-274">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="8579c-275">Sélectionnez **modifier** à partir de la **modèle de vue de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-275">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="8579c-276">Les autres champs avec leur valeur par défaut et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8579c-276">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="8579c-277">![Ajout d’une vue d’édition](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Ajout d’une vue d’édition")</span><span class="sxs-lookup"><span data-stu-id="8579c-277">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="8579c-278">*Ajout d’une vue d’édition*</span><span class="sxs-lookup"><span data-stu-id="8579c-278">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="8579c-279">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-279">Task 3 - Running the Application</span></span>

<span data-ttu-id="8579c-280">Dans cette tâche, vous allez tester que le **StoreManager** **modifier** afficher la page affiche les valeurs des propriétés de l’album passé comme paramètre.</span><span class="sxs-lookup"><span data-stu-id="8579c-280">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="8579c-281">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-281">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-282">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-282">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-283">Modifier l’URL pour **/StoreManager/Edit/1** pour vérifier que les valeurs des propriétés de l’album passé sont affichées.</span><span class="sxs-lookup"><span data-stu-id="8579c-283">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="8579c-284">![Parcourir vue d’édition Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "modifier la vue Album de navigation")</span><span class="sxs-lookup"><span data-stu-id="8579c-284">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="8579c-285">*Vue d’édition Album de navigation*</span><span class="sxs-lookup"><span data-stu-id="8579c-285">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="8579c-286">Tâche 4 : mise en œuvre les zones déroulantes sur le modèle de l’éditeur de l’Album</span><span class="sxs-lookup"><span data-stu-id="8579c-286">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="8579c-287">Dans cette tâche, vous allez ajouter des listes déroulantes pour le modèle d’affichage créé dans la dernière tâche, afin que l’utilisateur peut sélectionner dans une liste d’artistes et Genres.</span><span class="sxs-lookup"><span data-stu-id="8579c-287">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="8579c-288">Remplacer tout le **Album** fieldset par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8579c-288">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="8579c-289">Un **Html.DropDownList** helper a été ajouté pour afficher les zones déroulantes permettant de choisir les artistes et Genres.</span><span class="sxs-lookup"><span data-stu-id="8579c-289">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="8579c-290">Les paramètres transmis à **Html.DropDownList** sont :</span><span class="sxs-lookup"><span data-stu-id="8579c-290">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="8579c-291">Le nom du champ de formulaire (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="8579c-291">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="8579c-292">Le **SelectList** de valeurs pour la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-292">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="8579c-293">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-293">Task 5 - Running the Application</span></span>

<span data-ttu-id="8579c-294">Dans cette tâche, vous allez tester que le **StoreManager** **modifier** afficher la page affiche les zones déroulantes au lieu de champs de texte artiste et l’ID de Genre.</span><span class="sxs-lookup"><span data-stu-id="8579c-294">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="8579c-295">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-295">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-296">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-296">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-297">Modifier l’URL pour **/StoreManager/Edit/1** pour vérifier qu’elle affiche les zones déroulantes au lieu de champs de texte artiste et l’ID de Genre.</span><span class="sxs-lookup"><span data-stu-id="8579c-297">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="8579c-298">![Modifier la vue Album avec les zones de liste déroulante de navigation](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "modifier la vue Album de navigation avec les zones de liste déroulante")</span><span class="sxs-lookup"><span data-stu-id="8579c-298">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="8579c-299">*Modifier cet affichage navigation Album, avec des listes déroulantes*</span><span class="sxs-lookup"><span data-stu-id="8579c-299">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="8579c-300">Tâche 6 : implémentation de la méthode d’action HTTP-POST modifier</span><span class="sxs-lookup"><span data-stu-id="8579c-300">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="8579c-301">Maintenant que la vue à modifier s’affiche comme prévu, vous devez implémenter la méthode de modifier l’Action HTTP-POST pour enregistrer les modifications apportées à l’Album.</span><span class="sxs-lookup"><span data-stu-id="8579c-301">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="8579c-302">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8579c-302">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="8579c-303">Ouvrez **StoreManagerController** à partir de la **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-303">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="8579c-304">Remplacez **HTTP-POST modifier** action méthode par le code suivant (Notez que la méthode qui doit être remplacée est une version surchargée qui reçoit deux paramètres) :</span><span class="sxs-lookup"><span data-stu-id="8579c-304">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="8579c-305">(Code d’extrait de code - *Validation - Ex3 StoreManagerController HTTP-POST modifier l’action et des formulaires et des programmes d’assistance de ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="8579c-305">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="8579c-306">Cette méthode est exécutée quand l’utilisateur clique sur le **enregistrer** bouton de la vue et effectue une requête HTTP-POST des valeurs de formulaire sur le serveur pour les conserver dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-306">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="8579c-307">Le decorator **[HttpPost]** indique que la méthode doit être utilisée pour les scénarios HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="8579c-307">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="8579c-308">La méthode prend un **Album** objet.</span><span class="sxs-lookup"><span data-stu-id="8579c-308">The method takes an **Album** object.</span></span> <span data-ttu-id="8579c-309">ASP.NET MVC crée automatiquement l’objet Album à partir de la validé &lt;formulaire&gt; valeurs.</span><span class="sxs-lookup"><span data-stu-id="8579c-309">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="8579c-310">La méthode va effectuer ces étapes :</span><span class="sxs-lookup"><span data-stu-id="8579c-310">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="8579c-311">Si le modèle est valide :</span><span class="sxs-lookup"><span data-stu-id="8579c-311">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="8579c-312">Mettre à jour l’entrée de l’album dans le contexte pour la marquer comme un objet modifié.</span><span class="sxs-lookup"><span data-stu-id="8579c-312">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="8579c-313">Enregistrer les modifications et rediriger vers la vue index.</span><span class="sxs-lookup"><span data-stu-id="8579c-313">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="8579c-314">Si le modèle n’est pas valide, il remplit l’élément ViewBag avec la **GenreId** et **ArtistId**, elle retourne la vue avec l’objet Album reçu pour autoriser l’utilisateur effectuer toute mise à jour requise.</span><span class="sxs-lookup"><span data-stu-id="8579c-314">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="8579c-315">Tâche 7 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-315">Task 7 - Running the Application</span></span>

<span data-ttu-id="8579c-316">Dans cette tâche, vous allez tester que le **StoreManager modifier** page de vue enregistre réellement les données Album mis à jour dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-316">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="8579c-317">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-317">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-318">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-318">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-319">Modifier l’URL pour **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="8579c-319">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="8579c-320">Remplacez le titre d’Album par **charge** , puis cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-320">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="8579c-321">Vérifiez que le titre d’album réellement modifié dans la liste des albums.</span><span class="sxs-lookup"><span data-stu-id="8579c-321">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="8579c-322">![Mise à jour un album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "mise à jour d’un album")</span><span class="sxs-lookup"><span data-stu-id="8579c-322">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="8579c-323">*Mise à jour d’un Album*</span><span class="sxs-lookup"><span data-stu-id="8579c-323">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="8579c-324">Exercice 4 : Ajout d’une vue de créer</span><span class="sxs-lookup"><span data-stu-id="8579c-324">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="8579c-325">Maintenant que le **StoreManagerController** prend en charge la **modifier** capacité, dans cet exercice, vous allez apprendre à ajouter un modèle Create View pour vous permettre de stocker les gestionnaires ajouter les nouveaux Albums à l’application.</span><span class="sxs-lookup"><span data-stu-id="8579c-325">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="8579c-326">Comme vous le faisiez avec les fonctionnalités d’édition, vous allez implémenter le scénario de créer à l’aide de deux méthodes distinctes dans le **StoreManagerController** classe :</span><span class="sxs-lookup"><span data-stu-id="8579c-326">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="8579c-327">Une méthode d’action affiche un formulaire vide lorsque le magasin de gestionnaires d’abord visiter le **/StoreManager/créer** URL.</span><span class="sxs-lookup"><span data-stu-id="8579c-327">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="8579c-328">Une deuxième méthode d’action gère le scénario où le directeur du magasin clique sur le **enregistrer** bouton dans le formulaire et soumet à nouveau les valeurs du **/StoreManager/créer** URL sous la forme d’une requête HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="8579c-328">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="8579c-329">Tâche 1 : implémentation de la méthode d’action HTTP-GET créer</span><span class="sxs-lookup"><span data-stu-id="8579c-329">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="8579c-330">Dans cette tâche, vous allez implémenter la version HTTP-GET de la méthode d’action Create pour récupérer une liste de tous les Genres et artistes, empaqueter ces données dans un **StoreManagerViewModel** objet, qui est ensuite transmis à un modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="8579c-330">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="8579c-331">Ouvrez le **commencer** solution situé dans **Source/Ex4-AddingACreateView/début/** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-331">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="8579c-332">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8579c-332">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8579c-333">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="8579c-333">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8579c-334">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8579c-334">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8579c-335">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8579c-335">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8579c-336">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8579c-336">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-337">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-337">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8579c-338">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-338">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8579c-339">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8579c-339">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8579c-340">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="8579c-340">Open **StoreManagerController** class.</span></span> <span data-ttu-id="8579c-341">Pour ce faire, développez le **contrôleurs** et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8579c-341">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="8579c-342">Remplacez le **créer** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8579c-342">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="8579c-343">(Code d’extrait de code - *Validation - action de création de Ex4 StoreManagerController HTTP-GET, les formulaires et les programmes d’assistance de ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="8579c-343">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="8579c-344">Tâche 2 : ajout de la vue de créer</span><span class="sxs-lookup"><span data-stu-id="8579c-344">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="8579c-345">Dans cette tâche, vous allez ajouter le modèle de créer une vue qui affiche un nouveau formulaire Album (vide).</span><span class="sxs-lookup"><span data-stu-id="8579c-345">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="8579c-346">Avec le bouton droit à l’intérieur de la **créer** méthode d’action et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8579c-346">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="8579c-347">Cela affiche la boîte de dialogue Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="8579c-347">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="8579c-348">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **créer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-348">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="8579c-349">Sélectionnez le **créer une vue fortement typée** option et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante et **créer** à partir de la **modèle de vue de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-349">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="8579c-350">Les autres champs avec leur valeur par défaut et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8579c-350">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="8579c-351">![Ajout d’une vue de créer](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "ajout-a-créer-view.png")</span><span class="sxs-lookup"><span data-stu-id="8579c-351">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="8579c-352">*Ajout de la vue de créer*</span><span class="sxs-lookup"><span data-stu-id="8579c-352">*Adding the Create View*</span></span>
3. <span data-ttu-id="8579c-353">Mise à jour la **GenreId** et **ArtistId** champs à utiliser une liste déroulante, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8579c-353">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="8579c-354">Tâche 3 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-354">Task 3 - Running the Application</span></span>

<span data-ttu-id="8579c-355">Dans cette tâche, vous allez tester que le **StoreManager** **créer** afficher la page affiche un formulaire Album vide.</span><span class="sxs-lookup"><span data-stu-id="8579c-355">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="8579c-356">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-356">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-357">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-357">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-358">Modifier l’URL pour **/StoreManager/créer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-358">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="8579c-359">Vérifiez qu’un formulaire vide s’affiche pour remplir les nouvelles propriétés de l’Album.</span><span class="sxs-lookup"><span data-stu-id="8579c-359">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="8579c-360">![Créer un affichage avec un formulaire vide](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "créer une vue avec un formulaire vide")</span><span class="sxs-lookup"><span data-stu-id="8579c-360">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="8579c-361">*Créer un affichage avec un formulaire vide*</span><span class="sxs-lookup"><span data-stu-id="8579c-361">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="8579c-362">Tâche 4 : implémentation de la requête HTTP-POST créer la méthode d’Action</span><span class="sxs-lookup"><span data-stu-id="8579c-362">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="8579c-363">Dans cette tâche, vous allez implémenter la version HTTP-POST de la méthode d’action de création qui sera appelée quand un utilisateur clique sur le **enregistrer** bouton.</span><span class="sxs-lookup"><span data-stu-id="8579c-363">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="8579c-364">La méthode doit enregistrer le nouvel album dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-364">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="8579c-365">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8579c-365">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="8579c-366">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="8579c-366">Open **StoreManagerController** class.</span></span> <span data-ttu-id="8579c-367">Pour ce faire, développez le **contrôleurs** et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8579c-367">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="8579c-368">Remplacez **HTTP-POST créer** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8579c-368">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="8579c-369">(Code d’extrait de code - *Validation - Ex4 StoreManagerController HTTP - POST permet de créer une action, les formulaires et les programmes d’assistance de ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="8579c-369">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="8579c-370">L’action de création est assez similaire à la méthode d’action de modification précédente, mais au lieu de définir l’objet modifié, il est ajouté au contexte.</span><span class="sxs-lookup"><span data-stu-id="8579c-370">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="8579c-371">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-371">Task 5 - Running the Application</span></span>

<span data-ttu-id="8579c-372">Dans cette tâche, vous allez tester que le **StoreManager créer** afficher la page vous permet de créer un nouvel Album et redirige ensuite à la vue d’Index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="8579c-372">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="8579c-373">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-373">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-374">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-374">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-375">Modifier l’URL pour **/StoreManager/créer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-375">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="8579c-376">Remplir les champs de formulaire de données pour un nouvel Album, à celui illustré dans la figure suivante :</span><span class="sxs-lookup"><span data-stu-id="8579c-376">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="8579c-377">![Création d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "création d’un Album")</span><span class="sxs-lookup"><span data-stu-id="8579c-377">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="8579c-378">*Création d’un Album*</span><span class="sxs-lookup"><span data-stu-id="8579c-378">*Creating an Album*</span></span>
3. <span data-ttu-id="8579c-379">Vérifiez que vous êtes redirigé vers la vue Index StoreManager qui inclut le nouvel Album venez de créer.</span><span class="sxs-lookup"><span data-stu-id="8579c-379">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="8579c-380">![Nouvel Album créé](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nouvel Album créé")</span><span class="sxs-lookup"><span data-stu-id="8579c-380">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="8579c-381">*Nouvel Album créé*</span><span class="sxs-lookup"><span data-stu-id="8579c-381">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="8579c-382">Exercice 5 : Gestion de la suppression</span><span class="sxs-lookup"><span data-stu-id="8579c-382">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="8579c-383">La possibilité de supprimer des albums n’est pas encore implémentée.</span><span class="sxs-lookup"><span data-stu-id="8579c-383">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="8579c-384">Voici ce que cet exercice sera sur.</span><span class="sxs-lookup"><span data-stu-id="8579c-384">This is what this exercise will be about.</span></span> <span data-ttu-id="8579c-385">Comme avant, vous allez implémenter le scénario de suppression à l’aide de deux méthodes distinctes dans le **StoreManagerController** classe :</span><span class="sxs-lookup"><span data-stu-id="8579c-385">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="8579c-386">Une méthode d’action affiche un écran de confirmation</span><span class="sxs-lookup"><span data-stu-id="8579c-386">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="8579c-387">Une deuxième méthode d’action gère l’envoi du formulaire</span><span class="sxs-lookup"><span data-stu-id="8579c-387">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="8579c-388">Tâche 1 : implémentation de la méthode d’Action Delete HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="8579c-388">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="8579c-389">Dans cette tâche, vous allez implémenter la version HTTP-GET de la méthode d’action de suppression pour récupérer les informations de l’album.</span><span class="sxs-lookup"><span data-stu-id="8579c-389">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="8579c-390">Ouvrez le **commencer** solution situé dans **début/HandlingDeletion-Ex5/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-390">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="8579c-391">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8579c-391">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8579c-392">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="8579c-392">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8579c-393">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8579c-393">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8579c-394">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8579c-394">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8579c-395">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8579c-395">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-396">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-396">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8579c-397">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-397">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8579c-398">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8579c-398">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8579c-399">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="8579c-399">Open **StoreManagerController** class.</span></span> <span data-ttu-id="8579c-400">Pour ce faire, développez le **contrôleurs** et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8579c-400">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="8579c-401">La contrôleur action Delete est exactement le même que l’action de contrôleur précédente détails du magasin : elle interroge le **album** objet à partir de la base de données à l’aide de la **id** fourni dans l’URL et retourne le approprié **vue**.</span><span class="sxs-lookup"><span data-stu-id="8579c-401">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="8579c-402">Pour ce faire, remplacez la méthode HTTP-GET **supprimer** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8579c-402">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="8579c-403">(Code d’extrait de code - *Validation - Ex5 gestion suppression HTTP-GET supprimer une action, les formulaires et les programmes d’assistance de ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="8579c-403">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="8579c-404">Avec le bouton droit à l’intérieur de la **supprimer** méthode d’action et sélectionnez **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="8579c-404">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="8579c-405">Cela affiche la boîte de dialogue Ajouter une vue.</span><span class="sxs-lookup"><span data-stu-id="8579c-405">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="8579c-406">Dans la boîte de dialogue Ajouter une vue, vérifiez que le nom de la vue est **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-406">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="8579c-407">Sélectionnez le **créer une vue fortement typée** option et sélectionnez **Album (MvcMusicStore.Models)** à partir de la **classe de modèle** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-407">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="8579c-408">Sélectionnez **supprimer** à partir de la **modèle de vue de structure** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="8579c-408">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="8579c-409">Les autres champs avec leur valeur par défaut et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="8579c-409">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="8579c-410">![Ajout d’une vue Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Ajout d’une vue de la suppression")</span><span class="sxs-lookup"><span data-stu-id="8579c-410">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="8579c-411">*Ajout d’une vue de la suppression*</span><span class="sxs-lookup"><span data-stu-id="8579c-411">*Adding a Delete View*</span></span>
6. <span data-ttu-id="8579c-412">Le modèle de suppression affiche tous les champs à partir du modèle.</span><span class="sxs-lookup"><span data-stu-id="8579c-412">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="8579c-413">Vous afficherez le titre de l’album uniquement.</span><span class="sxs-lookup"><span data-stu-id="8579c-413">You will show only the album's title.</span></span> <span data-ttu-id="8579c-414">Pour ce faire, remplacez le contenu de la vue par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8579c-414">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="8579c-415">Tâche 2 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-415">Task 2 - Running the Application</span></span>

<span data-ttu-id="8579c-416">Dans cette tâche, vous allez tester que le **StoreManager** **supprimer** afficher la page affiche un formulaire de suppression de confirmation.</span><span class="sxs-lookup"><span data-stu-id="8579c-416">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="8579c-417">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-417">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-418">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-418">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-419">Modifier l’URL pour **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="8579c-419">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="8579c-420">Sélectionnez un album à supprimer en cliquant sur **supprimer** et vérifiez que le nouvel affichage est téléchargé.</span><span class="sxs-lookup"><span data-stu-id="8579c-420">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="8579c-421">![Suppression d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "suppression d’un Album")</span><span class="sxs-lookup"><span data-stu-id="8579c-421">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="8579c-422">*Suppression d’un Album*</span><span class="sxs-lookup"><span data-stu-id="8579c-422">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="8579c-423">Tâche 3 : implémentation de la méthode d’Action Delete HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="8579c-423">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="8579c-424">Dans cette tâche, vous allez implémenter la version HTTP-POST de la méthode d’action Delete qui sera appelée quand un utilisateur clique sur le **supprimer** bouton.</span><span class="sxs-lookup"><span data-stu-id="8579c-424">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="8579c-425">La méthode doit supprimer l’album dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-425">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="8579c-426">Fermez le navigateur si nécessaire, pour revenir à la fenêtre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8579c-426">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="8579c-427">Ouvrez **StoreManagerController** classe.</span><span class="sxs-lookup"><span data-stu-id="8579c-427">Open **StoreManagerController** class.</span></span> <span data-ttu-id="8579c-428">Pour ce faire, développez le **contrôleurs** et double-cliquez sur **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8579c-428">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="8579c-429">Remplacez **HTTP-POST supprimer** code de méthode d’action avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8579c-429">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="8579c-430">(Code d’extrait de code - *Validation - Ex5 gestion suppression HTTP-POST supprimer une action, les formulaires et les programmes d’assistance de ASP.NET MVC 4*)</span><span class="sxs-lookup"><span data-stu-id="8579c-430">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="8579c-431">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-431">Task 4 - Running the Application</span></span>

<span data-ttu-id="8579c-432">Dans cette tâche, vous allez tester que le **StoreManager Delete** afficher la page vous permet de supprimer un Album et redirige ensuite à la vue d’Index StoreManager.</span><span class="sxs-lookup"><span data-stu-id="8579c-432">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="8579c-433">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-433">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-434">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-434">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-435">Modifier l’URL pour **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="8579c-435">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="8579c-436">Sélectionnez un album à supprimer en cliquant sur **supprimer.**</span><span class="sxs-lookup"><span data-stu-id="8579c-436">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="8579c-437">Confirmer la suppression en cliquant sur **supprimer** bouton :</span><span class="sxs-lookup"><span data-stu-id="8579c-437">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="8579c-438">![Suppression d’un Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "suppression d’un Album")</span><span class="sxs-lookup"><span data-stu-id="8579c-438">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="8579c-439">*Suppression d’un Album*</span><span class="sxs-lookup"><span data-stu-id="8579c-439">*Deleting an Album*</span></span>
3. <span data-ttu-id="8579c-440">Vérifiez que l’album a été supprimée, car elle n’apparaît pas dans le **Index** page.</span><span class="sxs-lookup"><span data-stu-id="8579c-440">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="8579c-441">Exercice 6 : Ajout de la Validation</span><span class="sxs-lookup"><span data-stu-id="8579c-441">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="8579c-442">Actuellement, les formulaires de créer et modifier en place sans effectuer tout type de validation.</span><span class="sxs-lookup"><span data-stu-id="8579c-442">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="8579c-443">Si l’utilisateur quitte un champ obligatoire vide ou des lettres de type dans le champ prix, la première erreur, que vous obtiendrez sera à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-443">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="8579c-444">Vous pouvez ajouter une validation à l’application en ajoutant des Annotations de données à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="8579c-444">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="8579c-445">Grâce aux Annotations de données décrivant les règles appliquées aux propriétés de votre modèle, et ASP.NET MVC s’occupe de l’application et afficher le message approprié pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="8579c-445">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="8579c-446">Tâche 1 : ajouter des Annotations de données</span><span class="sxs-lookup"><span data-stu-id="8579c-446">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="8579c-447">Dans cette tâche, vous allez ajouter au modèle Album qui permettront à la page Créer et modifier des Annotations de données afficher les messages de validation lorsque cela est approprié.</span><span class="sxs-lookup"><span data-stu-id="8579c-447">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="8579c-448">Pour une classe de modèle simple, ajout d’une Annotation de données est uniquement géré en ajoutant un **à l’aide de** instruction pour **System.ComponentModel.DataAnnotation**, puis placer un **[obligatoire]**attribut sur les propriétés appropriées.</span><span class="sxs-lookup"><span data-stu-id="8579c-448">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="8579c-449">L’exemple suivant rendrait la **nom** propriété un champ obligatoire dans la vue.</span><span class="sxs-lookup"><span data-stu-id="8579c-449">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="8579c-450">Cela est un peu plus complexe dans le cas de cette application, où le modèle de données d’entité est généré.</span><span class="sxs-lookup"><span data-stu-id="8579c-450">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="8579c-451">Si vous avez ajouté des Annotations de données directement dans les classes de modèle, elles sont remplacées si vous mettez à jour le modèle à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-451">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="8579c-452">Au lieu de cela, vous pouvez rendre l’utilisation des métadonnées des classes partielles qui existera pour contenir les annotations et sont associés avec le modèle de classes à l’aide la **[MetadataType]** attribut.</span><span class="sxs-lookup"><span data-stu-id="8579c-452">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="8579c-453">Ouvrez le **commencer** solution situé dans **Source/Ex6-AddingValidation/début/** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-453">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="8579c-454">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8579c-454">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8579c-455">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="8579c-455">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8579c-456">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8579c-456">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8579c-457">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8579c-457">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8579c-458">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8579c-458">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-459">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-459">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8579c-460">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-460">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8579c-461">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8579c-461">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8579c-462">Ouvrez le **Album.cs** à partir de la **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-462">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="8579c-463">Remplacez **Album.cs** de contenu avec le code en surbrillance, afin qu’il ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="8579c-463">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-464">La ligne **[DisplayFormat(ConvertEmptyStringToNull=false)]** indique que les chaînes vides à partir du modèle ne sont pas être converties en valeurs null lorsque le champ de données est mise à jour dans la source de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-464">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="8579c-465">Ce paramètre permet d’éviter une exception lors de l’Entity Framework assigne des valeurs null pour le modèle avant de l’Annotation de données valide les champs.</span><span class="sxs-lookup"><span data-stu-id="8579c-465">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="8579c-466">(Code d’extrait de code - *programmes d’assistance de ASP.NET MVC 4 et de formulaires et de Validation - classe de métadonnées partielle Ex6 Album*)</span><span class="sxs-lookup"><span data-stu-id="8579c-466">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="8579c-467">Cela **Album** classe partielle a une **MetadataType** attribut qui pointe vers le **AlbumMetaData** classe pour les Annotations de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-467">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="8579c-468">Voici les attributs de l’Annotation de données que vous utilisez pour annoter le modèle Album :</span><span class="sxs-lookup"><span data-stu-id="8579c-468">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="8579c-469">Obligatoire - indique que la propriété est un champ obligatoire</span><span class="sxs-lookup"><span data-stu-id="8579c-469">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="8579c-470">DisplayName - définit le texte doit être utilisé sur les champs de formulaire et des messages de validation</span><span class="sxs-lookup"><span data-stu-id="8579c-470">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="8579c-471">DisplayFormat - Spécifie le mode d’affichage et de mise en forme des champs de données.</span><span class="sxs-lookup"><span data-stu-id="8579c-471">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="8579c-472">StringLength - définit une longueur maximale pour un champ de chaîne</span><span class="sxs-lookup"><span data-stu-id="8579c-472">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="8579c-473">Plage - donne la valeur minimale et maximale pour un champ numérique</span><span class="sxs-lookup"><span data-stu-id="8579c-473">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="8579c-474">ScaffoldColumn - permet de masquer des champs à partir de formulaires de l’éditeur</span><span class="sxs-lookup"><span data-stu-id="8579c-474">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="8579c-475">Tâche 2 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="8579c-475">Task 2 - Running the Application</span></span>

<span data-ttu-id="8579c-476">Dans cette tâche, vous allez tester la validation que les pages créer et modifier des champs, en utilisant les noms d’affichage choisis dans la dernière tâche.</span><span class="sxs-lookup"><span data-stu-id="8579c-476">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="8579c-477">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="8579c-477">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8579c-478">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-478">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-479">Modifier l’URL pour **/StoreManager/créer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-479">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="8579c-480">Vérifiez que les noms d’affichage correspondent à celles de la classe partielle (comme **Album Art URL** au lieu de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="8579c-480">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="8579c-481">Cliquez sur **créer**, sans le remplir le formulaire.</span><span class="sxs-lookup"><span data-stu-id="8579c-481">Click **Create**, without filling the form.</span></span> <span data-ttu-id="8579c-482">Vérifiez que vous obtenez les messages de validation correspondants.</span><span class="sxs-lookup"><span data-stu-id="8579c-482">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="8579c-483">![Validation des champs dans la page Créer](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "validé des champs dans la page Créer")</span><span class="sxs-lookup"><span data-stu-id="8579c-483">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="8579c-484">*Champs validés dans la page de création*</span><span class="sxs-lookup"><span data-stu-id="8579c-484">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="8579c-485">Vous pouvez vérifier que le même se produit avec le **modifier** page.</span><span class="sxs-lookup"><span data-stu-id="8579c-485">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="8579c-486">Modifier l’URL pour **/StoreManager/Edit/1** et vérifiez que les noms d’affichage correspondent à celles de la classe partielle (comme **Album Art URL** au lieu de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="8579c-486">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="8579c-487">Vide le **titre** et **prix** champs et cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-487">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="8579c-488">Vérifiez que vous obtenez les messages de validation correspondants.</span><span class="sxs-lookup"><span data-stu-id="8579c-488">Verify that you get the corresponding validation messages.</span></span>

    ![Champs validés dans la page de modification](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="8579c-490">*Champs validés dans la page de modification*</span><span class="sxs-lookup"><span data-stu-id="8579c-490">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="8579c-491">Exercice 7 : À l’aide de jQuery non obstructive côté Client</span><span class="sxs-lookup"><span data-stu-id="8579c-491">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="8579c-492">Dans cet exercice, vous allez apprendre à activer la validation jQuery MVC 4 discrète côté client.</span><span class="sxs-lookup"><span data-stu-id="8579c-492">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="8579c-493">JQuery non obstructive utilise le préfixe de données-ajax JavaScript pour appeler des méthodes d’action sur le serveur et non intrusive l’émission de scripts clients inline.</span><span class="sxs-lookup"><span data-stu-id="8579c-493">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="8579c-494">Tâche 1 : exécution de l’Application avant l’activation discrète jQuery</span><span class="sxs-lookup"><span data-stu-id="8579c-494">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="8579c-495">Dans cette tâche, vous allez exécuter l’application avant l’inclusion de jQuery afin de comparer les deux modèles de validation.</span><span class="sxs-lookup"><span data-stu-id="8579c-495">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="8579c-496">Ouvrez le **commencer** solution situé dans **Source/Ex7-UnobtrusivejQueryValidation/début/** dossier.</span><span class="sxs-lookup"><span data-stu-id="8579c-496">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="8579c-497">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="8579c-497">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8579c-498">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="8579c-498">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8579c-499">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8579c-499">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8579c-500">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="8579c-500">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8579c-501">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="8579c-501">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8579c-502">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-502">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8579c-503">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-503">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8579c-504">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="8579c-504">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8579c-505">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="8579c-505">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="8579c-506">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-506">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-507">Parcourir **/StoreManager/créer** et cliquez sur **créer** sans le remplir le formulaire pour vérifier que vous obtenez des messages de validation :</span><span class="sxs-lookup"><span data-stu-id="8579c-507">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="8579c-508">![Validation du client désactivée](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "validation Client désactivée")</span><span class="sxs-lookup"><span data-stu-id="8579c-508">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="8579c-509">*Validation du client désactivée*</span><span class="sxs-lookup"><span data-stu-id="8579c-509">*Client validation disabled*</span></span>
4. <span data-ttu-id="8579c-510">Dans le navigateur, ouvrez le code source HTML :</span><span class="sxs-lookup"><span data-stu-id="8579c-510">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="8579c-511">Tâche 2 : Validation de Client non obstructive l’activation</span><span class="sxs-lookup"><span data-stu-id="8579c-511">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="8579c-512">Dans cette tâche, vous allez activer jQuery **validation client non obstructive** de **Web.config** fichier, qui est par défaut la valeur false dans tous les nouveaux projets ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8579c-512">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="8579c-513">En outre, vous allez ajouter que les références pour rendre le travail de la Validation Client non obstructive de jQuery les scripts nécessaires.</span><span class="sxs-lookup"><span data-stu-id="8579c-513">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="8579c-514">Ouvrez **Web.Config** de fichiers à la racine du projet et vous assurer que le **ClientValidationEnabled** et **UnobtrusiveJavaScriptEnabled** clés ont la valeur **true**.</span><span class="sxs-lookup"><span data-stu-id="8579c-514">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="8579c-515">Vous pouvez également activer la validation côté client par le code à Global.asax.cs pour obtenir les mêmes résultats :</span><span class="sxs-lookup"><span data-stu-id="8579c-515">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="8579c-516">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="8579c-516">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="8579c-517">En outre, vous pouvez affecter ClientValidationEnabled attribut dans n’importe quel contrôleur d’avoir un comportement personnalisé.</span><span class="sxs-lookup"><span data-stu-id="8579c-517">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="8579c-518">Ouvrez **Create.cshtml** à **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="8579c-518">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="8579c-519">Assurez-vous que les fichiers de script suivant **jquery.validate** et **jquery.validate.unobtrusive**, sont référencées dans la vue via la &quot; **~/bundles/jqueryval** &quot; offre groupée.</span><span class="sxs-lookup"><span data-stu-id="8579c-519">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="8579c-520">Toutes ces bibliothèques jQuery sont inclus dans les nouveaux projets de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8579c-520">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="8579c-521">Vous trouverez plus de bibliothèques dans le **/Scripts** dossier de votre projet.</span><span class="sxs-lookup"><span data-stu-id="8579c-521">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="8579c-522">Pour rendre cette validation bibliothèques fonctionnent, vous devez ajouter une référence à la bibliothèque d’infrastructure jQuery.</span><span class="sxs-lookup"><span data-stu-id="8579c-522">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="8579c-523">Étant donné que cette référence est déjà ajoutée à la  **\_Layout.cshtml** fichier, vous n’avez pas besoin pour l’ajouter dans cette vue particulière.</span><span class="sxs-lookup"><span data-stu-id="8579c-523">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="8579c-524">Tâche 3 - Application à l’aide de discrète jQuery Validation en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="8579c-524">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="8579c-525">Dans cette tâche, vous allez tester que le **StoreManager** créer la vue de modèle effectue la validation côté client à l’aide des bibliothèques de jQuery lorsque l’utilisateur crée un nouvel album.</span><span class="sxs-lookup"><span data-stu-id="8579c-525">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="8579c-526">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="8579c-526">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="8579c-527">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="8579c-527">The project starts in the Home page.</span></span> <span data-ttu-id="8579c-528">Parcourir **/StoreManager/créer** et cliquez sur **créer** sans le remplir le formulaire pour vérifier que vous obtenez des messages de validation :</span><span class="sxs-lookup"><span data-stu-id="8579c-528">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="8579c-529">![Validation du client avec jQuery activé](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "validation Client avec jQuery activé")</span><span class="sxs-lookup"><span data-stu-id="8579c-529">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="8579c-530">*Validation du client avec jQuery activé*</span><span class="sxs-lookup"><span data-stu-id="8579c-530">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="8579c-531">Dans le navigateur, ouvrez le code source pour créer une vue :</span><span class="sxs-lookup"><span data-stu-id="8579c-531">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="8579c-532">Pour chaque règle de validation client, jQuery non obstructive ajoute un attribut avec des données-val -*rulename*=&quot;*message*&quot;.</span><span class="sxs-lookup"><span data-stu-id="8579c-532">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="8579c-533">Voici une liste de balises qui discrète jQuery insère dans le champ d’entrée html pour effectuer la validation côté client :</span><span class="sxs-lookup"><span data-stu-id="8579c-533">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="8579c-534">Data-val</span><span class="sxs-lookup"><span data-stu-id="8579c-534">Data-val</span></span>
    > - <span data-ttu-id="8579c-535">Data-val-number</span><span class="sxs-lookup"><span data-stu-id="8579c-535">Data-val-number</span></span>
    > - <span data-ttu-id="8579c-536">Data-val-range</span><span class="sxs-lookup"><span data-stu-id="8579c-536">Data-val-range</span></span>
    > - <span data-ttu-id="8579c-537">Données val plage min / données-val-range-max</span><span class="sxs-lookup"><span data-stu-id="8579c-537">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="8579c-538">Données val requises</span><span class="sxs-lookup"><span data-stu-id="8579c-538">Data-val-required</span></span>
    > - <span data-ttu-id="8579c-539">Longueur de données val</span><span class="sxs-lookup"><span data-stu-id="8579c-539">Data-val-length</span></span>
    > - <span data-ttu-id="8579c-540">Données-val-longueur-max / données val-Longueur-min.</span><span class="sxs-lookup"><span data-stu-id="8579c-540">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="8579c-541">Toutes les valeurs de données sont remplis avec le modèle **Annotation de données**.</span><span class="sxs-lookup"><span data-stu-id="8579c-541">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="8579c-542">Vous pouvez ensuite exécuter toute la logique qui fonctionne au côté serveur côté client.</span><span class="sxs-lookup"><span data-stu-id="8579c-542">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="8579c-543">Par exemple, des attributs de prix est l’annotation de données suivant dans le modèle :</span><span class="sxs-lookup"><span data-stu-id="8579c-543">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="8579c-544">Une fois à l’aide de jQuery non obstructive, le code généré est :</span><span class="sxs-lookup"><span data-stu-id="8579c-544">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8579c-545">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="8579c-545">Summary</span></span>

<span data-ttu-id="8579c-546">À la fin de cet atelier pratique, vous avez appris comment permettre aux utilisateurs de modifier les données stockées dans la base de données à l’aide des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8579c-546">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="8579c-547">Actions de contrôleur comme Index, créer, modifier, supprimer</span><span class="sxs-lookup"><span data-stu-id="8579c-547">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="8579c-548">Fonctionnalité de génération de modèles automatique de ASP.NET MVC pour afficher les propriétés d’une table HTML</span><span class="sxs-lookup"><span data-stu-id="8579c-548">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="8579c-549">Les applications d’assistance HTML personnalisées pour améliorer l’utilisateur expérience</span><span class="sxs-lookup"><span data-stu-id="8579c-549">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="8579c-550">Méthodes d’action qui réagissent appels HTTP-POST ou HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="8579c-550">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="8579c-551">Un modèle partagé éditeur pour afficher les modèles similaires, comme créer et modifier</span><span class="sxs-lookup"><span data-stu-id="8579c-551">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="8579c-552">Les éléments de formulaire tels que les zones déroulantes</span><span class="sxs-lookup"><span data-stu-id="8579c-552">Form elements like drop-downs</span></span>
- <span data-ttu-id="8579c-553">Annotations de données pour la validation de modèle</span><span class="sxs-lookup"><span data-stu-id="8579c-553">Data annotations for Model validation</span></span>
- <span data-ttu-id="8579c-554">Validation côté client à l’aide de la bibliothèque de jQuery non obstructive</span><span class="sxs-lookup"><span data-stu-id="8579c-554">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="8579c-555">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="8579c-555">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="8579c-556">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="8579c-556">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="8579c-557">Les instructions suivantes vous guident à travers les étapes requises pour installer *Visual studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="8579c-557">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="8579c-558">Accédez à [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="8579c-558">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8579c-559">Sinon, si vous avez déjà installé Web Platform Installer, vous pouvez ouvrir il et recherchez le produit &quot; *Visual Studio Express 2012 pour le Web avec Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="8579c-559">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="8579c-560">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="8579c-560">Click on **Install Now**.</span></span> <span data-ttu-id="8579c-561">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="8579c-561">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8579c-562">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="8579c-562">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8579c-563">![Installer Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="8579c-563">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="8579c-564">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="8579c-564">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="8579c-565">Lisez les termes et les licences de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="8579c-565">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="8579c-567">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="8579c-567">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8579c-568">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="8579c-568">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="8579c-570">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="8579c-570">*Installation progress*</span></span>
6. <span data-ttu-id="8579c-571">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="8579c-571">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="8579c-573">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="8579c-573">*Installation completed*</span></span>
7. <span data-ttu-id="8579c-574">Cliquez sur **Exit** fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="8579c-574">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="8579c-575">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="8579c-575">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour la vignette du Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="8579c-577">*VS Express pour la vignette du Web*</span><span class="sxs-lookup"><span data-stu-id="8579c-577">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="8579c-578">Annexe b : à l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="8579c-578">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="8579c-579">Avec des extraits de code, vous avez tout le code que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="8579c-579">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="8579c-580">Le document lab vous indique exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="8579c-580">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="8579c-581">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "des extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="8579c-581">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="8579c-582">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="8579c-582">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="8579c-583">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="8579c-583">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="8579c-584">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="8579c-584">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="8579c-585">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="8579c-585">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="8579c-586">Observez comment IntelliSense affiche les noms des extraits de code de mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="8579c-586">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="8579c-587">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entier est sélectionné).</span><span class="sxs-lookup"><span data-stu-id="8579c-587">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="8579c-588">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="8579c-588">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="8579c-589">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="8579c-589">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="8579c-590">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="8579c-590">*Start typing the snippet name*</span></span>

<span data-ttu-id="8579c-591">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="8579c-591">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="8579c-592">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="8579c-592">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="8579c-593">![Appuyez sur Tab à nouveau et l’extrait de code sont développés](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="8579c-593">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="8579c-594">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="8579c-594">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="8579c-595">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="8579c-595">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="8579c-596">Clic droit où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="8579c-596">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="8579c-597">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="8579c-597">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="8579c-598">Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="8579c-598">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="8579c-599">![Avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="8579c-599">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="8579c-600">*Avec le bouton droit sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="8579c-600">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="8579c-601">![Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="8579c-601">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="8579c-602">*Sélectionnez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="8579c-602">*Pick the relevant snippet from the list, by clicking on it*</span></span>
