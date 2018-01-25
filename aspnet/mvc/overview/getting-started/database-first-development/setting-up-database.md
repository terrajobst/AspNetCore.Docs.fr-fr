---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "Mise en route avec Entity Framework 6 Database First à l’aide de MVC 5 | Documents Microsoft"
author: tfitzmac
description: "À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: cb979333131cc6ac87fd640bf7c96931054a1814
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="eff05-104">Mise en route avec Entity Framework 6 Database First à l’aide de MVC 5</span><span class="sxs-lookup"><span data-stu-id="eff05-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="eff05-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="eff05-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="eff05-106">À l’aide de la structure d’ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante.</span><span class="sxs-lookup"><span data-stu-id="eff05-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="eff05-107">Cette série de didacticiels vous montre comment automatiquement générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer des données qui résident dans une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="eff05-108">Le code généré correspond aux colonnes dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="eff05-109">Dans la dernière partie de la série, vous allez déployer le site et la base de données vers Azure.</span><span class="sxs-lookup"><span data-stu-id="eff05-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="eff05-110">Cette partie de la série se concentre sur la création d’une base de données et le remplir avec les données.</span><span class="sxs-lookup"><span data-stu-id="eff05-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="eff05-111">Cette série a été écrit avec les contributions de Tom Dykstra et Rick Anderson.</span><span class="sxs-lookup"><span data-stu-id="eff05-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="eff05-112">Il a été améliorée en fonction des commentaires des utilisateurs dans la section commentaires.</span><span class="sxs-lookup"><span data-stu-id="eff05-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="eff05-113">Introduction</span><span class="sxs-lookup"><span data-stu-id="eff05-113">Introduction</span></span>

