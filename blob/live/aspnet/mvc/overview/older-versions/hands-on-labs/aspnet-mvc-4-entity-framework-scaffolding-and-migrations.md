---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "Génération de modèles automatique ASP.NET MVC 4 Entity Framework et les Migrations | Documents Microsoft"
author: rick-anderson
description: "Si vous êtes familiarisé avec les méthodes de contrôleur ASP.NET MVC 4, ou s’est terminé le &quot;programmes d’assistance, de formulaires et de Validation&quot; atelier pratique, vous devez être conscient..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 15db1589eb90739458b430c35cea38e93e3dec5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="1e083-103">Migrations et la génération de modèles automatique ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="1e083-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>
====================
<span data-ttu-id="1e083-104">par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1e083-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="1e083-105">Si vous êtes familiarisé avec les méthodes de contrôleur ASP.NET MVC 4, ou s’est terminé le &quot;programmes d’assistance, de formulaires et de Validation&quot; atelier pratique, vous devez être conscient que la plupart de la logique permettant de créer, mettre à jour, afficher et supprimer une entité de données qu’il est répété entre l’application.</span><span class="sxs-lookup"><span data-stu-id="1e083-105">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="1e083-106">Ne pas de mentionner que, si votre modèle comporte plusieurs classes à manipuler, vous serez susceptible de prendre une écriture les méthodes d’action POST et GET pour chaque opération de l’entité, ainsi que chacune des vues.</span><span class="sxs-lookup"><span data-stu-id="1e083-106">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>
> 
> <span data-ttu-id="1e083-107">Dans cet atelier, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 pour générer automatiquement la ligne de base CRUD de votre application (création, lecture, mise à jour et suppression).</span><span class="sxs-lookup"><span data-stu-id="1e083-107">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="1e083-108">À partir d’une classe de modèle simple et, sans écrire une seule ligne de code, vous allez créer un contrôleur contenant toutes les opérations CRUD, ainsi que toutes les vues nécessaires.</span><span class="sxs-lookup"><span data-stu-id="1e083-108">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="1e083-109">Après la génération et exécution de la solution simple, vous devez la base de données d’application généré, ainsi que la logique MVC et les vues pour la manipulation de données.</span><span class="sxs-lookup"><span data-stu-id="1e083-109">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>
> 
> <span data-ttu-id="1e083-110">En outre, vous allez apprendre combien il est facile à utiliser Entity Framework Migrations pour effectuer des mises à jour dans toute votre application.</span><span class="sxs-lookup"><span data-stu-id="1e083-110">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="1e083-111">Entity Framework Migrations vous permettent de modifier votre base de données une fois que le modèle a changé étapes simples.</span><span class="sxs-lookup"><span data-stu-id="1e083-111">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="1e083-112">Avec tous ces éléments à l’esprit, vous serez en mesure de créer et gérer des applications web plus efficacement, tirant parti des fonctionnalités plus récentes d’ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="1e083-112">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1e083-113">Objectifs</span><span class="sxs-lookup"><span data-stu-id="1e083-113">Objectives</span></span>

<span data-ttu-id="1e083-114">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="1e083-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="1e083-115">Utiliser génération de modèles automatique ASP.NET pour les opérations CRUD dans les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="1e083-115">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="1e083-116">Modifier le modèle de base de données à l’aide d’Entity Framework Migrations.</span><span class="sxs-lookup"><span data-stu-id="1e083-116">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1e083-117">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="1e083-117">Prerequisites</span></span>

