---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Database First avec ASP.NET MVC : génération de vues | Microsoft Docs'
author: tfitzmac
description: À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: aspnetcontent
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 9c85b330ac415d8c73d58b31a5108197b754669f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835326"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="538c0-104">EF Database First avec ASP.NET MVC : génération de vues</span><span class="sxs-lookup"><span data-stu-id="538c0-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="538c0-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="538c0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="538c0-106">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="538c0-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="538c0-107">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="538c0-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="538c0-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="538c0-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="538c0-109">Cette partie de la série se concentre sur l’utilisation de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.</span><span class="sxs-lookup"><span data-stu-id="538c0-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="538c0-110">Ajouter une structure</span><span class="sxs-lookup"><span data-stu-id="538c0-110">Add scaffold</span></span>

<span data-ttu-id="538c0-111">Vous êtes prêt à générer du code qui fournira les opérations de données standard pour les classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="538c0-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="538c0-112">Vous ajoutez le code en ajoutant un élément d’une structure.</span><span class="sxs-lookup"><span data-stu-id="538c0-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="538c0-113">Il existe de nombreuses options pour le type de la structure que vous pouvez ajouter ; Dans ce didacticiel, la structure inclura un contrôleur et des vues qui correspondent aux modèles Student et d’inscription, que vous avez créé dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="538c0-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="538c0-114">Pour maintenir la cohérence dans votre projet, vous allez ajouter le nouveau contrôleur à l’objet de **contrôleurs** dossier.</span><span class="sxs-lookup"><span data-stu-id="538c0-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="538c0-115">Avec le bouton droit le **contrôleurs** dossier, puis sélectionnez **ajouter** – **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="538c0-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Ajouter une structure](generating-views/_static/image1.png)

<span data-ttu-id="538c0-117">Sélectionnez le **contrôleur MVC 5 avec vues, utilisant Entity Framework** option.</span><span class="sxs-lookup"><span data-stu-id="538c0-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="538c0-118">Cette option génère le contrôleur et les vues pour la mise à jour, suppression, création et affichage des données dans votre modèle.</span><span class="sxs-lookup"><span data-stu-id="538c0-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Ajoutez un contrôleur mvc](generating-views/_static/image2.png)

<span data-ttu-id="538c0-120">Sélectionnez **étudiant** pour la classe de modèle, puis sélectionnez le **ContosoUniversityEntities** pour la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="538c0-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="538c0-121">Conservez le nom de contrôleur en tant que **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="538c0-121">Keep the controller name as **StudentsController**,</span></span>

![spécifier le contrôleur](generating-views/_static/image3.png)

<span data-ttu-id="538c0-123">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="538c0-123">Click **Add**.</span></span>

<span data-ttu-id="538c0-124">Si vous recevez une erreur, il peut être, car vous n’avez pas généré le projet dans la section précédente.</span><span class="sxs-lookup"><span data-stu-id="538c0-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="538c0-125">Dans ce cas, essayez de générer le projet et puis ajoutez de nouveau l’élément généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="538c0-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="538c0-126">Une fois le processus de génération de code terminé, vous verrez un nouveau contrôleur et les vues dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="538c0-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![afficher les affichages](generating-views/_static/image4.png)

