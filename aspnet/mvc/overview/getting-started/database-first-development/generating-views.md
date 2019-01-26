---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutoriel : Générer des vues pour EF Database First avec une application ASP.NET MVC'
description: Cet article se concentre sur l’utilisation de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e1f6646cdf10d293268b92f44b018709e70c0f86
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889780"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="86873-103">Tutoriel : Générer des vues pour EF Database First avec une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="86873-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="86873-104">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="86873-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="86873-105">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="86873-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="86873-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="86873-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="86873-107">Cet article se concentre sur l’utilisation de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="86873-107">This article focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="86873-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="86873-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86873-109">Ajouter une structure</span><span class="sxs-lookup"><span data-stu-id="86873-109">Add scaffold</span></span>
> * <span data-ttu-id="86873-110">Ajouter des liens vers des vues</span><span class="sxs-lookup"><span data-stu-id="86873-110">Add links to new views</span></span>
> * <span data-ttu-id="86873-111">Afficher les vues de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="86873-111">Display student views</span></span>
> * <span data-ttu-id="86873-112">Afficher les vues de l’inscription</span><span class="sxs-lookup"><span data-stu-id="86873-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="86873-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="86873-113">Prerequisite</span></span>

* [<span data-ttu-id="86873-114">Créer des modèles d’application et les données du web</span><span class="sxs-lookup"><span data-stu-id="86873-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="86873-115">Ajouter une structure</span><span class="sxs-lookup"><span data-stu-id="86873-115">Add scaffold</span></span>

<span data-ttu-id="86873-116">Vous êtes prêt à générer du code qui fournira les opérations de données standard pour les classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="86873-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="86873-117">Vous ajoutez le code en ajoutant un élément d’une structure.</span><span class="sxs-lookup"><span data-stu-id="86873-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="86873-118">Il existe de nombreuses options pour le type de la structure que vous pouvez ajouter ; Dans ce didacticiel, la structure inclura un contrôleur et des vues qui correspondent aux modèles Student et d’inscription, que vous avez créé dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="86873-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="86873-119">Pour maintenir la cohérence dans votre projet, vous allez ajouter le nouveau contrôleur à l’objet de **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="86873-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="86873-120">Avec le bouton droit le **contrôleurs** dossier, puis sélectionnez **ajouter** > **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="86873-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="86873-121">Sélectionnez le **contrôleur MVC 5 avec vues, utilisant Entity Framework** option.</span><span class="sxs-lookup"><span data-stu-id="86873-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="86873-122">Cette option génère le contrôleur et les vues pour la mise à jour, suppression, création et affichage des données dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="86873-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Ajoutez un contrôleur mvc](generating-views/_static/image2.png)

<span data-ttu-id="86873-124">Sélectionnez **étudiant (ContosoSite.Models)** pour la classe de modèle et sélectionnez le **ContosoUniversityDataEntities (ContosoSite.Models)** pour la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="86873-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="86873-125">Conservez le nom de contrôleur en tant que **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="86873-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="86873-126">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="86873-126">Click **Add**.</span></span>

<span data-ttu-id="86873-127">Si vous recevez une erreur, il peut être, car vous n’avez pas généré le projet dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="86873-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="86873-128">Dans ce cas, essayez de générer le projet et puis ajoutez de nouveau l’élément généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="86873-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="86873-129">Une fois le processus de génération de code terminé, vous verrez un nouveau contrôleur et les vues dans votre projet **contrôleurs** et **vues** > **étudiants** dossiers .</span><span class="sxs-lookup"><span data-stu-id="86873-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>


<span data-ttu-id="86873-130">Effectuez de nouveau les mêmes étapes, mais ajouter une structure pour le **inscription** classe.</span><span class="sxs-lookup"><span data-stu-id="86873-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="86873-131">Une fois terminé, vous avez un **EnrollmentsController.cs** fichier et un dossier sous **vues** nommé **les inscriptions** avec les vues Create, Delete, détails, Edit et Index.</span><span class="sxs-lookup"><span data-stu-id="86873-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="86873-132">Ajouter des liens vers des vues</span><span class="sxs-lookup"><span data-stu-id="86873-132">Add links to new views</span></span>

<span data-ttu-id="86873-133">Pour le rendre plus facile pour vous permettent d’accéder à vos nouvelles vues, vous pouvez ajouter deux liens hypertexte dans les vues d’Index pour les étudiants et les inscriptions.</span><span class="sxs-lookup"><span data-stu-id="86873-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="86873-134">Ouvrez le fichier à **vues** > **domestique** > *Index.cshtml*, qui est la page d’accueil de votre site.</span><span class="sxs-lookup"><span data-stu-id="86873-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="86873-135">Ajoutez le code suivant sous le jumbotron.</span><span class="sxs-lookup"><span data-stu-id="86873-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="86873-136">Pour la méthode ActionLink, le premier paramètre est le texte à afficher dans le lien.</span><span class="sxs-lookup"><span data-stu-id="86873-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="86873-137">Le deuxième paramètre est l’action et le troisième paramètre est le nom du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="86873-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="86873-138">Par exemple, le premier lien pointe vers l’action de l’Index dans StudentsController.</span><span class="sxs-lookup"><span data-stu-id="86873-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="86873-139">Le lien hypertexte est construit à partir de ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="86873-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="86873-140">Le premier lien finalement mène à la **Index.cshtml** fichier inclus dans le **vues/étudiants** dossier.</span><span class="sxs-lookup"><span data-stu-id="86873-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="86873-141">Afficher les vues de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="86873-141">Display student views</span></span>

<span data-ttu-id="86873-142">Vous allez vérifier que le code ajouté à votre projet correctement affiche une liste des étudiants et permet aux utilisateurs de modifier, créer ou supprimer les enregistrements d’étudiants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="86873-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="86873-143">Avec le bouton droit le **vues** > **accueil** > *Index.cshtml* et sélectionnez **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="86873-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="86873-144">Sur la page d’accueil application, sélectionnez **liste d’étudiants**.</span><span class="sxs-lookup"><span data-stu-id="86873-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="86873-145">Sur le **Index** page, notez la liste des étudiants et des liens pour modifier ces données.</span><span class="sxs-lookup"><span data-stu-id="86873-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="86873-146">Sélectionnez le **créer un nouveau** lier et fournir des valeurs pour un nouvel étudiant.</span><span class="sxs-lookup"><span data-stu-id="86873-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="86873-147">Cliquez sur **créer**et notez le nouvel étudiant est ajouté à votre liste.</span><span class="sxs-lookup"><span data-stu-id="86873-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="86873-148">Sur le **Index** page, sélectionnez le **modifier** lien et modifier certains des valeurs d’un étudiant.</span><span class="sxs-lookup"><span data-stu-id="86873-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="86873-149">Cliquez sur **enregistrer**et notez l’enregistrement d’étudiant a été modifié.</span><span class="sxs-lookup"><span data-stu-id="86873-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="86873-150">Enfin, sélectionnez le **supprimer** lier et confirmer que vous souhaitez supprimer l’enregistrement en cliquant sur le **supprimer** bouton.</span><span class="sxs-lookup"><span data-stu-id="86873-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="86873-151">Sans écrire de code, vous avez ajouté des vues qui effectuent des opérations courantes sur les données dans la table Student.</span><span class="sxs-lookup"><span data-stu-id="86873-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="86873-152">Vous avez peut-être remarqué que l’étiquette de texte pour un champ est basé sur la propriété de base de données (tel que **LastName**) qui n’est pas nécessairement ce que vous voulez afficher sur la page web.</span><span class="sxs-lookup"><span data-stu-id="86873-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="86873-153">Par exemple, vous pouvez préférer l’étiquette à être **nom**.</span><span class="sxs-lookup"><span data-stu-id="86873-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="86873-154">Vous pouvez résoudre ce affichage problème ultérieurement dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="86873-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="86873-155">Afficher les vues de l’inscription</span><span class="sxs-lookup"><span data-stu-id="86873-155">Display enrollment views</span></span>

<span data-ttu-id="86873-156">Votre base de données inclut une relation un-à-plusieurs entre les tables Student et d’inscription et une relation un-à-plusieurs entre les tables de la Course et Enrollment.</span><span class="sxs-lookup"><span data-stu-id="86873-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="86873-157">Les vues pour l’inscription gèrent correctement ces relations.</span><span class="sxs-lookup"><span data-stu-id="86873-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="86873-158">Accédez à la page d’accueil de votre site, puis sélectionnez le **liste des inscriptions** lien, puis le **créer un nouveau** lien.</span><span class="sxs-lookup"><span data-stu-id="86873-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="86873-159">La vue affiche un formulaire pour la création d’un nouvel enregistrement d’inscription.</span><span class="sxs-lookup"><span data-stu-id="86873-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="86873-160">En particulier, notez que le formulaire contient un **CourseID** liste déroulante et un **StudentID** liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="86873-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="86873-161">Les deux sont remplies avec les valeurs des tables connexes.</span><span class="sxs-lookup"><span data-stu-id="86873-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="86873-162">En outre, la validation de valeurs fournies est automatiquement appliquée en fonction du type de données du champ.</span><span class="sxs-lookup"><span data-stu-id="86873-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="86873-163">**Niveau** nécessite un nombre, un message d’erreur s’affiche si vous essayez de fournir une valeur incompatible : *Le niveau de champ doit être un nombre.*</span><span class="sxs-lookup"><span data-stu-id="86873-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="86873-164">Vous avez vérifié que les vues générées automatiquement permettent aux utilisateurs de travailler avec les données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="86873-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="86873-165">Dans le didacticiel suivant de cette série, vous mettez à jour de la base de données et effectuez les modifications correspondantes dans l’application web.</span><span class="sxs-lookup"><span data-stu-id="86873-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86873-166">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="86873-166">Next steps</span></span>

<span data-ttu-id="86873-167">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="86873-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="86873-168">Structure ajoutée</span><span class="sxs-lookup"><span data-stu-id="86873-168">Added scaffold</span></span>
> * <span data-ttu-id="86873-169">Liens ajoutés à des vues</span><span class="sxs-lookup"><span data-stu-id="86873-169">Added links to new views</span></span>
> * <span data-ttu-id="86873-170">Vues affichées étudiant</span><span class="sxs-lookup"><span data-stu-id="86873-170">Displayed student views</span></span>
> * <span data-ttu-id="86873-171">Vues affichées d’inscription</span><span class="sxs-lookup"><span data-stu-id="86873-171">Displayed enrollment views</span></span>

<span data-ttu-id="86873-172">Passez à l’article suivant pour apprendre à modifier la base de données.</span><span class="sxs-lookup"><span data-stu-id="86873-172">Advance to the next article to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="86873-173">Modifier la base de données</span><span class="sxs-lookup"><span data-stu-id="86873-173">Change the database</span></span>](changing-the-database.md)