---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "Base de données EF tout d’abord avec ASP.NET MVC : génération de vues | Documents Microsoft"
author: tfitzmac
description: "À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="40bbe-104">Base de données EF tout d’abord avec ASP.NET MVC : génération de vues</span><span class="sxs-lookup"><span data-stu-id="40bbe-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="40bbe-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="40bbe-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="40bbe-106">À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="40bbe-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="40bbe-107">Cette série de didacticiels vous montre comment automatiquement générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer des données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="40bbe-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="40bbe-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="40bbe-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="40bbe-109">Cette partie de la série consacrée à l’aide de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="40bbe-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="40bbe-110">Ajouter une vue de structure</span><span class="sxs-lookup"><span data-stu-id="40bbe-110">Add scaffold</span></span>

<span data-ttu-id="40bbe-111">Vous êtes prêt à générer le code qui fournira les opérations de données standard pour les classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="40bbe-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="40bbe-112">Vous ajoutez le code en ajoutant un élément de la vue de structure.</span><span class="sxs-lookup"><span data-stu-id="40bbe-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="40bbe-113">Il existe de nombreuses options pour le type de la structure que vous pouvez ajouter ; Dans ce didacticiel, la vue inclura un contrôleur et les vues qui correspondent aux modèles Student et d’inscription, que vous avez créé dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="40bbe-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="40bbe-114">Pour maintenir la cohérence dans votre projet, vous allez ajouter le nouveau contrôleur à l’objet existant **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="40bbe-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="40bbe-115">Avec le bouton droit le **contrôleurs** dossier, puis sélectionnez **ajouter** – **un nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="40bbe-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Ajouter une vue de structure](generating-views/_static/image1.png)

<span data-ttu-id="40bbe-117">Sélectionnez le **contrôleur MVC 5 avec vues, utilisant Entity Framework** option.</span><span class="sxs-lookup"><span data-stu-id="40bbe-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="40bbe-118">Cette option génère le contrôleur et les vues pour la mise à jour, suppression, création et affichage des données dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="40bbe-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![ajouter le contrôleur mvc](generating-views/_static/image2.png)

<span data-ttu-id="40bbe-120">Sélectionnez **Student** pour la classe de modèle, puis sélectionnez le **ContosoUniversityEntities** pour la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="40bbe-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="40bbe-121">Conservez le nom de contrôleur en tant que **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="40bbe-121">Keep the controller name as **StudentsController**,</span></span>

![Spécifiez le contrôleur](generating-views/_static/image3.png)

<span data-ttu-id="40bbe-123">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="40bbe-123">Click **Add**.</span></span>

<span data-ttu-id="40bbe-124">Si vous recevez une erreur, il peut être, car vous n’avez pas généré le projet dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="40bbe-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="40bbe-125">Dans ce cas, essayez de générer le projet, puis ajoutez de nouveau l’élément généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="40bbe-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="40bbe-126">Une fois le processus de génération de code est terminé, vous verrez un nouveau contrôleur et les vues dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="40bbe-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![afficher les affichages](generating-views/_static/image4.png)

