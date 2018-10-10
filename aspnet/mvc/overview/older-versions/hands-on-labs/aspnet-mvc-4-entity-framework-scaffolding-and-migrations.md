---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Génération de modèles automatique ASP.NET MVC 4 Entity Framework et les Migrations | Microsoft Docs
author: rick-anderson
description: Si vous êtes familiarisé avec les méthodes de contrôleur ASP.NET MVC 4, ou s’est terminé le &quot;Helpers, formulaires et la Validation&quot; atelier pratique, vous devez être conscient...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: bfb1edfcb756706e44126e7e96803bd2e9ce99fb
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911017"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="53cd6-103">Migrations et la génération de modèles automatique ASP.NET MVC 4 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="53cd6-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="53cd6-104">par [Web Camps Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="53cd6-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="53cd6-105">Télécharger le Kit de formation de Web Camps</span><span class="sxs-lookup"><span data-stu-id="53cd6-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="53cd6-106">Si vous êtes familiarisé avec les méthodes de contrôleur ASP.NET MVC 4, ou s’est terminé le &quot;Helpers, formulaires et la Validation&quot; atelier pratique, vous devez être conscient que la plupart de la logique permettant de créer, mettre à jour, répertorier et supprimer une entité de données qu’il est répété entre l’application.</span><span class="sxs-lookup"><span data-stu-id="53cd6-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="53cd6-107">Ne pas de mentionner que, si votre modèle comporte plusieurs classes à manipuler, vous serez susceptible de prendre une écriture les méthodes d’action POST et GET pour chaque opération de l’entité, ainsi que chacune des vues.</span><span class="sxs-lookup"><span data-stu-id="53cd6-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="53cd6-108">Dans cet atelier, vous allez apprendre à utiliser la génération de modèles automatique ASP.NET MVC 4 pour générer automatiquement la ligne de base CRUD de votre application (Create, Read, Update et Delete).</span><span class="sxs-lookup"><span data-stu-id="53cd6-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="53cd6-109">À partir d’une classe de modèle simple et sans écrire une seule ligne de code, vous allez créer un contrôleur qui contiendra toutes les opérations CRUD, ainsi que toutes les vues nécessaires.</span><span class="sxs-lookup"><span data-stu-id="53cd6-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="53cd6-110">Après la génération et exécution de la solution simple, vous disposez de la base de données de l’application générée, ainsi que la logique MVC et les vues pour la manipulation des données.</span><span class="sxs-lookup"><span data-stu-id="53cd6-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="53cd6-111">En outre, vous allez apprendre combien il est facile à utiliser des Migrations Entity Framework pour effectuer des mises à jour du modèle dans toute votre application.</span><span class="sxs-lookup"><span data-stu-id="53cd6-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="53cd6-112">Migrations Entity Framework vous permet de modifier votre base de données une fois que le modèle a changé avec étapes simples.</span><span class="sxs-lookup"><span data-stu-id="53cd6-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="53cd6-113">Avec tous ces cas à l’esprit, vous serez en mesure de créer et maintenir des applications web plus efficacement, en tirant parti des dernières fonctionnalités d’ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="53cd6-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="53cd6-114">Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à partir d’à l’adresse [les versions de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="53cd6-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="53cd6-115">Le projet spécifique à ce laboratoire est disponible à l’adresse [génération de modèles automatique ASP.NET MVC 4 Entity Framework et les Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="53cd6-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="53cd6-116">Objectifs</span><span class="sxs-lookup"><span data-stu-id="53cd6-116">Objectives</span></span>

<span data-ttu-id="53cd6-117">Dans cet atelier pratique, vous allez apprendre comment :</span><span class="sxs-lookup"><span data-stu-id="53cd6-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="53cd6-118">Utiliser génération de modèles automatique ASP.NET pour les opérations CRUD dans les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="53cd6-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="53cd6-119">Modifier le modèle de base de données à l’aide des Migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="53cd6-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="53cd6-120">Prérequis</span><span class="sxs-lookup"><span data-stu-id="53cd6-120">Prerequisites</span></span>

<span data-ttu-id="53cd6-121">Vous devez disposer des éléments suivants pour terminer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="53cd6-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="53cd6-122">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer).</span><span class="sxs-lookup"><span data-stu-id="53cd6-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="53cd6-123">Installation</span><span class="sxs-lookup"><span data-stu-id="53cd6-123">Setup</span></span>

<span data-ttu-id="53cd6-124">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="53cd6-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="53cd6-125">Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53cd6-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="53cd6-126">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="53cd6-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="53cd6-127">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe b :](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="53cd6-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="53cd6-128">Exercices</span><span class="sxs-lookup"><span data-stu-id="53cd6-128">Exercises</span></span>

<span data-ttu-id="53cd6-129">L’exercice suivant composent cet atelier pratique sur :</span><span class="sxs-lookup"><span data-stu-id="53cd6-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="53cd6-130">À l’aide de la génération de modèles automatique ASP.NET MVC 4 avec des Migrations Entity Framework</span><span class="sxs-lookup"><span data-stu-id="53cd6-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="53cd6-131">Cet exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir à l’issue de l’exercice.</span><span class="sxs-lookup"><span data-stu-id="53cd6-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="53cd6-132">Si vous avez besoin d’aide supplémentaire, utilisation de cet exercice, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="53cd6-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="53cd6-133">Durée estimée pour effectuer ce laboratoire : **30 minutes**</span><span class="sxs-lookup"><span data-stu-id="53cd6-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="53cd6-134">Exercice 1 : À l’aide de la structure ASP.NET MVC 4 avec des Migrations Entity Framework</span><span class="sxs-lookup"><span data-stu-id="53cd6-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="53cd6-135">Génération de modèles automatique ASP.NET MVC offre un moyen rapide pour générer les opérations CRUD dans une méthode standardisée, création de la logique nécessaire qui permet à votre application d’interagir avec la couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="53cd6-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="53cd6-136">Dans cet exercice, vous allez apprendre à utiliser la structure ASP.NET MVC 4 avec le code en premier pour créer les méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="53cd6-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="53cd6-137">Ensuite, vous allez apprendre à mettre à jour votre modèle en appliquant les modifications dans la base de données à l’aide des Migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="53cd6-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="53cd6-138">Tâche 1-Création un ASP.NET MVC 4 nouveau projet à l’aide de la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="53cd6-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="53cd6-139">Si elle est déjà ouverte, démarrez **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="53cd6-140">Sélectionnez **fichier | Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-140">Select **File | New Project**.</span></span> <span data-ttu-id="53cd6-141">Dans la nouvelle boîte de dialogue projet, sous la **Visual C# | Web** section, sélectionnez **ASP.NET MVC 4 Web Application**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="53cd6-142">Nommez le projet à **MVC4andEFMigrations** et définissez l’emplacement sur **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** dossier de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="53cd6-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="53cd6-143">Définir le **nom de la Solution** à **commencer** et vérifiez **créer le répertoire pour la solution** est activée.</span><span class="sxs-lookup"><span data-stu-id="53cd6-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="53cd6-144">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-144">Click **OK**.</span></span>

    <span data-ttu-id="53cd6-145">![Nouvelle boîte de dialogue projet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nouvelle boîte de dialogue projet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="53cd6-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="53cd6-146">*Nouvelle boîte de dialogue projet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="53cd6-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="53cd6-147">Dans le **nouveau projet ASP.NET MVC 4** boîte de dialogue Sélectionnez le **Application Internet** modèle et vous assurer que **Razor** est sélectionné **moteur d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="53cd6-148">Cliquez sur **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="53cd6-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="53cd6-149">![Nouvelle Application Internet de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nouvelle Application Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="53cd6-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="53cd6-150">*Nouvelle Application Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="53cd6-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="53cd6-151">Dans l’Explorateur de solutions, cliquez sur **modèles** et sélectionnez **ajouter | Classe** pour créer une personne de classe simple (POCO).</span><span class="sxs-lookup"><span data-stu-id="53cd6-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="53cd6-152">Nommez-le **personne** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="53cd6-153">Ouvrez la classe Person et insérer les propriétés suivantes.</span><span class="sxs-lookup"><span data-stu-id="53cd6-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="53cd6-154">(Code Snippet - *ASP.NET MVC 4 et Migrations Entity Framework - propriétés de personne Ex1*)</span><span class="sxs-lookup"><span data-stu-id="53cd6-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="53cd6-155">Cliquez sur **générer | Générer la Solution** pour enregistrer les modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="53cd6-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="53cd6-156">![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "génération de l’application")</span><span class="sxs-lookup"><span data-stu-id="53cd6-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="53cd6-157">*Génération de l’application*</span><span class="sxs-lookup"><span data-stu-id="53cd6-157">*Building the Application*</span></span>
7. <span data-ttu-id="53cd6-158">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs, puis sélectionnez **ajouter | Contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="53cd6-159">Nommez le contrôleur *PersonController* et terminez le **les options de génération de modèles automatique** avec les valeurs suivantes.</span><span class="sxs-lookup"><span data-stu-id="53cd6-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="53cd6-160">Dans le **modèle** liste déroulante, sélectionnez le **contrôleur MVC avec des actions de lecture/écriture et de vues, utilisant Entity Framework** option.</span><span class="sxs-lookup"><span data-stu-id="53cd6-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="53cd6-161">Dans le **classe de modèle** liste déroulante, sélectionnez le **personne** classe.</span><span class="sxs-lookup"><span data-stu-id="53cd6-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="53cd6-162">Dans le **classe de contexte de données** liste, sélectionnez  **&lt;nouveau contexte de données... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="53cd6-163">Choisissez n’importe quel nom et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="53cd6-164">Dans le **vues** déroulante liste, assurez-vous que l’option **Razor** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="53cd6-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="53cd6-165">![Ajout du contrôleur de la personne avec la structure](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Ajout du contrôleur de la personne avec la génération de modèles automatique")</span><span class="sxs-lookup"><span data-stu-id="53cd6-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="53cd6-166">*Ajout du contrôleur de la personne avec la génération de modèles automatique*</span><span class="sxs-lookup"><span data-stu-id="53cd6-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="53cd6-167">Cliquez sur **ajouter** pour créer le nouveau contrôleur de personne avec la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="53cd6-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="53cd6-168">Vous avez généré les actions de contrôleur, ainsi que les vues.</span><span class="sxs-lookup"><span data-stu-id="53cd6-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="53cd6-169">![Après avoir créé le contrôleur de la personne avec la structure](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "après avoir créé le contrôleur de la personne avec la structure")</span><span class="sxs-lookup"><span data-stu-id="53cd6-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="53cd6-170">*Après avoir créé le contrôleur de la personne avec la structure*</span><span class="sxs-lookup"><span data-stu-id="53cd6-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="53cd6-171">Ouvrez **PersonController** classe.</span><span class="sxs-lookup"><span data-stu-id="53cd6-171">Open **PersonController** class.</span></span> <span data-ttu-id="53cd6-172">Notez que les méthodes d’action CRUD complètes ont été générées automatiquement.</span><span class="sxs-lookup"><span data-stu-id="53cd6-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="53cd6-173">![À l’intérieur du contrôleur de personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "contrôleur d’à l’intérieur de la personne")</span><span class="sxs-lookup"><span data-stu-id="53cd6-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="53cd6-174">*À l’intérieur du contrôleur de personne*</span><span class="sxs-lookup"><span data-stu-id="53cd6-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="53cd6-175">Tâche 2-en cours d’exécution l’application</span><span class="sxs-lookup"><span data-stu-id="53cd6-175">Task 2- Running the application</span></span>

<span data-ttu-id="53cd6-176">À ce stade, la base de données n’est pas encore créé.</span><span class="sxs-lookup"><span data-stu-id="53cd6-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="53cd6-177">Dans cette tâche, vous exécutez l’application pour la première fois et tester les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="53cd6-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="53cd6-178">La base de données est créé à la volée avec Code First.</span><span class="sxs-lookup"><span data-stu-id="53cd6-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="53cd6-179">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="53cd6-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="53cd6-180">Dans le navigateur, ajoutez **/Person** à l’URL pour ouvrir la page de la personne.</span><span class="sxs-lookup"><span data-stu-id="53cd6-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="53cd6-181">![Tout d’abord l’exécution de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "tout d’abord l’exécution de l’Application")</span><span class="sxs-lookup"><span data-stu-id="53cd6-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="53cd6-182">*L’application : exécutez d’abord*</span><span class="sxs-lookup"><span data-stu-id="53cd6-182">*Application: first run*</span></span>
3. <span data-ttu-id="53cd6-183">Maintenant vous parcourez les pages de personne et tester les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="53cd6-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="53cd6-184">Cliquez sur **créer un nouveau** pour ajouter une nouvelle personne.</span><span class="sxs-lookup"><span data-stu-id="53cd6-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="53cd6-185">Entrez un prénom et nom de famille et cliquez sur **créer**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="53cd6-186">![Ajout d’une nouvelle personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Ajout d’une nouvelle personne")</span><span class="sxs-lookup"><span data-stu-id="53cd6-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="53cd6-187">*Ajout d’une nouvelle personne*</span><span class="sxs-lookup"><span data-stu-id="53cd6-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="53cd6-188">Dans la liste de la personne, vous pouvez supprimer, modifier ou ajouter des éléments.</span><span class="sxs-lookup"><span data-stu-id="53cd6-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="53cd6-189">![liste de personnes](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "liste de personnes")</span><span class="sxs-lookup"><span data-stu-id="53cd6-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="53cd6-190">*Liste de personnes*</span><span class="sxs-lookup"><span data-stu-id="53cd6-190">*Person list*</span></span>
    3. <span data-ttu-id="53cd6-191">Cliquez sur **détails** pour ouvrir les détails de la personne.</span><span class="sxs-lookup"><span data-stu-id="53cd6-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="53cd6-192">![Détails de la personne](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "les détails de la personne")</span><span class="sxs-lookup"><span data-stu-id="53cd6-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="53cd6-193">*Détails de la personne*</span><span class="sxs-lookup"><span data-stu-id="53cd6-193">*Person's details*</span></span>
4. <span data-ttu-id="53cd6-194">Fermez le navigateur et revenir à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53cd6-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="53cd6-195">Notez que vous avez créé le CRUD entière pour l’entité person dans toute votre application - à partir du modèle pour les vues, sans avoir à écrire une seule ligne de code !</span><span class="sxs-lookup"><span data-stu-id="53cd6-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="53cd6-196">3 de tâche-mise à jour la base de données à l’aide des Migrations Entity Framework</span><span class="sxs-lookup"><span data-stu-id="53cd6-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="53cd6-197">Dans cette tâche, vous mettrez à jour la base de données à l’aide des Migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="53cd6-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="53cd6-198">Vous allez découvrir combien il est facile à modifier le modèle et de refléter les modifications dans vos bases de données à l’aide de la fonctionnalité de Migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="53cd6-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="53cd6-199">Ouvrez la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="53cd6-199">Open the Package Manager Console.</span></span> <span data-ttu-id="53cd6-200">Sélectionnez **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="53cd6-201">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="53cd6-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="53cd6-202">PMC</span><span class="sxs-lookup"><span data-stu-id="53cd6-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="53cd6-203">![L’activation de Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "autorisant la migration")</span><span class="sxs-lookup"><span data-stu-id="53cd6-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="53cd6-204">*L’activation des migrations*</span><span class="sxs-lookup"><span data-stu-id="53cd6-204">*Enabling migrations*</span></span>

    <span data-ttu-id="53cd6-205">La commande d’activation de la Migration crée le **Migrations** dossier qui contient un script pour initialiser la base de données.</span><span class="sxs-lookup"><span data-stu-id="53cd6-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="53cd6-206">![Dossier migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "dossier Migrations")</span><span class="sxs-lookup"><span data-stu-id="53cd6-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="53cd6-207">*Dossier migrations*</span><span class="sxs-lookup"><span data-stu-id="53cd6-207">*Migrations folder*</span></span>
3. <span data-ttu-id="53cd6-208">Ouvrez le **Configuration.cs** fichier dans le dossier Migrations.</span><span class="sxs-lookup"><span data-stu-id="53cd6-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="53cd6-209">Recherchez le constructeur de classe et modifiez le **AutomaticMigrationsEnabled** valeur *true*.</span><span class="sxs-lookup"><span data-stu-id="53cd6-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="53cd6-210">Ouvrez la classe Person et ajoutez un attribut pour le deuxième prénom de la personne.</span><span class="sxs-lookup"><span data-stu-id="53cd6-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="53cd6-211">Avec ce nouvel attribut, vous modifiez le modèle.</span><span class="sxs-lookup"><span data-stu-id="53cd6-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="53cd6-212">Sélectionnez **générer | Générer la Solution** dans le menu pour générer l’application.</span><span class="sxs-lookup"><span data-stu-id="53cd6-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="53cd6-213">![Génération de l’application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "génération de l’application")</span><span class="sxs-lookup"><span data-stu-id="53cd6-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="53cd6-214">*Génération de l’application*</span><span class="sxs-lookup"><span data-stu-id="53cd6-214">*Building the application*</span></span>
6. <span data-ttu-id="53cd6-215">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="53cd6-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="53cd6-216">PMC</span><span class="sxs-lookup"><span data-stu-id="53cd6-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="53cd6-217">Cette commande recherche les modifications dans les objets de données, et ensuite, il ajoute les commandes nécessaires pour modifier la base de données en conséquence.</span><span class="sxs-lookup"><span data-stu-id="53cd6-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="53cd6-218">![Ajout d’un deuxième prénom](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Ajout d’un deuxième prénom")</span><span class="sxs-lookup"><span data-stu-id="53cd6-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="53cd6-219">*Ajout d’un deuxième prénom*</span><span class="sxs-lookup"><span data-stu-id="53cd6-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="53cd6-220">(Facultatif) Vous pouvez exécuter la commande suivante pour générer un script SQL avec la mise à jour différentiel.</span><span class="sxs-lookup"><span data-stu-id="53cd6-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="53cd6-221">Ainsi, vous pouvez mettre à jour manuellement de la base de données (dans ce cas il n’est pas nécessaire), ou appliquer les modifications dans les autres bases de données :</span><span class="sxs-lookup"><span data-stu-id="53cd6-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="53cd6-222">PMC</span><span class="sxs-lookup"><span data-stu-id="53cd6-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="53cd6-223">![Génération d’un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "génération d’un script SQL")</span><span class="sxs-lookup"><span data-stu-id="53cd6-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="53cd6-224">*Génération d’un script SQL*</span><span class="sxs-lookup"><span data-stu-id="53cd6-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="53cd6-225">![Mise à jour du Script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "mise à jour du Script SQL")</span><span class="sxs-lookup"><span data-stu-id="53cd6-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="53cd6-226">*Mise à jour du Script SQL*</span><span class="sxs-lookup"><span data-stu-id="53cd6-226">*SQL Script update*</span></span>
8. <span data-ttu-id="53cd6-227">Dans la Console du Gestionnaire de Package, entrez la commande suivante pour mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="53cd6-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="53cd6-228">PMC</span><span class="sxs-lookup"><span data-stu-id="53cd6-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="53cd6-229">![La mise à jour de la base de données](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "mise à jour de la base de données")</span><span class="sxs-lookup"><span data-stu-id="53cd6-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="53cd6-230">*La mise à jour de la base de données*</span><span class="sxs-lookup"><span data-stu-id="53cd6-230">*Updating the Database*</span></span>

    <span data-ttu-id="53cd6-231">Cela ajoutera le **MiddleName** colonne dans le **personnes** table pour correspondre à la définition actuelle de la **personne** classe.</span><span class="sxs-lookup"><span data-stu-id="53cd6-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="53cd6-232">Une fois que la base de données est mis à jour, cliquez sur le dossier Controller et sélectionnez **ajouter | Contrôleur** pour ajouter le contrôleur de personne (complète avec les mêmes valeurs).</span><span class="sxs-lookup"><span data-stu-id="53cd6-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="53cd6-233">Cela met à jour les méthodes existantes et les vues Ajout du nouvel attribut.</span><span class="sxs-lookup"><span data-stu-id="53cd6-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="53cd6-234">![Ajout d’une mise à jour du contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Ajout d’une mise à jour du contrôleur")</span><span class="sxs-lookup"><span data-stu-id="53cd6-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="53cd6-235">*La mise à jour le contrôleur*</span><span class="sxs-lookup"><span data-stu-id="53cd6-235">*Updating the controller*</span></span>
10. <span data-ttu-id="53cd6-236">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-236">Click **Add**.</span></span> <span data-ttu-id="53cd6-237">Ensuite, sélectionnez les valeurs **PersonController.cs remplacer** et **remplacer les vues associées** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Ajout d’un remplacement de contrôleur](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="53cd6-239">*La mise à jour le contrôleur*</span><span class="sxs-lookup"><span data-stu-id="53cd6-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="53cd6-240">Task4 - exécution de l’application</span><span class="sxs-lookup"><span data-stu-id="53cd6-240">Task4- Running the application</span></span>

1. <span data-ttu-id="53cd6-241">Appuyez sur **F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="53cd6-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="53cd6-242">Ouvrez **/Person**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-242">Open **/Person**.</span></span> <span data-ttu-id="53cd6-243">Notez que les données ont été préservées, tandis que la colonne Prénom a été ajoutée.</span><span class="sxs-lookup"><span data-stu-id="53cd6-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="53cd6-244">![Deuxième prénom ajouté](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "prénom ajouté")</span><span class="sxs-lookup"><span data-stu-id="53cd6-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="53cd6-245">*Deuxième prénom ajouté*</span><span class="sxs-lookup"><span data-stu-id="53cd6-245">*Middle Name added*</span></span>
3. <span data-ttu-id="53cd6-246">Si vous cliquez sur **modifier**, vous serez en mesure d’ajouter un deuxième prénom à la personne actuelle.</span><span class="sxs-lookup"><span data-stu-id="53cd6-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="53cd6-247">![Deuxième prénom édition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "édition du deuxième prénom")</span><span class="sxs-lookup"><span data-stu-id="53cd6-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="53cd6-248">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="53cd6-248">Summary</span></span>

<span data-ttu-id="53cd6-249">Dans cet atelier, vous avez appris les étapes simples pour créer des opérations CRUD avec ASP.NET MVC 4 génération de modèles automatique à l’aide de n’importe quelle classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="53cd6-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="53cd6-250">Ensuite, vous avez appris à effectuer une mise à jour de bout en bout dans votre application - à partir de la base de données aux vues - à l’aide de Migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="53cd6-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="53cd6-251">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="53cd6-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="53cd6-252">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="53cd6-253">Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="53cd6-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="53cd6-254">Accédez à [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="53cd6-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="53cd6-255">Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="53cd6-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="53cd6-256">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-256">Click on **Install Now**.</span></span> <span data-ttu-id="53cd6-257">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.</span><span class="sxs-lookup"><span data-stu-id="53cd6-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="53cd6-258">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="53cd6-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="53cd6-259">![Installer Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="53cd6-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="53cd6-260">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="53cd6-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="53cd6-261">Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="53cd6-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="53cd6-263">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="53cd6-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="53cd6-264">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="53cd6-264">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="53cd6-266">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="53cd6-266">*Installation progress*</span></span>
6. <span data-ttu-id="53cd6-267">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-267">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="53cd6-269">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="53cd6-269">*Installation completed*</span></span>
7. <span data-ttu-id="53cd6-270">Cliquez sur **Exit** pour fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="53cd6-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="53cd6-271">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="53cd6-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour une vignette de Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="53cd6-273">*VS Express pour une vignette de Web*</span><span class="sxs-lookup"><span data-stu-id="53cd6-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="53cd6-274">Annexe b : utilisation d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="53cd6-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="53cd6-275">Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main.</span><span class="sxs-lookup"><span data-stu-id="53cd6-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="53cd6-276">Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="53cd6-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="53cd6-277">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="53cd6-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="53cd6-278">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="53cd6-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="53cd6-279">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="53cd6-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="53cd6-280">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="53cd6-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="53cd6-281">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="53cd6-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="53cd6-282">Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.</span><span class="sxs-lookup"><span data-stu-id="53cd6-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="53cd6-283">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).</span><span class="sxs-lookup"><span data-stu-id="53cd6-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="53cd6-284">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="53cd6-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="53cd6-285">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="53cd6-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="53cd6-286">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="53cd6-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="53cd6-287">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="53cd6-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="53cd6-288">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="53cd6-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="53cd6-289">![Appuyez sur Tab à nouveau et l’extrait de code seront développe](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="53cd6-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="53cd6-290">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="53cd6-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="53cd6-291">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="53cd6-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="53cd6-292">Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="53cd6-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="53cd6-293">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="53cd6-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="53cd6-294">Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="53cd6-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="53cd6-295">![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="53cd6-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="53cd6-296">*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="53cd6-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="53cd6-297">![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="53cd6-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="53cd6-298">*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="53cd6-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
