---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Base de données EF tout d’abord avec ASP.NET MVC : modification de la base de données | Documents Microsoft'
author: tfitzmac
description: À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879320"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="05659-104">Base de données EF tout d’abord avec ASP.NET MVC : modification de la base de données</span><span class="sxs-lookup"><span data-stu-id="05659-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="05659-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="05659-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="05659-106">À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="05659-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="05659-107">Cette série de didacticiels vous montre comment automatiquement générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer des données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="05659-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="05659-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="05659-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="05659-109">Cette partie de la série consacrée qui effectue une mise à jour à la structure de base de données et la propagation de cette modification dans l’ensemble de l’application web.</span><span class="sxs-lookup"><span data-stu-id="05659-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="05659-110">Ajouter une colonne</span><span class="sxs-lookup"><span data-stu-id="05659-110">Add a column</span></span>

<span data-ttu-id="05659-111">Si vous mettez à jour la structure d’une table dans votre base de données, vous devez vous assurer que votre modification est propagée vers le modèle de données, vues et contrôleur.</span><span class="sxs-lookup"><span data-stu-id="05659-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="05659-112">Pour ce didacticiel, vous allez ajouter une nouvelle colonne à la table de l’étudiant pour enregistrer le deuxième prénom de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="05659-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="05659-113">Pour ajouter cette colonne, ouvrez le projet de base de données, ouvrez le fichier Student.sql.</span><span class="sxs-lookup"><span data-stu-id="05659-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="05659-114">Par le biais du concepteur ou dans le code T-SQL, ajoutez une colonne nommée **MiddleName** qui est un nvarchar (50) et autorise les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="05659-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![ajouter le nom de jeune fille](changing-the-database/_static/image1.png)

<span data-ttu-id="05659-116">Déployer cette modification sur votre base de données locale en démarrant votre projet de base de données (ou F5).</span><span class="sxs-lookup"><span data-stu-id="05659-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="05659-117">Le nouveau champ est ajouté à la table.</span><span class="sxs-lookup"><span data-stu-id="05659-117">The new field is added to the table.</span></span> <span data-ttu-id="05659-118">Si vous ne voyez pas dans l’Explorateur d’objets SQL Server, cliquez sur le bouton Actualiser dans le volet.</span><span class="sxs-lookup"><span data-stu-id="05659-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![afficher la nouvelle colonne](changing-the-database/_static/image2.png)

<span data-ttu-id="05659-120">La nouvelle colonne existe dans la table de base de données, mais il n’existe pas dans la classe de modèle de données.</span><span class="sxs-lookup"><span data-stu-id="05659-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="05659-121">Vous devez mettre à jour le modèle pour inclure votre nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="05659-121">You must update the model to include your new column.</span></span> <span data-ttu-id="05659-122">Dans le **modèles** dossier, ouvrez le **ContosoModel.edmx** fichier pour afficher le diagramme de modèle.</span><span class="sxs-lookup"><span data-stu-id="05659-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="05659-123">Notez que le modèle étudiant ne contient pas la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="05659-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="05659-124">Avec le bouton droit n’importe où sur l’aire de conception, puis sélectionnez **modèle de mise à jour à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="05659-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![modèle de mise à jour](changing-the-database/_static/image3.png)

<span data-ttu-id="05659-126">Dans l’Assistant Mise à jour, sélectionnez le **Actualiser** onglet et la **étudiant** table.</span><span class="sxs-lookup"><span data-stu-id="05659-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Assistant Mise à jour](changing-the-database/_static/image4.png)

<span data-ttu-id="05659-128">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="05659-128">Click **Finish**.</span></span>

<span data-ttu-id="05659-129">Une fois le processus de mise à jour est terminé, le diagramme de base de données inclut la nouvelle **MiddleName** propriété.</span><span class="sxs-lookup"><span data-stu-id="05659-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="05659-130">Enregistrer le **ContosoModel.edmx** fichier.</span><span class="sxs-lookup"><span data-stu-id="05659-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="05659-131">Vous devez enregistrer ce fichier pour la nouvelle propriété d’être propagées à la **Student.cs** classe.</span><span class="sxs-lookup"><span data-stu-id="05659-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="05659-132">Vous avez maintenant mis à jour la base de données et le modèle.</span><span class="sxs-lookup"><span data-stu-id="05659-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="05659-133">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="05659-133">Build the solution.</span></span>

<span data-ttu-id="05659-134">Malheureusement, les vues toujours ne contiennent pas la nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="05659-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="05659-135">Pour mettre à jour les vues vous avez deux options : vous pouvez soit générer de nouveau les vues en ajoutant de nouveau la structure de la classe Student, ou vous pouvez ajouter manuellement la nouvelle propriété à vos vues existantes.</span><span class="sxs-lookup"><span data-stu-id="05659-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="05659-136">Dans ce didacticiel, vous allez ajouter la génération de modèles automatique à nouveau, car vous n’avez pas apporté des modifications personnalisées aux vues générés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="05659-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="05659-137">Vous pouvez envisager l’ajout de la propriété manuellement lorsque vous avez apporté des modifications aux vues et que vous ne souhaitez pas perdre les modifications.</span><span class="sxs-lookup"><span data-stu-id="05659-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="05659-138">Pour garantir que les vues sont recréées, supprimez le **étudiants** dossier sous **vues**et supprimer les **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="05659-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="05659-139">Ensuite, avec le bouton droit le **contrôleurs** dossier et ajouter la génération de modèles automatique pour le **étudiant** modèle.</span><span class="sxs-lookup"><span data-stu-id="05659-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="05659-140">Là encore, nommez le contrôleur **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="05659-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="05659-141">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="05659-141">Select **OK**.</span></span>

<span data-ttu-id="05659-142">Les vues contiennent désormais la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="05659-142">The views now contain the MiddleName property.</span></span>

![afficher le nom de jeune fille](changing-the-database/_static/image5.png)

<span data-ttu-id="05659-144">Dans la section suivante, vous ajouterez du code pour personnaliser l’affichage pour afficher les détails d’un enregistrement de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="05659-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="05659-145">[Précédent](generating-views.md)
> [Suivant](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="05659-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
