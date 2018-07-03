---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Affichage d’une Table de base de données (VB) | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, je vais montrer deux méthodes d’affichage d’un ensemble d’enregistrements de base de données. Je montrerai des deux méthodes de mise en forme un jeu d’enregistrements de base de données dans le code HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f5d9062d901f28b1d64f400e13ed5eb0ca788ab8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362490"
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="bcfc6-104">Affichage d’une Table de base de données (VB)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="bcfc6-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="bcfc6-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="bcfc6-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="bcfc6-107">Dans ce didacticiel, je vais montrer deux méthodes d’affichage d’un ensemble d’enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="bcfc6-108">Je montrerai des deux méthodes de mise en forme d’un jeu d’enregistrements de base de données dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="bcfc6-109">Tout d’abord, je montrerai comment vous pouvez mettre en forme les enregistrements de base de données directement dans une vue.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="bcfc6-110">Ensuite, je vais montrer comment vous pouvez tirer parti des vues partielles lors de la mise en forme d’enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="bcfc6-111">L’objectif de ce didacticiel est d’expliquer comment vous pouvez afficher un tableau HTML de base de données dans une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="bcfc6-112">Tout d’abord, vous allez apprendre à utiliser les outils de génération de modèles automatique inclus dans Visual Studio pour générer une vue qui affiche un ensemble d’enregistrements automatiquement.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="bcfc6-113">Ensuite, vous allez apprendre à utiliser un partiel en tant que modèle lors de la mise en forme d’enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="bcfc6-114">Créer les Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="bcfc6-114">Create the Model Classes</span></span>

<span data-ttu-id="bcfc6-115">Nous allons afficher le jeu d’enregistrements de la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="bcfc6-116">La table de base de données de films contient les colonnes suivantes :</span><span class="sxs-lookup"><span data-stu-id="bcfc6-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="bcfc6-117">**Nom de colonne**</span><span class="sxs-lookup"><span data-stu-id="bcfc6-117">**Column Name**</span></span> | <span data-ttu-id="bcfc6-118">**Type de données**</span><span class="sxs-lookup"><span data-stu-id="bcfc6-118">**Data Type**</span></span> | <span data-ttu-id="bcfc6-119">**Autoriser les valeurs null**</span><span class="sxs-lookup"><span data-stu-id="bcfc6-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bcfc6-120">Id</span><span class="sxs-lookup"><span data-stu-id="bcfc6-120">Id</span></span> | <span data-ttu-id="bcfc6-121">Int</span><span class="sxs-lookup"><span data-stu-id="bcfc6-121">Int</span></span> | <span data-ttu-id="bcfc6-122">False</span><span class="sxs-lookup"><span data-stu-id="bcfc6-122">False</span></span> |
| <span data-ttu-id="bcfc6-123">Titre</span><span class="sxs-lookup"><span data-stu-id="bcfc6-123">Title</span></span> | <span data-ttu-id="bcfc6-124">Nvarchar (200)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-124">Nvarchar(200)</span></span> | <span data-ttu-id="bcfc6-125">False</span><span class="sxs-lookup"><span data-stu-id="bcfc6-125">False</span></span> |
| <span data-ttu-id="bcfc6-126">Directeur</span><span class="sxs-lookup"><span data-stu-id="bcfc6-126">Director</span></span> | <span data-ttu-id="bcfc6-127">Nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-127">NVarchar(50)</span></span> | <span data-ttu-id="bcfc6-128">False</span><span class="sxs-lookup"><span data-stu-id="bcfc6-128">False</span></span> |
| <span data-ttu-id="bcfc6-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="bcfc6-129">DateReleased</span></span> | <span data-ttu-id="bcfc6-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="bcfc6-130">DateTime</span></span> | <span data-ttu-id="bcfc6-131">False</span><span class="sxs-lookup"><span data-stu-id="bcfc6-131">False</span></span> |


