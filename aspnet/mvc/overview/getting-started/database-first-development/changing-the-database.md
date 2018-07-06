---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First avec ASP.NET MVC : modification de la base de données | Microsoft Docs'
author: tfitzmac
description: À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 0176274694426a527ed0862b5138919d4cf5c319
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840938"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="a78b8-104">EF Database First avec ASP.NET MVC : modification de la base de données</span><span class="sxs-lookup"><span data-stu-id="a78b8-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="a78b8-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a78b8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a78b8-106">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="a78b8-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="a78b8-107">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a78b8-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="a78b8-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="a78b8-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="a78b8-109">Cette partie de la série se concentre sur effectuer une mise à jour à la structure de base de données et la propagation de cette modification tout au long de l’application web.</span><span class="sxs-lookup"><span data-stu-id="a78b8-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="a78b8-110">Ajouter une colonne</span><span class="sxs-lookup"><span data-stu-id="a78b8-110">Add a column</span></span>

<span data-ttu-id="a78b8-111">Si vous mettez à jour la structure d’une table dans votre base de données, vous devez vous assurer que votre modification est propagée vers le modèle de données, vues et contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a78b8-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="a78b8-112">Pour ce didacticiel, vous allez ajouter une nouvelle colonne à la table Student pour enregistrer le deuxième prénom de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="a78b8-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="a78b8-113">Pour ajouter cette colonne, ouvrez le projet de base de données et ouvrez le fichier Student.sql.</span><span class="sxs-lookup"><span data-stu-id="a78b8-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="a78b8-114">Par le biais du concepteur ou dans le code T-SQL, ajoutez une colonne nommée **MiddleName** qui est un nvarchar (50) et autorise les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="a78b8-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![ajouter le deuxième prénom](changing-the-database/_static/image1.png)

<span data-ttu-id="a78b8-116">Déployer cette modification à votre base de données locale en démarrant votre projet de base de données (ou F5).</span><span class="sxs-lookup"><span data-stu-id="a78b8-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="a78b8-117">Le nouveau champ est ajouté à la table.</span><span class="sxs-lookup"><span data-stu-id="a78b8-117">The new field is added to the table.</span></span> <span data-ttu-id="a78b8-118">Si vous ne voyez pas dans l’Explorateur d’objets SQL Server, cliquez sur le bouton Actualiser dans le volet.</span><span class="sxs-lookup"><span data-stu-id="a78b8-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![afficher la nouvelle colonne](changing-the-database/_static/image2.png)

<span data-ttu-id="a78b8-120">La nouvelle colonne existe dans la table de base de données, mais il n’existe pas dans la classe de modèle de données.</span><span class="sxs-lookup"><span data-stu-id="a78b8-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="a78b8-121">Vous devez mettre à jour le modèle pour inclure votre nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="a78b8-121">You must update the model to include your new column.</span></span> <span data-ttu-id="a78b8-122">Dans le **modèles** dossier, ouvrez le **ContosoModel.edmx** fichier pour afficher le diagramme de modèle.</span><span class="sxs-lookup"><span data-stu-id="a78b8-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="a78b8-123">Notez que le modèle Student ne contient pas la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="a78b8-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="a78b8-124">Avec le bouton droit n’importe où sur l’aire de conception, puis sélectionnez **modèle de mise à jour à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="a78b8-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![modèle de mise à jour](changing-the-database/_static/image3.png)

<span data-ttu-id="a78b8-126">Dans l’Assistant Mise à jour, sélectionnez le **Actualiser** onglet et le **étudiant** table.</span><span class="sxs-lookup"><span data-stu-id="a78b8-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Assistant Mise à jour](changing-the-database/_static/image4.png)

<span data-ttu-id="a78b8-128">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="a78b8-128">Click **Finish**.</span></span>

<span data-ttu-id="a78b8-129">Une fois le processus de mise à jour est terminé, le schéma de base de données inclut la nouvelle **MiddleName** propriété.</span><span class="sxs-lookup"><span data-stu-id="a78b8-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="a78b8-130">Enregistrer le **ContosoModel.edmx** fichier.</span><span class="sxs-lookup"><span data-stu-id="a78b8-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="a78b8-131">Vous devez enregistrer ce fichier pour la nouvelle propriété à propager la **Student.cs** classe.</span><span class="sxs-lookup"><span data-stu-id="a78b8-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="a78b8-132">Vous avez maintenant mis à jour la base de données et le modèle.</span><span class="sxs-lookup"><span data-stu-id="a78b8-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="a78b8-133">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="a78b8-133">Build the solution.</span></span>

<span data-ttu-id="a78b8-134">Malheureusement, les vues toujours ne contiennent pas la nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="a78b8-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="a78b8-135">Pour mettre à jour les vues vous avez deux options : vous pouvez soit générer de nouveau les vues en ajoutant une fois encore de génération de modèles automatique pour la classe Student, ou vous pouvez ajouter manuellement la nouvelle propriété à vos affichages existants.</span><span class="sxs-lookup"><span data-stu-id="a78b8-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="a78b8-136">Dans ce didacticiel, vous allez ajouter la génération de modèles automatique à nouveau, car vous n’avez pas apporté des modifications personnalisées aux affichages généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="a78b8-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="a78b8-137">Vous pouvez envisager d’ajouter manuellement la propriété lorsque vous avez apporté des modifications aux vues et que vous ne souhaitez pas perdre ces modifications.</span><span class="sxs-lookup"><span data-stu-id="a78b8-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="a78b8-138">Pour vous assurer les vues sont recréées, supprimez le **étudiants** dossier sous **vues**et supprimer le **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="a78b8-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="a78b8-139">Ensuite, cliquez sur le **contrôleurs** dossier et ajouter la génération de modèles automatique pour le **étudiant** modèle.</span><span class="sxs-lookup"><span data-stu-id="a78b8-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="a78b8-140">Là encore, nommez le contrôleur **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="a78b8-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="a78b8-141">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="a78b8-141">Select **OK**.</span></span>

<span data-ttu-id="a78b8-142">Les vues contiennent désormais la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="a78b8-142">The views now contain the MiddleName property.</span></span>

![afficher le deuxième prénom](changing-the-database/_static/image5.png)

<span data-ttu-id="a78b8-144">Dans la section suivante, vous ajouterez du code pour personnaliser l’affichage de l’affichage des détails d’un enregistrement d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="a78b8-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a78b8-145">[Précédent](generating-views.md)
> [Suivant](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="a78b8-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