<span data-ttu-id="538c0-128">Effectuez de nouveau les mêmes étapes, mais ajouter une structure pour la classe de l’inscription.</span><span class="sxs-lookup"><span data-stu-id="538c0-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="538c0-129">Lorsque vous avez terminé, vous devez avoir un **EnrollmentsController.cs** fichier et un dossier sous **vues** nommé **les inscriptions** avec Create, Delete, détails, Edit et Index Affichage.</span><span class="sxs-lookup"><span data-stu-id="538c0-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![afficher les affichages](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="538c0-131">Ajouter des liens vers des vues</span><span class="sxs-lookup"><span data-stu-id="538c0-131">Add links to new views</span></span>

<span data-ttu-id="538c0-132">Pour le rendre plus facile pour vous permettent d’accéder à vos nouvelles vues, vous pouvez ajouter deux liens hypertexte dans les vues d’Index pour les étudiants et les inscriptions.</span><span class="sxs-lookup"><span data-stu-id="538c0-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="538c0-133">Ouvrez le fichier à **Views/Home/Index.cshtml**, qui est la page d’accueil de votre site.</span><span class="sxs-lookup"><span data-stu-id="538c0-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="538c0-134">Ajoutez le code suivant sous le jumbotron.</span><span class="sxs-lookup"><span data-stu-id="538c0-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="538c0-135">Pour la méthode ActionLink, le premier paramètre est le texte à afficher dans le lien.</span><span class="sxs-lookup"><span data-stu-id="538c0-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="538c0-136">Le deuxième paramètre est l’action et le troisième paramètre est le nom du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="538c0-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="538c0-137">Par exemple, le premier lien pointe vers l’action de l’Index dans StudentsController.</span><span class="sxs-lookup"><span data-stu-id="538c0-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="538c0-138">Le lien hypertexte est construit à partir de ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="538c0-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="538c0-139">Le premier lien finalement mène à la **Index.cshtml** fichier inclus dans le **vues/étudiants** dossier.</span><span class="sxs-lookup"><span data-stu-id="538c0-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="538c0-140">Afficher les vues de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="538c0-140">Display student views</span></span>

<span data-ttu-id="538c0-141">Vous allez vérifier que le code ajouté à votre projet correctement affiche une liste des étudiants et permet aux utilisateurs de modifier, créer ou supprimer les enregistrements d’étudiants dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="538c0-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="538c0-142">Cliquez sur le **Views/Home/Index.cshtml** et sélectionnez **afficher dans le navigateur**.</span><span class="sxs-lookup"><span data-stu-id="538c0-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="538c0-143">Dans cette page, cliquez sur le lien pour obtenir la liste d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="538c0-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="538c0-144">Dans cette page, notez la liste des étudiants et des liens pour modifier ces données.</span><span class="sxs-lookup"><span data-stu-id="538c0-144">On this page, notice the list of the students and links to modify this data.</span></span>

![liste des étudiants](generating-views/_static/image7.png)

<span data-ttu-id="538c0-146">Cliquez sur le **créer un nouveau** lier et fournir des valeurs pour un nouvel étudiant.</span><span class="sxs-lookup"><span data-stu-id="538c0-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Créer nouvel étudiant](generating-views/_static/image8.png)

<span data-ttu-id="538c0-148">Cliquez sur **créer**et notez le nouvel étudiant est ajouté à votre liste.</span><span class="sxs-lookup"><span data-stu-id="538c0-148">Click **Create**, and notice the new student is added to your list.</span></span>

![liste avec le nouvel étudiant](generating-views/_static/image9.png)

<span data-ttu-id="538c0-150">Sélectionnez le **modifier** lien et modifier certains des valeurs d’un étudiant.</span><span class="sxs-lookup"><span data-stu-id="538c0-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![modifier un étudiant](generating-views/_static/image10.png)

<span data-ttu-id="538c0-152">Cliquez sur **enregistrer**et notez l’enregistrement d’étudiant a été modifié.</span><span class="sxs-lookup"><span data-stu-id="538c0-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="538c0-153">Enfin, sélectionnez le **supprimer** lier et confirmer que vous souhaitez supprimer l’enregistrement en cliquant sur le **supprimer** bouton.</span><span class="sxs-lookup"><span data-stu-id="538c0-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![supprimer les étudiants](generating-views/_static/image11.png)

<span data-ttu-id="538c0-155">Sans écrire de code, vous avez ajouté des vues qui effectuent des opérations courantes sur les données dans la table Student.</span><span class="sxs-lookup"><span data-stu-id="538c0-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="538c0-156">Vous avez peut-être remarqué que l’étiquette de texte pour un champ est basé sur la propriété de base de données (tel que **LastName**) qui n’est pas nécessairement ce que vous voulez afficher sur la page web.</span><span class="sxs-lookup"><span data-stu-id="538c0-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="538c0-157">Par exemple, vous pouvez préférer l’étiquette à être **nom**.</span><span class="sxs-lookup"><span data-stu-id="538c0-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="538c0-158">Vous pouvez résoudre ce affichage problème ultérieurement dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="538c0-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="538c0-159">Afficher les vues de l’inscription</span><span class="sxs-lookup"><span data-stu-id="538c0-159">Display enrollment views</span></span>

<span data-ttu-id="538c0-160">Votre base de données inclut une relation un-à-plusieurs entre les tables Student et d’inscription et une relation un-à-plusieurs entre les tables de la Course et Enrollment.</span><span class="sxs-lookup"><span data-stu-id="538c0-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="538c0-161">Les vues pour l’inscription gèrent correctement ces relations.</span><span class="sxs-lookup"><span data-stu-id="538c0-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="538c0-162">Accédez à la page d’accueil de votre site, puis sélectionnez le **liste des inscriptions** lien, puis le **créer un nouveau** lien.</span><span class="sxs-lookup"><span data-stu-id="538c0-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="538c0-163">La vue affiche un formulaire pour la création d’un nouvel enregistrement d’inscription.</span><span class="sxs-lookup"><span data-stu-id="538c0-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="538c0-164">En particulier, notez que le formulaire contient deux listes déroulantes sont remplies avec les valeurs des tables connexes.</span><span class="sxs-lookup"><span data-stu-id="538c0-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Création d’inscription](generating-views/_static/image12.png)

<span data-ttu-id="538c0-166">En outre, la validation de valeurs fournies est automatiquement appliquée en fonction du type de données du champ.</span><span class="sxs-lookup"><span data-stu-id="538c0-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="538c0-167">Niveau requiert un nombre, donc un message d’erreur s’affiche si vous essayez de fournir une valeur incompatible.</span><span class="sxs-lookup"><span data-stu-id="538c0-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![message de validation](generating-views/_static/image13.png)

<span data-ttu-id="538c0-169">Vous avez vérifié que les vues générées automatiquement permettent aux utilisateurs de travailler avec les données dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="538c0-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="538c0-170">Dans le didacticiel suivant de cette série, vous mettez à jour de la base de données et effectuez les modifications correspondantes dans l’application web.</span><span class="sxs-lookup"><span data-stu-id="538c0-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="538c0-171">[Précédent](creating-the-web-application.md)
> [Suivant](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="538c0-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
