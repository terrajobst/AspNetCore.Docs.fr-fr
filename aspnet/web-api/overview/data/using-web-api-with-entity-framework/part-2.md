---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: "Ajoutez des modèles et des contrôleurs | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: b75eae11fd99b60864256f79d4770a3007487964
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="add-models-and-controllers"></a><span data-ttu-id="6e903-102">Ajoutez des modèles et des contrôleurs</span><span class="sxs-lookup"><span data-stu-id="6e903-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="6e903-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6e903-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6e903-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="6e903-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="6e903-105">Dans cette section, vous allez ajouter des classes de modèle qui définissent les entités de base de données.</span><span class="sxs-lookup"><span data-stu-id="6e903-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="6e903-106">Ensuite, vous allez ajouter des contrôleurs Web API qui effectuent des opérations CRUD sur les entités.</span><span class="sxs-lookup"><span data-stu-id="6e903-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="6e903-107">Ajouter des Classes de modèle</span><span class="sxs-lookup"><span data-stu-id="6e903-107">Add Model Classes</span></span>

<span data-ttu-id="6e903-108">Dans ce didacticiel, nous allons créer la base de données à l’aide de l’approche « Code First » pour Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="6e903-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="6e903-109">Code First, vous écrivez des classes c# qui correspondent aux tables de base de données et EF crée la base de données.</span><span class="sxs-lookup"><span data-stu-id="6e903-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="6e903-110">(Pour plus d’informations, consultez [approches de développement Entity Framework](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="6e903-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="6e903-111">Nous allons commencer en définissant les objets de domaine en tant que POCOs (des objets brut-old CLR).</span><span class="sxs-lookup"><span data-stu-id="6e903-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="6e903-112">Nous allons créer les POCOs suivants :</span><span class="sxs-lookup"><span data-stu-id="6e903-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="6e903-113">Auteur</span><span class="sxs-lookup"><span data-stu-id="6e903-113">Author</span></span>
- <span data-ttu-id="6e903-114">Livre</span><span class="sxs-lookup"><span data-stu-id="6e903-114">Book</span></span>

<span data-ttu-id="6e903-115">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier de modèles.</span><span class="sxs-lookup"><span data-stu-id="6e903-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="6e903-116">Sélectionnez **ajouter**, puis sélectionnez **classe**.</span><span class="sxs-lookup"><span data-stu-id="6e903-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="6e903-117">Nommez la classe `Author`.</span><span class="sxs-lookup"><span data-stu-id="6e903-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="6e903-118">Remplacez tout le code réutilisable dans Author.cs avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="6e903-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="6e903-119">Ajouter une autre classe nommée `Book`, avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="6e903-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="6e903-120">Entity Framework utilisera ces modèles pour créer des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="6e903-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="6e903-121">Pour chaque modèle, le `Id` propriété deviendra la colonne de clé primaire de la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="6e903-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="6e903-122">Dans la classe Book, le `AuthorId` définit une clé étrangère dans la `Author` table.</span><span class="sxs-lookup"><span data-stu-id="6e903-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="6e903-123">(Par souci de simplicité, je suppose qu’un seul auteur de chaque livre.) La classe book contient également une propriété de navigation à la `Author`.</span><span class="sxs-lookup"><span data-stu-id="6e903-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="6e903-124">Vous pouvez utiliser la propriété de navigation pour accéder aux connexe `Author` dans le code.</span><span class="sxs-lookup"><span data-stu-id="6e903-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="6e903-125">Plus d’informations sur les propriétés de navigation dans la partie 4, dire [la gestion des Relations d’entité](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="6e903-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="6e903-126">Ajouter des contrôleurs de l’API Web</span><span class="sxs-lookup"><span data-stu-id="6e903-126">Add Web API Controllers</span></span>

<span data-ttu-id="6e903-127">Dans cette section, nous allons ajouter des contrôleurs Web API qui prennent en charge les opérations CRUD (créer, lire, mettre à jour et supprimer).</span><span class="sxs-lookup"><span data-stu-id="6e903-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="6e903-128">Les contrôleurs utilisera Entity Framework pour communiquer avec la couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="6e903-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="6e903-129">Tout d’abord, vous pouvez supprimer le fichier Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="6e903-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="6e903-130">Ce fichier contient un contrôleur d’API Web exemple, mais vous n’en avez pas besoin de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="6e903-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="6e903-131">Ensuite, générez le projet.</span><span class="sxs-lookup"><span data-stu-id="6e903-131">Next, build the project.</span></span> <span data-ttu-id="6e903-132">La génération de modèles automatique API Web utilise la réflexion pour rechercher les classes du modèle, par conséquent, il doit l’assembly compilé.</span><span class="sxs-lookup"><span data-stu-id="6e903-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="6e903-133">Dans l’Explorateur de solutions, cliquez sur le dossier contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6e903-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="6e903-134">Sélectionnez **ajouter**, puis sélectionnez **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="6e903-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="6e903-135">Dans le **ajouter une vue de structure** boîte de dialogue, sélectionnez « API Web 2 contrôleur avec actions, utilisant Entity Framework ».</span><span class="sxs-lookup"><span data-stu-id="6e903-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="6e903-136">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e903-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="6e903-137">Dans le **ajouter un contrôleur** boîte de dialogue, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e903-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="6e903-138">Dans le **classe de modèle** liste déroulante, sélectionnez le `Author` classe.</span><span class="sxs-lookup"><span data-stu-id="6e903-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="6e903-139">(Si vous ne voyez pas répertoriés dans la liste déroulante, assurez-vous que vous avez créé le projet.)</span><span class="sxs-lookup"><span data-stu-id="6e903-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="6e903-140">Consultez « Utilisez actions asynchrones du contrôleur ».</span><span class="sxs-lookup"><span data-stu-id="6e903-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="6e903-141">Laissez le nom du contrôleur en tant que &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="6e903-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="6e903-142">Cliquez sur plus (+) situé en regard **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="6e903-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="6e903-143">Dans le **nouveau contexte de données** boîte de dialogue, conservez le nom par défaut et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e903-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="6e903-144">Cliquez sur **ajouter** pour terminer le **ajouter un contrôleur** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="6e903-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="6e903-145">La boîte de dialogue ajoute deux classes à votre projet :</span><span class="sxs-lookup"><span data-stu-id="6e903-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="6e903-146">`AuthorsController`définit un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="6e903-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="6e903-147">Le contrôleur implémente l’API REST que les clients utilisent pour effectuer des opérations CRUD sur la liste des auteurs.</span><span class="sxs-lookup"><span data-stu-id="6e903-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="6e903-148">`BookServiceContext`gère des objets d’entité pendant l’exécution, ce qui inclut le remplissage des objets avec des données à partir d’une base de données, le suivi des modifications et conserver les données de la base de données.</span><span class="sxs-lookup"><span data-stu-id="6e903-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="6e903-149">Il hérite de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6e903-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="6e903-150">À ce stade, régénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="6e903-150">At this point, build the project again.</span></span> <span data-ttu-id="6e903-151">Ensuite suivre les mêmes étapes pour ajouter un contrôleur d’API pour `Book` entités.</span><span class="sxs-lookup"><span data-stu-id="6e903-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="6e903-152">Cette fois-ci, sélectionnez `Book` pour la classe de modèle, puis sélectionnez existants `BookServiceContext` classe de la classe de contexte de données.</span><span class="sxs-lookup"><span data-stu-id="6e903-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="6e903-153">(Ne pas créer un nouveau contexte de données.) Cliquez sur **ajouter** pour ajouter le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e903-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

>[!div class="step-by-step"]
<span data-ttu-id="6e903-154">[Précédent](part-1.md)
[Suivant](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="6e903-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
