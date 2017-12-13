---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "Base de données EF tout d’abord avec ASP.NET MVC : création de l’Application Web et les modèles de données | Documents Microsoft"
author: tfitzmac
description: "À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: f495bfa3aa5332e4ca3e44c2ffbfb760fbbeafc8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="b433a-104">Base de données EF tout d’abord avec ASP.NET MVC : création de l’Application Web et les modèles de données</span><span class="sxs-lookup"><span data-stu-id="b433a-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="b433a-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b433a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b433a-106">À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="b433a-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b433a-107">Cette série de didacticiels vous montre comment automatiquement générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer des données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="b433a-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b433a-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="b433a-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="b433a-109">Cette partie de la série se concentre sur la création de l’application web et générer les modèles de données en fonction de vos tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="b433a-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="b433a-110">Créez une Application Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b433a-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="b433a-111">Dans une solution ou dans la même solution que le projet de base de données, créez un nouveau projet dans Visual Studio et sélectionnez le **Application Web ASP.NET** modèle.</span><span class="sxs-lookup"><span data-stu-id="b433a-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="b433a-112">Nommez le projet **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="b433a-112">Name the project **ContosoSite**.</span></span>

![créer le projet](creating-the-web-application/_static/image1.png)

<span data-ttu-id="b433a-114">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b433a-114">Click **OK**.</span></span>

<span data-ttu-id="b433a-115">Dans la fenêtre Nouveau projet ASP.NET, sélectionnez le **MVC** modèle.</span><span class="sxs-lookup"><span data-stu-id="b433a-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="b433a-116">Vous pouvez effacer le **hôte dans le cloud** option pour l’instant, car vous allez déployer l’application vers le cloud plus tard.</span><span class="sxs-lookup"><span data-stu-id="b433a-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="b433a-117">Cliquez sur **OK** pour créer l’application.</span><span class="sxs-lookup"><span data-stu-id="b433a-117">Click **OK** to create the application.</span></span>

![Sélectionnez le modèle mvc](creating-the-web-application/_static/image2.png)

<span data-ttu-id="b433a-119">Le projet est créé avec les fichiers par défaut et les dossiers.</span><span class="sxs-lookup"><span data-stu-id="b433a-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="b433a-120">Dans ce didacticiel, vous allez utiliser Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b433a-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="b433a-121">Vous pouvez vérifier la version d’Entity Framework dans votre projet via la fenêtre Gérer les Packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="b433a-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="b433a-122">Si nécessaire, mettez à jour votre version d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b433a-122">If necessary, update your version of Entity Framework.</span></span>

![afficher la version](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="b433a-124">Générer les modèles</span><span class="sxs-lookup"><span data-stu-id="b433a-124">Generate the models</span></span>

<span data-ttu-id="b433a-125">Vous allez maintenant créer les modèles Entity Framework à partir des tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="b433a-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="b433a-126">Ces modèles sont des classes que vous allez utiliser pour travailler avec les données.</span><span class="sxs-lookup"><span data-stu-id="b433a-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="b433a-127">Chaque modèle reflète une table dans la base de données et contient des propriétés qui correspondent aux colonnes dans la table.</span><span class="sxs-lookup"><span data-stu-id="b433a-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="b433a-128">Avec le bouton droit le **modèles** dossier, puis sélectionnez **ajouter** et **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="b433a-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Ajouter un nouvel élément](creating-the-web-application/_static/image4.png)

<span data-ttu-id="b433a-130">Dans la fenêtre Ajouter un nouvel élément, sélectionnez **données** dans le volet gauche et **ADO.NET Entity Data Model** parmi les options dans le volet central.</span><span class="sxs-lookup"><span data-stu-id="b433a-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="b433a-131">Nommez le nouveau fichier de modèle **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="b433a-131">Name the new model file **ContosoModel**.</span></span>

![créer le modèle](creating-the-web-application/_static/image5.png)

<span data-ttu-id="b433a-133">Cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b433a-133">Click **Add**.</span></span>

<span data-ttu-id="b433a-134">Dans l’Assistant Entity Data Model, sélectionnez **EF Designer à partir de la base de données**.</span><span class="sxs-lookup"><span data-stu-id="b433a-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![générer à partir de la base de données](creating-the-web-application/_static/image6.png)