<span data-ttu-id="eff05-114">Cette rubrique montre comment démarrer avec un existant de base de données et de créer rapidement une application web qui permet aux utilisateurs d’interagir avec les données.</span><span class="sxs-lookup"><span data-stu-id="eff05-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="eff05-115">Elle utilise l’Entity Framework 6 et MVC 5 pour générer l’application web.</span><span class="sxs-lookup"><span data-stu-id="eff05-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="eff05-116">La fonctionnalité de génération de modèles automatique ASP.NET vous permet de générer automatiquement le code pour l’affichage, la mise à jour, la création et la suppression de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="eff05-117">À l’aide des outils de publication dans Visual Studio, vous pouvez facilement déployer le site et la base de données vers Azure.</span><span class="sxs-lookup"><span data-stu-id="eff05-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="eff05-118">Cette rubrique décrit la situation où vous avez une base de données et que vous voulez générer du code pour une application web basée sur les champs de cette base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="eff05-119">Cette approche est appelée développement première base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-119">This approach is called Database First development.</span></span> <span data-ttu-id="eff05-120">Si vous n’avez pas déjà d’une base de données existante, vous pouvez utiliser une approche de développement Code First qui implique la définition des classes de données et la génération de la base de données à partir des propriétés de classe.</span><span class="sxs-lookup"><span data-stu-id="eff05-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="eff05-121">Pour obtenir un exemple d’introduction du développement Code First, consultez [mise en route avec ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="eff05-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="eff05-122">Pour obtenir un exemple plus avancé, consultez [création d’un modèle de données Entity Framework pour une application ASP.NET MVC est 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="eff05-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="eff05-123">Pour obtenir des conseils sur la sélection de l’approche Entity Framework à utiliser, consultez [approches de développement Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="eff05-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eff05-124">Prérequis</span><span class="sxs-lookup"><span data-stu-id="eff05-124">Prerequisites</span></span>

<span data-ttu-id="eff05-125">Visual Studio 2013 ou Visual Studio Express 2013 pour le Web</span><span class="sxs-lookup"><span data-stu-id="eff05-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="eff05-126">Configurer la base de données</span><span class="sxs-lookup"><span data-stu-id="eff05-126">Set up the database</span></span>

<span data-ttu-id="eff05-127">Pour simuler l’environnement de disposer d’une base de données existante, vous tout d’abord créer une base de données avec des données automatiquement et puis créer votre application web qui se connecte à la base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="eff05-128">Ce didacticiel a été développé à l’aide de base de données locale avec Visual Studio 2013 ou Visual Studio Express 2013 pour le Web.</span><span class="sxs-lookup"><span data-stu-id="eff05-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="eff05-129">Vous pouvez utiliser un serveur de base de données existant au lieu de la base de données locale, mais selon votre version de Visual Studio et le type de base de données, tous les outils de données dans Visual Studio ne peuvent pas être pris en charge.</span><span class="sxs-lookup"><span data-stu-id="eff05-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="eff05-130">Si les outils ne sont pas disponibles pour votre base de données, vous devrez peut-être effectuer certaines étapes spécifiques à la base de données de la suite de gestion pour votre base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="eff05-131">Si vous avez un problème avec les outils de base de données dans votre version de Visual Studio, assurez-vous que vous avez installé la version la plus récente des outils de base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="eff05-132">Pour plus d’informations sur la mise à jour ou installation des outils de base de données, consultez [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="eff05-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="eff05-133">Lancez Visual Studio et créez un **projet de base de données SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="eff05-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="eff05-134">Nommez le projet **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="eff05-134">Name the project **ContosoUniversityData**.</span></span>

![créer le projet de base de données](setting-up-database/_static/image1.png)

<span data-ttu-id="eff05-136">Vous avez maintenant un projet de base de données vide.</span><span class="sxs-lookup"><span data-stu-id="eff05-136">You now have an empty database project.</span></span> <span data-ttu-id="eff05-137">Vous allez déployer cette base de données vers Azure plus loin dans ce didacticiel, vous devez définir des base de données SQL Azure comme plateforme cible pour le projet.</span><span class="sxs-lookup"><span data-stu-id="eff05-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="eff05-138">Définition de la plateforme cible ne déploie pas réellement de la base de données ; Cela signifie uniquement que le projet de base de données va vérifier que la conception de base de données est compatible avec la plateforme cible.</span><span class="sxs-lookup"><span data-stu-id="eff05-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="eff05-139">Pour définir la plateforme cible, ouvrez le **propriétés** pour le projet et sélectionnez **Microsoft Azure SQL Database** pour la plateforme cible.</span><span class="sxs-lookup"><span data-stu-id="eff05-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![Définissez la plateforme cible](setting-up-database/_static/image2.png)

<span data-ttu-id="eff05-141">Vous pouvez créer les tables nécessaires pour ce didacticiel en ajoutant des scripts SQL qui définissent les tables.</span><span class="sxs-lookup"><span data-stu-id="eff05-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="eff05-142">Avec le bouton droit de votre projet et ajouter un nouvel élément.</span><span class="sxs-lookup"><span data-stu-id="eff05-142">Right-click your project and add a new item.</span></span>

![Ajouter un nouvel élément](setting-up-database/_static/image3.png)

<span data-ttu-id="eff05-144">Ajouter une nouvelle table nommée étudiant.</span><span class="sxs-lookup"><span data-stu-id="eff05-144">Add a new table named Student.</span></span>

![Ajouter une table d’étudiants](setting-up-database/_static/image4.png)

<span data-ttu-id="eff05-146">Dans le fichier de la table, remplacez la commande T-SQL avec le code suivant pour créer la table.</span><span class="sxs-lookup"><span data-stu-id="eff05-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="eff05-147">Notez que la fenêtre de conception se synchronise automatiquement avec le code.</span><span class="sxs-lookup"><span data-stu-id="eff05-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="eff05-148">Vous pouvez travailler avec le code ou le concepteur.</span><span class="sxs-lookup"><span data-stu-id="eff05-148">You can work with either the code or designer.</span></span>

![afficher le code et conception](setting-up-database/_static/image5.png)

<span data-ttu-id="eff05-150">Ajouter une autre table.</span><span class="sxs-lookup"><span data-stu-id="eff05-150">Add another table.</span></span> <span data-ttu-id="eff05-151">Cette fois-ci, nommez-le cours et utilisez la commande T-SQL suivante.</span><span class="sxs-lookup"><span data-stu-id="eff05-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="eff05-152">Et répétez le plus de temps pour créer une table nommée d’inscription.</span><span class="sxs-lookup"><span data-stu-id="eff05-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="eff05-153">Vous pouvez remplir votre base de données avec les données via un script à exécuter après le déployée de la base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="eff05-154">Ajouter un Script de post-déploiement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="eff05-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="eff05-155">Vous pouvez utiliser le nom par défaut.</span><span class="sxs-lookup"><span data-stu-id="eff05-155">You can use the default name.</span></span>

![Ajouter un script de post-déploiement](setting-up-database/_static/image6.png)

<span data-ttu-id="eff05-157">Ajoutez le code T-SQL suivant pour le script de post-déploiement.</span><span class="sxs-lookup"><span data-stu-id="eff05-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="eff05-158">Ce script ajoute simplement les données à la base de données lorsque aucun enregistrement correspondant n’est trouvé.</span><span class="sxs-lookup"><span data-stu-id="eff05-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="eff05-159">Il ne pas remplacer ou supprimer toutes les données que vous avez entré dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="eff05-160">Il est important de noter que le script de post-déploiement est exécuté chaque fois que vous déployez votre projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="eff05-161">Par conséquent, vous devez soigneusement vos besoins lors de l’écriture de ce script.</span><span class="sxs-lookup"><span data-stu-id="eff05-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="eff05-162">Dans certains cas, vous pouvez souhaiter recommencer à partir d’un jeu connu de données chaque fois que le projet est déployé.</span><span class="sxs-lookup"><span data-stu-id="eff05-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="eff05-163">Dans d’autres cas, vous voudrez pas modifier les données existantes en aucune façon.</span><span class="sxs-lookup"><span data-stu-id="eff05-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="eff05-164">Selon vos besoins, vous pouvez décider si vous avez besoin d’un script de post-déploiement ou ce que vous devez inclure dans le script.</span><span class="sxs-lookup"><span data-stu-id="eff05-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="eff05-165">Pour plus d’informations sur le renseignement de votre base de données avec un script de post-déploiement, consultez [, y compris les données dans un projet de base de données SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="eff05-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="eff05-166">Vous avez maintenant 4 fichiers de script SQL, mais aucune table réelle.</span><span class="sxs-lookup"><span data-stu-id="eff05-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="eff05-167">Vous êtes prêt à déployer votre projet de base de données à la base de données locale.</span><span class="sxs-lookup"><span data-stu-id="eff05-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="eff05-168">Dans Visual Studio, cliquez sur le bouton Démarrer (ou F5) pour générer et déployer votre projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="eff05-169">Vérifiez l’onglet sortie pour vérifier que la génération et le déploiement a réussi.</span><span class="sxs-lookup"><span data-stu-id="eff05-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![afficher le résultat](setting-up-database/_static/image7.png)

<span data-ttu-id="eff05-171">Pour voir que la nouvelle base de données a été créée, ouvrez **l’Explorateur d’objets SQL Server** et recherchez le nom du projet dans le serveur local correct de la base de données (dans ce cas **(localdb) \ProjectsV12**).</span><span class="sxs-lookup"><span data-stu-id="eff05-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![afficher la nouvelle base de données](setting-up-database/_static/image8.png)

<span data-ttu-id="eff05-173">Pour voir que les tables sont remplies avec des données, cliquez sur une table, puis sélectionnez **des données d’affichage**.</span><span class="sxs-lookup"><span data-stu-id="eff05-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![afficher les données de table](setting-up-database/_static/image9.png)

<span data-ttu-id="eff05-175">Une vue modifiable des données de la table s’affiche.</span><span class="sxs-lookup"><span data-stu-id="eff05-175">An editable view of the table data is displayed.</span></span>

![afficher les résultats des données de table](setting-up-database/_static/image10.png)

<span data-ttu-id="eff05-177">Votre base de données est maintenant défini et rempli avec des données.</span><span class="sxs-lookup"><span data-stu-id="eff05-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="eff05-178">Dans l’étape suivante du didacticiel, vous allez créer une application web pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="eff05-178">In the next tutorial, you will create a web application for the database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="eff05-179">Next</span><span class="sxs-lookup"><span data-stu-id="eff05-179">Next</span></span>](creating-the-web-application.md)