<span data-ttu-id="40bbe-128">Effectuer les mêmes étapes à nouveau, mais ajouter une structure de la classe de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="40bbe-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="40bbe-129">Lorsque vous avez terminé, vous devez avoir un **EnrollmentsController.cs** fichier et un dossier sous **vues** nommé **inscriptions** avec Create, Delete, détails, modifier et Index Affichage.</span><span class="sxs-lookup"><span data-stu-id="40bbe-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![afficher les affichages](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="40bbe-131">Ajouter des liens vers les nouveaux affichages</span><span class="sxs-lookup"><span data-stu-id="40bbe-131">Add links to new views</span></span>

<span data-ttu-id="40bbe-132">Pour le rendre plus facile pour vous permettent d’accéder à vos vues de nouveau, vous pouvez ajouter deux liens hypertexte aux vues des Index pour les étudiants et les inscriptions.</span><span class="sxs-lookup"><span data-stu-id="40bbe-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="40bbe-133">Ouvrez le fichier **Views/Home/Index.cshtml**, qui est la page d’accueil de votre site.</span><span class="sxs-lookup"><span data-stu-id="40bbe-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="40bbe-134">Ajoutez le code suivant sous le jumbotron.</span><span class="sxs-lookup"><span data-stu-id="40bbe-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="40bbe-135">Pour la méthode ActionLink, le premier paramètre est le texte à afficher dans le lien.</span><span class="sxs-lookup"><span data-stu-id="40bbe-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="40bbe-136">Le deuxième paramètre est l’action et le troisième paramètre est le nom du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="40bbe-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="40bbe-137">Par exemple, le premier lien pointe vers l’action de l’Index dans StudentsController.</span><span class="sxs-lookup"><span data-stu-id="40bbe-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="40bbe-138">Le lien hypertexte est construit à partir de ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="40bbe-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="40bbe-139">Le premier lien finalement mène à la **Index.cshtml** fichier inclus dans le **vues/étudiants** dossier.</span><span class="sxs-lookup"><span data-stu-id="40bbe-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="40bbe-140">Afficher des vues d’étudiant</span><span class="sxs-lookup"><span data-stu-id="40bbe-140">Display student views</span></span>

<span data-ttu-id="40bbe-141">Vous allez vérifier que le code ajouté à votre projet correctement affiche une liste des étudiants et permet aux utilisateurs de modifier, créer ou supprimer les enregistrements de l’étudiant dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="40bbe-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="40bbe-142">Cliquez sur le **Views/Home/Index.cshtml** de fichiers, puis sélectionnez **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="40bbe-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="40bbe-143">Dans cette page, cliquez sur le lien pour obtenir la liste d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="40bbe-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="40bbe-144">Dans cette page, notez la liste des étudiants et des liens pour modifier ces données.</span><span class="sxs-lookup"><span data-stu-id="40bbe-144">On this page, notice the list of the students and links to modify this data.</span></span>

![liste d’étudiants](generating-views/_static/image7.png)

<span data-ttu-id="40bbe-146">Cliquez sur le **créer un nouveau** lier et fournir des valeurs pour un nouvel étudiant.</span><span class="sxs-lookup"><span data-stu-id="40bbe-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Créer nouvel étudiant](generating-views/_static/image8.png)

<span data-ttu-id="40bbe-148">Cliquez sur **créer**et notez le nouvel étudiant est ajouté à votre liste.</span><span class="sxs-lookup"><span data-stu-id="40bbe-148">Click **Create**, and notice the new student is added to your list.</span></span>

![liste avec le nouvel étudiant](generating-views/_static/image9.png)

<span data-ttu-id="40bbe-150">Sélectionnez le **modifier** la liaison et modifier certains des valeurs pour un étudiant.</span><span class="sxs-lookup"><span data-stu-id="40bbe-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![modifier les étudiants](generating-views/_static/image10.png)

<span data-ttu-id="40bbe-152">Cliquez sur **enregistrer**et notez l’enregistrement de l’étudiant a été modifié.</span><span class="sxs-lookup"><span data-stu-id="40bbe-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="40bbe-153">Enfin, sélectionnez le **supprimer** lier et confirmer que vous souhaitez supprimer l’enregistrement en cliquant sur le **supprimer** bouton.</span><span class="sxs-lookup"><span data-stu-id="40bbe-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![supprimer les étudiants](generating-views/_static/image11.png)

<span data-ttu-id="40bbe-155">Sans écrire de code, vous avez ajouté des vues qui effectuent des opérations courantes sur les données dans la table d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="40bbe-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="40bbe-156">Vous avez peut-être remarqué que l’étiquette de texte pour un champ est basé sur la propriété de base de données (tel que **LastName**) qui n’est pas nécessairement ce que vous souhaitez afficher sur la page web.</span><span class="sxs-lookup"><span data-stu-id="40bbe-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="40bbe-157">Par exemple, vous souhaiterez peut-être l’étiquette **nom**.</span><span class="sxs-lookup"><span data-stu-id="40bbe-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="40bbe-158">Résoudre ce problème d’affichage plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="40bbe-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="40bbe-159">Afficher des vues de l’inscription</span><span class="sxs-lookup"><span data-stu-id="40bbe-159">Display enrollment views</span></span>

<span data-ttu-id="40bbe-160">Votre base de données inclut une relation un-à-plusieurs entre les tables Student et d’inscription et d’une relation un-à-plusieurs entre les tables en cours et d’inscription.</span><span class="sxs-lookup"><span data-stu-id="40bbe-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="40bbe-161">Les vues pour l’inscription de gérer correctement ces relations.</span><span class="sxs-lookup"><span data-stu-id="40bbe-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="40bbe-162">Accédez à la page d’accueil de votre site, puis sélectionnez le **liste des inscriptions** lien, puis le **créer un nouveau** lien.</span><span class="sxs-lookup"><span data-stu-id="40bbe-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="40bbe-163">La vue affiche le formulaire de création d’un nouvel enregistrement d’inscription.</span><span class="sxs-lookup"><span data-stu-id="40bbe-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="40bbe-164">En particulier, notez que le formulaire contient deux listes déroulantes sont remplies avec des valeurs de tables connexes.</span><span class="sxs-lookup"><span data-stu-id="40bbe-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![créer l’inscription](generating-views/_static/image12.png)

<span data-ttu-id="40bbe-166">En outre, la validation de valeurs fournies est appliquée automatiquement en fonction du type de données du champ.</span><span class="sxs-lookup"><span data-stu-id="40bbe-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="40bbe-167">Niveau requiert un nombre, afin qu’un message d’erreur est affiché si vous essayez de fournir une valeur incompatible.</span><span class="sxs-lookup"><span data-stu-id="40bbe-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![message de validation](generating-views/_static/image13.png)

<span data-ttu-id="40bbe-169">Vous avez vérifié que les vues générées automatiquement permettent aux utilisateurs de travailler avec les données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="40bbe-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="40bbe-170">Dans l’étape suivante du didacticiel de cette série, vous mettez à jour la base de données et apporte les modifications correspondantes dans l’application web.</span><span class="sxs-lookup"><span data-stu-id="40bbe-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="40bbe-171">[Précédent](creating-the-web-application.md)
[Suivant](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="40bbe-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