<span data-ttu-id="b433a-136">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="b433a-136">Click **Next**.</span></span>

<span data-ttu-id="b433a-137">Si vous avez des connexions de base de données définies dans votre environnement de développement, vous pouvez voir une de ces connexions présélectionnées.</span><span class="sxs-lookup"><span data-stu-id="b433a-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="b433a-138">Toutefois, vous souhaitez créer une nouvelle connexion à la base de données que vous avez créé dans la première partie de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="b433a-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="b433a-139">Cliquez sur le **nouvelle connexion** bouton.</span><span class="sxs-lookup"><span data-stu-id="b433a-139">Click the **New Connection** button.</span></span>

![se connecter à la base de données](creating-the-web-application/_static/image7.png)

<span data-ttu-id="b433a-141">Dans la fenêtre Propriétés de connexion, indiquez le nom du serveur local où votre base de données a été créé (dans ce cas **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="b433a-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="b433a-142">Après avoir entré le nom du serveur, sélectionnez le ContosoUniversityData à partir de bases de données disponibles.</span><span class="sxs-lookup"><span data-stu-id="b433a-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![définir les propriétés de connexion](creating-the-web-application/_static/image8.png)

<span data-ttu-id="b433a-144">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="b433a-144">Click **OK**.</span></span>

<span data-ttu-id="b433a-145">Les propriétés de connexion correctes sont maintenant affichées.</span><span class="sxs-lookup"><span data-stu-id="b433a-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="b433a-146">Vous pouvez utiliser le nom par défaut pour la connexion dans le fichier Web.Config</span><span class="sxs-lookup"><span data-stu-id="b433a-146">You can use the default name for connection in the Web.Config file</span></span>

![paramètres de connexion](creating-the-web-application/_static/image9.png)

<span data-ttu-id="b433a-148">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="b433a-148">Click **Next**.</span></span>

<span data-ttu-id="b433a-149">Sélectionnez **Tables** pour générer des modèles pour les trois tables.</span><span class="sxs-lookup"><span data-stu-id="b433a-149">Select **Tables** to generate models for all three tables.</span></span>

![Sélectionner des tables](creating-the-web-application/_static/image10.png)

<span data-ttu-id="b433a-151">Cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="b433a-151">Click **Finish**.</span></span>

<span data-ttu-id="b433a-152">Si vous recevez un avertissement de sécurité, sélectionnez **OK** pour continuer l’exécution du modèle.</span><span class="sxs-lookup"><span data-stu-id="b433a-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="b433a-153">Les modèles sont générés à partir de la base de données et un diagramme s’affiche et présente les propriétés et les relations entre les tables.</span><span class="sxs-lookup"><span data-stu-id="b433a-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagramme du modèle](creating-the-web-application/_static/image11.png)

<span data-ttu-id="b433a-155">Le dossier de modèles inclut désormais les nombreux nouveaux fichiers liés aux modèles qui ont été générées à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b433a-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![afficher les nouveaux fichiers de modèle](creating-the-web-application/_static/image12.png)

<span data-ttu-id="b433a-157">Le **ContosoModel.Context.cs** fichier contient une classe qui dérive de la **DbContext** classe et fournit une propriété pour chaque classe de modèle qui correspond à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="b433a-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="b433a-158">Le **Course.cs**, **Enrollment.cs**, et **Student.cs** fichiers contiennent les classes qui représentent les tables de bases de données du modèle.</span><span class="sxs-lookup"><span data-stu-id="b433a-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="b433a-159">Lorsque vous travaillez avec la structure, vous allez utiliser la classe de contexte et les classes du modèle.</span><span class="sxs-lookup"><span data-stu-id="b433a-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="b433a-160">Avant de poursuivre ce didacticiel, générez le projet.</span><span class="sxs-lookup"><span data-stu-id="b433a-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="b433a-161">Dans la section suivante, vous allez générer du code basé sur les modèles de données, mais cette section ne fonctionne pas si le projet n’a pas été généré.</span><span class="sxs-lookup"><span data-stu-id="b433a-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b433a-162">[Précédent](setting-up-database.md)
[Suivant](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="b433a-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