<span data-ttu-id="1e083-118">Vous devez disposer des éléments suivants pour effectuer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="1e083-118">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="1e083-119">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lecture [annexe A](#AppendixA) pour obtenir des instructions sur l’installation).</span><span class="sxs-lookup"><span data-stu-id="1e083-119">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="1e083-120">Installation</span><span class="sxs-lookup"><span data-stu-id="1e083-120">Setup</span></span>

<span data-ttu-id="1e083-121">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="1e083-121">**Installing Code Snippets**</span></span>

<span data-ttu-id="1e083-122">Pour plus de commodité, une grande partie du code que vous gérez le long de cet atelier est disponible sous les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e083-122">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="1e083-123">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="1e083-123">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="1e083-124">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et vous souhaitez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide de l’annexe b :](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="1e083-124">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1e083-125">Exercices</span><span class="sxs-lookup"><span data-stu-id="1e083-125">Exercises</span></span>

<span data-ttu-id="1e083-126">L’exercice suivant composent cet atelier pratique :</span><span class="sxs-lookup"><span data-stu-id="1e083-126">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="1e083-127">À l’aide de la génération de modèles automatique ASP.NET MVC 4 avec Entity Framework Migrations</span><span class="sxs-lookup"><span data-stu-id="1e083-127">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="1e083-128">Cet exercice est accompagné par un **fin** dossier qui contient la solution résultante, vous devez obtenir la fin de l’exercice.</span><span class="sxs-lookup"><span data-stu-id="1e083-128">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="1e083-129">Si vous avez besoin d’aide supplémentaire sur l’utilisation dans l’exercice, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="1e083-129">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="1e083-130">Durée estimée pour effectuer ce laboratoire : **30 minutes**</span><span class="sxs-lookup"><span data-stu-id="1e083-130">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="1e083-131">Exercice 1 : À l’aide de la structure d’ASP.NET MVC 4 avec Entity Framework Migrations</span><span class="sxs-lookup"><span data-stu-id="1e083-131">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="1e083-132">Génération de modèles automatique ASP.NET MVC fournit un moyen rapide pour générer les opérations CRUD de façon standardisée, création de la logique nécessaire qui permet à votre application d’interagir avec la couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="1e083-132">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="1e083-133">Dans cet exercice, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 avec le code tout d’abord créer les méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="1e083-133">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="1e083-134">Ensuite, vous allez apprendre comment mettre à jour votre modèle d’application des modifications dans la base de données à l’aide d’Entity Framework Migrations.</span><span class="sxs-lookup"><span data-stu-id="1e083-134">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="1e083-135">Tâche 1-Création un ASP.NET MVC 4 nouveau projet à l’aide de la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="1e083-135">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="1e083-136">S’il est déjà ouvert, démarrez **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="1e083-136">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="1e083-137">Sélectionnez **fichier | Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="1e083-137">Select **File | New Project**.</span></span> <span data-ttu-id="1e083-138">Dans le nouveau projet de boîte de dialogue, sous le **Visual C# | Web** section, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="1e083-138">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="1e083-139">Nommez le projet à **MVC4andEFMigrations** et définissez l’emplacement de **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** dossier de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="1e083-139">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="1e083-140">Définir le **nom de la Solution** à **commencer** et assurez-vous que **créer le répertoire de solution** est activée.</span><span class="sxs-lookup"><span data-stu-id="1e083-140">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="1e083-141">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e083-141">Click **OK**.</span></span>

    <span data-ttu-id="1e083-142">![Nouvelle boîte de dialogue projet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nouvelle boîte de dialogue projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="1e083-142">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="1e083-143">*Nouvelle boîte de dialogue projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="1e083-143">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="1e083-144">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue Sélectionnez le **Application Internet** modèle et vous assurer que **Razor** est sélectionné **moteur d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="1e083-144">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="1e083-145">Cliquez sur **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="1e083-145">Click **OK** to create the project.</span></span>

    <span data-ttu-id="1e083-146">![Nouvelle Application de Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nouvelle Application de Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="1e083-146">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="1e083-147">*Nouvelle Application de Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="1e083-147">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="1e083-148">Dans l’Explorateur de solutions, cliquez sur **modèles** et sélectionnez **ajouter | Classe** pour créer une personne de classe simple (POCO).</span><span class="sxs-lookup"><span data-stu-id="1e083-148">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="1e083-149">Nommez-le **personne** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e083-149">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="1e083-150">Ouvrez la classe de personne et insérer les propriétés suivantes.</span><span class="sxs-lookup"><span data-stu-id="1e083-150">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="1e083-151">(Code d’extrait de code - *ASP.NET MVC 4 et Entity Framework Migrations - propriétés de personne Ex1*)</span><span class="sxs-lookup"><span data-stu-id="1e083-151">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="1e083-152">Cliquez sur **générer | Générez la Solution** pour enregistrer les modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="1e083-152">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="1e083-153">![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "génération de l’application")</span><span class="sxs-lookup"><span data-stu-id="1e083-153">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="1e083-154">*Génération de l’Application*</span><span class="sxs-lookup"><span data-stu-id="1e083-154">*Building the Application*</span></span>
7. <span data-ttu-id="1e083-155">Dans l’Explorateur de solutions, cliquez sur le dossier controllers, puis sélectionnez **ajouter | Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="1e083-155">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="1e083-156">Nommez le contrôleur *PersonController* et terminer le **les options de génération de modèles automatique** avec les valeurs suivantes.</span><span class="sxs-lookup"><span data-stu-id="1e083-156">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

    1. <span data-ttu-id="1e083-157">Dans le **modèle** la liste déroulante, sélectionnez le **contrôleur MVC avec des actions de lecture/écriture et de vues, utilisant Entity Framework** option.</span><span class="sxs-lookup"><span data-stu-id="1e083-157">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
    2. <span data-ttu-id="1e083-158">Dans le **classe de modèle** la liste déroulante, sélectionnez le **personne** classe.</span><span class="sxs-lookup"><span data-stu-id="1e083-158">In the **Model class** drop-down list, select the **Person** class.</span></span>
    3. <span data-ttu-id="1e083-159">Dans le **classe du contexte de données** liste, sélectionnez  **&lt;nouveau contexte de données... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="1e083-159">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="1e083-160">Choisir un nom et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e083-160">Choose any name and click **OK**.</span></span>
    4. <span data-ttu-id="1e083-161">Dans le **vues** déroulante liste, assurez-vous que **Razor** est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="1e083-161">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

    <span data-ttu-id="1e083-162">![Ajout du contrôleur de personne avec la structure](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Ajout du contrôleur de personne avec la génération de modèles automatique")</span><span class="sxs-lookup"><span data-stu-id="1e083-162">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

    <span data-ttu-id="1e083-163">*Ajout du contrôleur de personne avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="1e083-163">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="1e083-164">Cliquez sur **ajouter** pour créer le nouveau contrôleur de personne avec la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="1e083-164">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="1e083-165">Vous avez généré les actions de contrôleur, ainsi que les vues.</span><span class="sxs-lookup"><span data-stu-id="1e083-165">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="1e083-166">![Après avoir créé le contrôleur de la personne avec la structure](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "après avoir créé le contrôleur de la personne avec la génération de modèles automatique")</span><span class="sxs-lookup"><span data-stu-id="1e083-166">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="1e083-167">*Après avoir créé le contrôleur de la personne avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="1e083-167">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="1e083-168">Ouvrez **PersonController** classe.</span><span class="sxs-lookup"><span data-stu-id="1e083-168">Open **PersonController** class.</span></span> <span data-ttu-id="1e083-169">Notez que les méthodes d’action CRUD complètes ont été générées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="1e083-169">Notice that the full CRUD action methods have been generated automatically.</span></span>

    <span data-ttu-id="1e083-170">![À l’intérieur du contrôleur personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "contrôleur d’à l’intérieur de la personne")</span><span class="sxs-lookup"><span data-stu-id="1e083-170">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

    <span data-ttu-id="1e083-171">*À l’intérieur du contrôleur de la personne*</span><span class="sxs-lookup"><span data-stu-id="1e083-171">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="1e083-172">Tâche 2-exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="1e083-172">Task 2- Running the application</span></span>

<span data-ttu-id="1e083-173">À ce stade, la base de données n’est pas encore créé.</span><span class="sxs-lookup"><span data-stu-id="1e083-173">At this point, the database is not yet created.</span></span> <span data-ttu-id="1e083-174">Dans cette tâche, vous exécutez l’application pour la première fois et les opérations CRUD de test.</span><span class="sxs-lookup"><span data-stu-id="1e083-174">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="1e083-175">La base de données est créé à la volée avec Code First.</span><span class="sxs-lookup"><span data-stu-id="1e083-175">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="1e083-176">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="1e083-176">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="1e083-177">Dans le navigateur, ajoutez **/Person** à l’URL pour ouvrir la page de la personne.</span><span class="sxs-lookup"><span data-stu-id="1e083-177">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="1e083-178">![Exécutez d’abord des application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "tout d’abord l’exécution de l’Application")</span><span class="sxs-lookup"><span data-stu-id="1e083-178">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="1e083-179">*L’application : exécutez d’abord*</span><span class="sxs-lookup"><span data-stu-id="1e083-179">*Application: first run*</span></span>
3. <span data-ttu-id="1e083-180">Vous allez maintenant Explorer les pages de la personne et tester les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="1e083-180">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="1e083-181">Cliquez sur **créer un nouveau** pour ajouter une nouvelle personne.</span><span class="sxs-lookup"><span data-stu-id="1e083-181">Click **Create New** to add a new person.</span></span> <span data-ttu-id="1e083-182">Entrez un prénom et nom de famille et cliquez sur **créer**.</span><span class="sxs-lookup"><span data-stu-id="1e083-182">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="1e083-183">![Ajout d’une nouvelle personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Ajout d’une nouvelle personne")</span><span class="sxs-lookup"><span data-stu-id="1e083-183">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="1e083-184">*Ajout d’une nouvelle personne*</span><span class="sxs-lookup"><span data-stu-id="1e083-184">*Adding a new person*</span></span>
    2. <span data-ttu-id="1e083-185">Dans la liste de la personne, vous pouvez supprimer, modifier ou ajouter des éléments.</span><span class="sxs-lookup"><span data-stu-id="1e083-185">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="1e083-186">![liste de personnes](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "liste de personnes")</span><span class="sxs-lookup"><span data-stu-id="1e083-186">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="1e083-187">*Liste de personnes*</span><span class="sxs-lookup"><span data-stu-id="1e083-187">*Person list*</span></span>
    3. <span data-ttu-id="1e083-188">Cliquez sur **détails** pour ouvrir les détails de la personne.</span><span class="sxs-lookup"><span data-stu-id="1e083-188">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="1e083-189">![Détails de la personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "les détails de la personne")</span><span class="sxs-lookup"><span data-stu-id="1e083-189">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="1e083-190">*Détails de la personne*</span><span class="sxs-lookup"><span data-stu-id="1e083-190">*Person's details*</span></span>
4. <span data-ttu-id="1e083-191">Fermez le navigateur et revenez à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e083-191">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="1e083-192">Notez que vous avez créé l’ensemble CRUD pour l’entité de la personne dans votre application - à partir du modèle pour les vues, sans avoir à écrire une seule ligne de code !</span><span class="sxs-lookup"><span data-stu-id="1e083-192">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="1e083-193">3 de tâche-mise à jour la base de données à l’aide d’Entity Framework Migrations</span><span class="sxs-lookup"><span data-stu-id="1e083-193">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="1e083-194">Dans cette tâche, vous mettrez à jour la base de données à l’aide d’Entity Framework Migrations.</span><span class="sxs-lookup"><span data-stu-id="1e083-194">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="1e083-195">Vous allez découvrir combien il est facile de modifier le modèle et de refléter les modifications apportées à vos bases de données à l’aide de la fonctionnalité d’Entity Framework Migrations.</span><span class="sxs-lookup"><span data-stu-id="1e083-195">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="1e083-196">Ouvrez la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="1e083-196">Open the Package Manager Console.</span></span> <span data-ttu-id="1e083-197">Sélectionnez **outils | Gestionnaire de Package de bibliothèque | Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="1e083-197">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="1e083-198">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1e083-198">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="1e083-199">PMC</span><span class="sxs-lookup"><span data-stu-id="1e083-199">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="1e083-200">![L’activation de Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "autorisant la migration")</span><span class="sxs-lookup"><span data-stu-id="1e083-200">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="1e083-201">*L’activation de migrations*</span><span class="sxs-lookup"><span data-stu-id="1e083-201">*Enabling migrations*</span></span>

    <span data-ttu-id="1e083-202">La commande Enable-Migration crée le **Migrations** dossier, qui contient un script pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="1e083-202">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="1e083-203">![Dossier migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "dossier Migrations")</span><span class="sxs-lookup"><span data-stu-id="1e083-203">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="1e083-204">*Dossier migrations*</span><span class="sxs-lookup"><span data-stu-id="1e083-204">*Migrations folder*</span></span>
3. <span data-ttu-id="1e083-205">Ouvrez le **Configuration.cs** fichier dans le dossier Migrations.</span><span class="sxs-lookup"><span data-stu-id="1e083-205">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="1e083-206">Recherchez le constructeur de classe et modifiez le **AutomaticMigrationsEnabled** valeur *true*.</span><span class="sxs-lookup"><span data-stu-id="1e083-206">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="1e083-207">Ouvrez la classe de personne et ajoutez un attribut pour le nom de personne intermédiaire.</span><span class="sxs-lookup"><span data-stu-id="1e083-207">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="1e083-208">Avec ce nouvel attribut, vous modifiez le modèle.</span><span class="sxs-lookup"><span data-stu-id="1e083-208">With this new attribute, you are changing the model.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="1e083-209">Sélectionnez **générer | Générez la Solution** dans le menu pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="1e083-209">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="1e083-210">![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "génération de l’application")</span><span class="sxs-lookup"><span data-stu-id="1e083-210">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="1e083-211">*Génération de l’application*</span><span class="sxs-lookup"><span data-stu-id="1e083-211">*Building the application*</span></span>
6. <span data-ttu-id="1e083-212">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="1e083-212">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="1e083-213">PMC</span><span class="sxs-lookup"><span data-stu-id="1e083-213">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="1e083-214">Cette commande recherche les modifications apportées à des objets de données, et ensuite, il ajoute les commandes nécessaires pour modifier la base de données en conséquence.</span><span class="sxs-lookup"><span data-stu-id="1e083-214">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="1e083-215">![Ajout d’un deuxième prénom](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Ajout d’un deuxième prénom")</span><span class="sxs-lookup"><span data-stu-id="1e083-215">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="1e083-216">*Ajout d’un deuxième prénom*</span><span class="sxs-lookup"><span data-stu-id="1e083-216">*Adding a middle name*</span></span>
7. <span data-ttu-id="1e083-217">(Facultatif) Vous pouvez exécuter la commande suivante pour générer un script SQL avec la mise à jour différentiel.</span><span class="sxs-lookup"><span data-stu-id="1e083-217">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="1e083-218">Cela vous permettra de mettre à jour manuellement de la base de données (dans ce cas il n’est pas nécessaire), ou appliquez les modifications dans les autres bases de données :</span><span class="sxs-lookup"><span data-stu-id="1e083-218">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="1e083-219">PMC</span><span class="sxs-lookup"><span data-stu-id="1e083-219">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="1e083-220">![Génération d’un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "génération d’un script SQL")</span><span class="sxs-lookup"><span data-stu-id="1e083-220">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="1e083-221">*Génération d’un script SQL*</span><span class="sxs-lookup"><span data-stu-id="1e083-221">*Generating a SQL script*</span></span>

    <span data-ttu-id="1e083-222">![Mise à jour du Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "mise à jour du Script SQL")</span><span class="sxs-lookup"><span data-stu-id="1e083-222">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="1e083-223">*Mise à jour du Script SQL*</span><span class="sxs-lookup"><span data-stu-id="1e083-223">*SQL Script update*</span></span>
8. <span data-ttu-id="1e083-224">Dans la Console du Gestionnaire de Package, entrez la commande suivante pour mettre à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="1e083-224">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="1e083-225">PMC</span><span class="sxs-lookup"><span data-stu-id="1e083-225">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="1e083-226">![Mise à jour de la base de données](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "mise à jour de la base de données")</span><span class="sxs-lookup"><span data-stu-id="1e083-226">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="1e083-227">*Mise à jour de la base de données*</span><span class="sxs-lookup"><span data-stu-id="1e083-227">*Updating the Database*</span></span>

    <span data-ttu-id="1e083-228">Cette opération ajoute le **MiddleName** colonne dans la **personnes** table pour correspondre à la définition actuelle de la **personne** classe.</span><span class="sxs-lookup"><span data-stu-id="1e083-228">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="1e083-229">Une fois la mise à jour de la base de données, cliquez sur le dossier du contrôleur, puis sélectionnez **ajouter | Contrôleur** pour ajouter le contrôleur de personne (complète avec les mêmes valeurs).</span><span class="sxs-lookup"><span data-stu-id="1e083-229">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="1e083-230">Met à jour les méthodes existantes et les vues Ajout du nouvel attribut.</span><span class="sxs-lookup"><span data-stu-id="1e083-230">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="1e083-231">![Ajout d’une mise à jour du contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Ajout d’une mise à jour du contrôleur")</span><span class="sxs-lookup"><span data-stu-id="1e083-231">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="1e083-232">*Mise à jour le contrôleur*</span><span class="sxs-lookup"><span data-stu-id="1e083-232">*Updating the controller*</span></span>
10. <span data-ttu-id="1e083-233">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="1e083-233">Click **Add**.</span></span> <span data-ttu-id="1e083-234">Ensuite, sélectionnez les valeurs **PersonController.cs de remplacer** et **remplacer les vues associées** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1e083-234">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

    ![Ajout d’un remplacement de contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    <span data-ttu-id="1e083-236">*Mise à jour le contrôleur*</span><span class="sxs-lookup"><span data-stu-id="1e083-236">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="1e083-237">Task4 - exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="1e083-237">Task4- Running the application</span></span>

1. <span data-ttu-id="1e083-238">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="1e083-238">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="1e083-239">Ouvrez **/Person**.</span><span class="sxs-lookup"><span data-stu-id="1e083-239">Open **/Person**.</span></span> <span data-ttu-id="1e083-240">Notez que les données ont été préservées, tandis que la colonne de nom de jeune fille a été ajoutée.</span><span class="sxs-lookup"><span data-stu-id="1e083-240">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="1e083-241">![Nom de jeune fille ajouté](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "prénom ajouté")</span><span class="sxs-lookup"><span data-stu-id="1e083-241">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="1e083-242">*Nom de jeune fille ajouté*</span><span class="sxs-lookup"><span data-stu-id="1e083-242">*Middle Name added*</span></span>
3. <span data-ttu-id="1e083-243">Si vous cliquez sur **modifier**, vous serez en mesure d’ajouter un deuxième prénom de la personne en cours.</span><span class="sxs-lookup"><span data-stu-id="1e083-243">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="1e083-244">![Nom de jeune fille édition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "prénom edition")</span><span class="sxs-lookup"><span data-stu-id="1e083-244">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1e083-245">Résumé</span><span class="sxs-lookup"><span data-stu-id="1e083-245">Summary</span></span>

<span data-ttu-id="1e083-246">Dans cet atelier, vous avez appris les étapes simples pour créer des opérations CRUD avec ASP.NET MVC 4 génération de modèles automatique à l’aide de n’importe quelle classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="1e083-246">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="1e083-247">Ensuite, vous avez appris comment effectuer une mise à jour de bout en bout dans votre application - à partir de la base de données pour les vues - à l’aide d’Entity Framework Migrations.</span><span class="sxs-lookup"><span data-stu-id="1e083-247">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="1e083-248">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="1e083-248">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="1e083-249">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="1e083-249">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="1e083-250">Les instructions suivantes vous guident à travers les étapes requises pour installer *Visual studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="1e083-250">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="1e083-251">Accédez à [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="1e083-251">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="1e083-252">Sinon, si vous avez déjà installé Web Platform Installer, vous pouvez ouvrir il et recherchez le produit &quot; *Visual Studio Express 2012 pour le Web avec Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="1e083-252">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="1e083-253">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="1e083-253">Click on **Install Now**.</span></span> <span data-ttu-id="1e083-254">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="1e083-254">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="1e083-255">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="1e083-255">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="1e083-256">![Installer Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="1e083-256">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="1e083-257">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="1e083-257">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="1e083-258">Lisez les termes et les licences de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="1e083-258">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="1e083-260">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="1e083-260">*Accepting the license terms*</span></span>
5. <span data-ttu-id="1e083-261">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="1e083-261">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="1e083-263">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="1e083-263">*Installation progress*</span></span>
6. <span data-ttu-id="1e083-264">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="1e083-264">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="1e083-266">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="1e083-266">*Installation completed*</span></span>
7. <span data-ttu-id="1e083-267">Cliquez sur **Exit** fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="1e083-267">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="1e083-268">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="1e083-268">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour la vignette du Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="1e083-270">*VS Express pour la vignette du Web*</span><span class="sxs-lookup"><span data-stu-id="1e083-270">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="1e083-271">Annexe b : à l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="1e083-271">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="1e083-272">Avec des extraits de code, vous avez tout le code que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="1e083-272">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="1e083-273">Le document lab vous indique exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="1e083-273">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="1e083-274">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "des extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="1e083-274">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="1e083-275">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="1e083-275">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="1e083-276">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="1e083-276">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="1e083-277">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="1e083-277">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="1e083-278">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="1e083-278">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="1e083-279">Observez comment IntelliSense affiche les noms des extraits de code de mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="1e083-279">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="1e083-280">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entier est sélectionné).</span><span class="sxs-lookup"><span data-stu-id="1e083-280">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="1e083-281">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="1e083-281">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="1e083-282">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="1e083-282">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="1e083-283">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="1e083-283">*Start typing the snippet name*</span></span>

<span data-ttu-id="1e083-284">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="1e083-284">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="1e083-285">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="1e083-285">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="1e083-286">![Appuyez sur Tab à nouveau et l’extrait de code sont développés](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="1e083-286">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="1e083-287">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="1e083-287">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="1e083-288">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="1e083-288">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="1e083-289">Clic droit où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="1e083-289">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="1e083-290">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="1e083-290">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="1e083-291">Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="1e083-291">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="1e083-292">![Avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="1e083-292">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="1e083-293">*Avec le bouton droit sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="1e083-293">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="1e083-294">![Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="1e083-294">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="1e083-295">*Sélectionnez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="1e083-295">*Pick the relevant snippet from the list, by clicking on it*</span></span>