<span data-ttu-id="bcfc6-132">Afin de représenter la table de films dans notre application ASP.NET MVC, nous devons créer une classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="bcfc6-133">Dans ce didacticiel, nous utilisons Microsoft Entity Framework pour créer nos classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="bcfc6-134">Dans ce didacticiel, nous utilisons Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="bcfc6-135">Toutefois, il est important de comprendre que vous pouvez utiliser une variété de technologies différentes pour interagir avec une base de données à partir d’une application ASP.NET MVC, y compris LINQ to SQL, NHibernate ou ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="bcfc6-136">Suivez ces étapes pour lancer l’Assistant Entity Data Model :</span><span class="sxs-lookup"><span data-stu-id="bcfc6-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="bcfc6-137">Cliquez sur le dossier de modèles dans la fenêtre Explorateur de solutions puis sélectionnez l’option de menu **ajouter, nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="bcfc6-138">Sélectionnez le **données** catégorie, puis sélectionnez le **ADO.NET Entity Data Model** modèle.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="bcfc6-139">Nommez votre modèle de données *MoviesDBModel.edmx* et cliquez sur le **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="bcfc6-140">Après avoir cliqué sur le bouton Ajouter, l’Assistant Entity Data Model s’affiche (voir Figure 1).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="bcfc6-141">Suivez ces étapes pour terminer l’Assistant :</span><span class="sxs-lookup"><span data-stu-id="bcfc6-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="bcfc6-142">Dans le **choisir le contenu du modèle** étape, sélectionnez le **générer à partir de la base de données** option.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="bcfc6-143">Dans le **choisir votre connexion de données** étape, utilisez le *MoviesDB.mdf* connexion de données et le nom *MoviesDBEntities* des paramètres de connexion.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="bcfc6-144">Cliquez sur le **suivant** bouton.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="bcfc6-145">Dans le **choisir vos objets de base de données** étape, développez le nœud Tables, sélectionnez la table de films.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="bcfc6-146">Entrez l’espace de noms *modèles* et cliquez sur le **Terminer** bouton.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="bcfc6-147">[![Création de LINQ aux classes SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="bcfc6-148">**Figure 01**: création de classes LINQ to SQL ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="bcfc6-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="bcfc6-149">Après avoir terminé l’Assistant Entity Data Model, Entity Data Model Designer s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="bcfc6-150">Le concepteur doit afficher l’entité de films (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="bcfc6-151">[![L’Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="bcfc6-152">**Figure 02**: The Entity Data Model Designer ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="bcfc6-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="bcfc6-153">Nous devons apporter une modification avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-153">We need to make one change before we continue.</span></span> <span data-ttu-id="bcfc6-154">L’Assistant génère une classe de modèle nommée *films* qui représente la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="bcfc6-155">Étant donné que nous allons utiliser la classe de films pour représenter un film, nous devons modifier le nom de la classe à être *film* au lieu de *films* (singulier plutôt qu’au pluriel).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="bcfc6-156">Double-cliquez sur le nom de la classe sur l’aire du concepteur et modifiez le nom de la classe films par film.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="bcfc6-157">Après avoir apporté cette modification, cliquez sur le **enregistrer** bouton (l’icône de la disquette) pour générer la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="bcfc6-158">Créer le contrôleur de films</span><span class="sxs-lookup"><span data-stu-id="bcfc6-158">Create the Movies Controller</span></span>

<span data-ttu-id="bcfc6-159">Maintenant que nous avons un moyen pour représenter des enregistrements de notre base de données, nous pouvons créer un contrôleur qui retourne la collection de films.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="bcfc6-160">Dans la fenêtre Explorateur de solutions Visual Studio, cliquez sur le dossier contrôleurs, puis sélectionnez l’option de menu **ajouter, de contrôleur** (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="bcfc6-161">[![Le contrôleur Menu ajouter](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="bcfc6-162">**Figure 03**: ajouter le Menu des contrôleur ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bcfc6-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="bcfc6-163">Lorsque le **ajouter un contrôleur** boîte de dialogue s’affiche, entrez le nom du contrôleur MovieController (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="bcfc6-164">Cliquez sur le **ajouter** pour ajouter le nouveau contrôleur.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="bcfc6-165">[![La boîte de dialogue Ajouter un contrôleur](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="bcfc6-166">**Figure 04**: boîte de dialogue Ajouter un contrôleur The ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="bcfc6-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="bcfc6-167">Nous avons besoin modifier l’action Index() exposée par le contrôleur de films afin qu’il retourne le jeu d’enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="bcfc6-168">Modifier le contrôleur afin qu’il semble que le contrôleur dans le Listing 1.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="bcfc6-169">**Liste 1 – Controllers\MovieController.vb**</span><span class="sxs-lookup"><span data-stu-id="bcfc6-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="bcfc6-170">Dans la liste 1, la classe MoviesDBEntities est utilisée pour représenter la base de données MoviesDB.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="bcfc6-171">L’expression *entités. MovieSet.ToList()* retourne le jeu de tous les films à partir de la table de base de données de films.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="bcfc6-172">Créer la vue</span><span class="sxs-lookup"><span data-stu-id="bcfc6-172">Create the View</span></span>

<span data-ttu-id="bcfc6-173">Pour afficher un jeu d’enregistrements de base de données dans un tableau HTML, le plus simple consiste à tirer parti de la génération de modèles fournie par Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="bcfc6-174">Générez votre application en sélectionnant l’option de menu **créer, générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="bcfc6-175">Vous devez générer votre application avant d’ouvrir le **ajouter une vue** boîte de dialogue ou de vos classes de données n’apparaîtront pas dans la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="bcfc6-176">L’action Index() de clic droit et sélectionnez l’option de menu **ajouter une vue** (voir Figure 5).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="bcfc6-177">[![Ajout d’une vue](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="bcfc6-178">**Figure 05**: ajout d’une vue ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="bcfc6-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="bcfc6-179">Dans le **ajouter une vue** boîte de dialogue, cochez la case intitulée **créer une vue fortement typée**.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="bcfc6-180">Sélectionnez la classe Movie comme le **afficher la classe de données**.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="bcfc6-181">Sélectionnez *liste* en tant que le **afficher le contenu** (voir Figure 6).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="bcfc6-182">Sélection de ces options génère une vue fortement typée qui affiche une liste de films.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="bcfc6-183">[![La boîte de dialogue Ajouter une vue](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="bcfc6-184">**Figure 06**: boîte de dialogue Ajouter une vue The ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="bcfc6-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="bcfc6-185">Après avoir cliqué sur le **ajouter** bouton, la vue dans la liste 2 est généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="bcfc6-186">Cette vue contient le code requis pour effectuer une itération dans la collection de films et afficher chacune des propriétés d’un film.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="bcfc6-187">**Listing 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="bcfc6-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="bcfc6-188">Vous pouvez exécuter l’application en sélectionnant l’option de menu **déboguer, démarrer le débogage** (ou en appuyant sur la touche F5).</span><span class="sxs-lookup"><span data-stu-id="bcfc6-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="bcfc6-189">Exécution de l’application lance Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="bcfc6-190">Si vous accédez à l’URL de /Movie vous verrez la page dans la Figure 7.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="bcfc6-191">[![Une table de films](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="bcfc6-192">**Figure 07**: une table de films ([cliquez pour afficher l’image en taille réelle](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="bcfc6-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="bcfc6-193">Si vous n’aimez pas quoi que ce soit sur l’apparence de la grille d’enregistrements de base de données dans la Figure 7 vous pouvez simplement modifier la vue Index.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="bcfc6-194">Par exemple, vous pouvez modifier le *DateReleased* en-tête à *Date de publication* en modifiant la vue Index.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="bcfc6-195">Créer un modèle avec un partiel</span><span class="sxs-lookup"><span data-stu-id="bcfc6-195">Create a Template with a Partial</span></span>

<span data-ttu-id="bcfc6-196">Lorsqu’une vue est trop compliquée, il est judicieux de commencer avec rupture de la vue dans les vues partielles.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="bcfc6-197">À l’aide de vues partielles rend plus facile à comprendre et à gérer vos vues.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="bcfc6-198">Nous allons créer un partiel qui nous pouvons utiliser comme modèle pour mettre en forme chacun des enregistrements de base de données de film.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="bcfc6-199">Suivez ces étapes pour créer le partielle :</span><span class="sxs-lookup"><span data-stu-id="bcfc6-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="bcfc6-200">Cliquez sur le dossier Views\Movie et sélectionnez l’option de menu **ajouter une vue**.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="bcfc6-201">Cochez la case intitulée *créer une vue partielle (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="bcfc6-202">Nommez le partielle *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="bcfc6-203">Cochez la case intitulée **créer une vue fortement typée**.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="bcfc6-204">Sélectionnez le film comme le *afficher la classe de données*.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="bcfc6-205">Sélectionnez vide en tant que le *afficher le contenu*.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="bcfc6-206">Cliquez sur le **ajouter** pour ajouter le partielle à votre projet.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="bcfc6-207">Après avoir effectué ces étapes, modifiez le MovieTemplate partielle ressemble à la liste 3.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="bcfc6-208">**Liste 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="bcfc6-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="bcfc6-209">Partielle dans le Listing 3 contient un modèle pour une seule ligne d’enregistrements.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="bcfc6-210">La vue Index modifiée sur la liste 4 utilise la MovieTemplate partielle.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="bcfc6-211">**Liste 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="bcfc6-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="bcfc6-212">La vue sur la liste 4 contient un pour chaque boucle qui itère au sein de tous les films.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="bcfc6-213">Pour chaque film, le MovieTemplate partielle est utilisé pour mettre en forme le film.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="bcfc6-214">Le MovieTemplate est rendu en appelant la méthode d’assistance RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="bcfc6-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="bcfc6-215">La vue Index modifiée restitue la même table HTML d’enregistrements de base de données.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="bcfc6-216">Toutefois, la vue a été considérablement simplifiée.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="bcfc6-217">La méthode RenderPartial() est différente de la plupart des autres méthodes d’assistance, car elle ne retourne pas une chaîne.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="bcfc6-218">Par conséquent, vous devez appeler la méthode RenderPartial() à l’aide &lt;Html.RenderPartial() %&gt; au lieu de &lt;% = Html.RenderPartial() %&gt;.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="bcfc6-219">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="bcfc6-219">Summary</span></span>

<span data-ttu-id="bcfc6-220">L’objectif de ce didacticiel est d’expliquer comment vous pouvez afficher un jeu d’enregistrements de base de données dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="bcfc6-221">Tout d’abord, vous avez appris à retourner un jeu d’enregistrements de base de données à partir d’une action de contrôleur en tirant parti de Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="bcfc6-222">Ensuite, vous avez appris à utiliser la structure de Visual Studio pour générer une vue qui affiche une collection d’éléments automatiquement.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="bcfc6-223">Enfin, vous avez appris à simplifier l’affichage en tirant parti d’un partiel.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="bcfc6-224">Vous avez appris comment utiliser un partiel en tant que modèle afin que vous pouvez mettre en forme chaque enregistrement de base de données.</span><span class="sxs-lookup"><span data-stu-id="bcfc6-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bcfc6-225">[Précédent](creating-model-classes-with-linq-to-sql-vb.md)
> [Suivant](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bcfc6-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>
