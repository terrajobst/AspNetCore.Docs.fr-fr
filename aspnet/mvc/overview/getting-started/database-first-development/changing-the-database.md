---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutoriel : Modifier la base de données pour EF Database First avec une application ASP.NET MVC'
description: Ce didacticiel se concentre sur effectuer une mise à jour à la structure de base de données et la propagation de cette modification tout au long de l’application web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667633"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="00b81-103">Tutoriel : Modifier la base de données pour EF Database First avec une application ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="00b81-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="00b81-104">À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="00b81-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="00b81-105">Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="00b81-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="00b81-106">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="00b81-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="00b81-107">Ce didacticiel se concentre sur effectuer une mise à jour à la structure de base de données et la propagation de cette modification tout au long de l’application web.</span><span class="sxs-lookup"><span data-stu-id="00b81-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="00b81-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="00b81-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="00b81-109">Ajouter une colonne</span><span class="sxs-lookup"><span data-stu-id="00b81-109">Add a column</span></span>
> * <span data-ttu-id="00b81-110">Ajoutez la propriété dans les vues</span><span class="sxs-lookup"><span data-stu-id="00b81-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00b81-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="00b81-111">Prerequisites</span></span>

* [<span data-ttu-id="00b81-112">Génération de vues</span><span class="sxs-lookup"><span data-stu-id="00b81-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="00b81-113">Ajouter une colonne</span><span class="sxs-lookup"><span data-stu-id="00b81-113">Add a column</span></span>

<span data-ttu-id="00b81-114">Si vous mettez à jour la structure d’une table dans votre base de données, vous devez vous assurer que votre modification est propagée vers le modèle de données, vues et contrôleur.</span><span class="sxs-lookup"><span data-stu-id="00b81-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="00b81-115">Pour ce didacticiel, vous allez ajouter une nouvelle colonne à la table Student pour enregistrer le deuxième prénom de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="00b81-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="00b81-116">Pour ajouter cette colonne, ouvrez le projet de base de données et ouvrez le fichier Student.sql.</span><span class="sxs-lookup"><span data-stu-id="00b81-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="00b81-117">Par le biais du concepteur ou dans le code T-SQL, ajoutez une colonne nommée **MiddleName** qui est un nvarchar (50) et autorise les valeurs NULL.</span><span class="sxs-lookup"><span data-stu-id="00b81-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="00b81-118">Déployer cette modification à votre base de données locale en démarrant votre projet de base de données (ou F5).</span><span class="sxs-lookup"><span data-stu-id="00b81-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="00b81-119">Le nouveau champ est ajouté à la table.</span><span class="sxs-lookup"><span data-stu-id="00b81-119">The new field is added to the table.</span></span> <span data-ttu-id="00b81-120">Si vous ne voyez pas dans l’Explorateur d’objets SQL Server, cliquez sur le bouton Actualiser dans le volet.</span><span class="sxs-lookup"><span data-stu-id="00b81-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![afficher la nouvelle colonne](changing-the-database/_static/image2.png)

<span data-ttu-id="00b81-122">La nouvelle colonne existe dans la table de base de données, mais il n’existe pas dans la classe de modèle de données.</span><span class="sxs-lookup"><span data-stu-id="00b81-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="00b81-123">Vous devez mettre à jour le modèle pour inclure votre nouvelle colonne.</span><span class="sxs-lookup"><span data-stu-id="00b81-123">You must update the model to include your new column.</span></span> <span data-ttu-id="00b81-124">Dans le **modèles** dossier, ouvrez le **ContosoModel.edmx** fichier pour afficher le diagramme de modèle.</span><span class="sxs-lookup"><span data-stu-id="00b81-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="00b81-125">Notez que le modèle Student ne contient pas la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="00b81-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="00b81-126">Avec le bouton droit n’importe où sur l’aire de conception, puis sélectionnez **modèle de mise à jour à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="00b81-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="00b81-127">Dans l’Assistant Mise à jour, sélectionnez le **Actualiser** onglet, puis sélectionnez **Tables** > **dbo** > **étudiant**.</span><span class="sxs-lookup"><span data-stu-id="00b81-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="00b81-128">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="00b81-128">Click **Finish**.</span></span>

<span data-ttu-id="00b81-129">Une fois le processus de mise à jour est terminé, le schéma de base de données inclut la nouvelle **MiddleName** propriété.</span><span class="sxs-lookup"><span data-stu-id="00b81-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="00b81-130">Enregistrer le **ContosoModel.edmx** fichier.</span><span class="sxs-lookup"><span data-stu-id="00b81-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="00b81-131">Vous devez enregistrer ce fichier pour la nouvelle propriété à propager la **Student.cs** classe.</span><span class="sxs-lookup"><span data-stu-id="00b81-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="00b81-132">Vous avez maintenant mis à jour la base de données et le modèle.</span><span class="sxs-lookup"><span data-stu-id="00b81-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="00b81-133">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="00b81-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="00b81-134">Ajoutez la propriété dans les vues</span><span class="sxs-lookup"><span data-stu-id="00b81-134">Add the property to the views</span></span>

<span data-ttu-id="00b81-135">Malheureusement, les vues toujours ne contiennent pas la nouvelle propriété.</span><span class="sxs-lookup"><span data-stu-id="00b81-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="00b81-136">Pour mettre à jour les vues vous avez deux options : vous pouvez soit générer de nouveau les vues en ajoutant une fois encore de génération de modèles automatique pour la classe Student, ou vous pouvez ajouter manuellement la nouvelle propriété à vos affichages existants.</span><span class="sxs-lookup"><span data-stu-id="00b81-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="00b81-137">Dans ce didacticiel, vous allez ajouter la génération de modèles automatique à nouveau, car vous n’avez pas apporté des modifications personnalisées aux affichages généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="00b81-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="00b81-138">Vous pouvez envisager d’ajouter manuellement la propriété lorsque vous avez apporté des modifications aux vues et que vous ne souhaitez pas perdre ces modifications.</span><span class="sxs-lookup"><span data-stu-id="00b81-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="00b81-139">Pour vous assurer les vues sont recréées, supprimez le **étudiants** dossier sous **vues**et supprimer le **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="00b81-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="00b81-140">Ensuite, cliquez sur le **contrôleurs** dossier et ajouter la génération de modèles automatique pour le **étudiant** modèle.</span><span class="sxs-lookup"><span data-stu-id="00b81-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="00b81-141">Là encore, nommez le contrôleur **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="00b81-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="00b81-142">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="00b81-142">Select **Add**.</span></span>

<span data-ttu-id="00b81-143">Générez à nouveau la solution.</span><span class="sxs-lookup"><span data-stu-id="00b81-143">Build the solution again.</span></span> <span data-ttu-id="00b81-144">Les vues contiennent désormais la propriété MiddleName.</span><span class="sxs-lookup"><span data-stu-id="00b81-144">The views now contain the MiddleName property.</span></span>

![afficher le deuxième prénom](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="00b81-146">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="00b81-146">Next steps</span></span>

<span data-ttu-id="00b81-147">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="00b81-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="00b81-148">Ajouter une colonne</span><span class="sxs-lookup"><span data-stu-id="00b81-148">Added a column</span></span>
> * <span data-ttu-id="00b81-149">La propriété ajoutée aux vues</span><span class="sxs-lookup"><span data-stu-id="00b81-149">Added the property to the views</span></span>

<span data-ttu-id="00b81-150">Passez au didacticiel suivant pour apprendre à personnaliser l’affichage de l’affichage des détails d’un enregistrement d’étudiant.</span><span class="sxs-lookup"><span data-stu-id="00b81-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="00b81-151">Personnaliser un affichage</span><span class="sxs-lookup"><span data-stu-id="00b81-151">Customize a view</span></span>](customizing-a-view.md)