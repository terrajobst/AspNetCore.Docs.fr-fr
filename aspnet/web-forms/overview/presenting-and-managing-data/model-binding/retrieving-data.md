---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Récupération et affichage des données avec les formulaires web et de la liaison de modèle | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET. Liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396283"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="7a640-104">Récupération et affichage des données avec la liaison de modèle et les web forms</span><span class="sxs-lookup"><span data-stu-id="7a640-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="7a640-105">Cette série de didacticiels montre les aspects de base de l’utilisation de la liaison de modèle avec un projet Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7a640-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="7a640-106">Liaison de modèle rend interaction des données plus simple que vous traitez des données des objets de source (tels que ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="7a640-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="7a640-107">Cette série commence par une partie introductive et progresse vers des concepts plus avancés dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="7a640-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="7a640-108">Le modèle de liaison de modèle fonctionne avec toute technologie d’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="7a640-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="7a640-109">Dans ce didacticiel, vous allez utiliser Entity Framework, mais vous pouvez utiliser la technologie d’accès aux données qui connaît plus.</span><span class="sxs-lookup"><span data-stu-id="7a640-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="7a640-110">À partir d’un contrôle serveur lié aux données, tel qu’un contrôle GridView, ListView, DetailsView ou FormView, vous spécifiez les noms des méthodes à utiliser pour la sélection, la mise à jour, la suppression et la création de données.</span><span class="sxs-lookup"><span data-stu-id="7a640-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="7a640-111">Dans ce didacticiel, vous spécifierez une valeur pour la méthode SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="7a640-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="7a640-112">Dans cette méthode, vous fournissez la logique de récupération des données.</span><span class="sxs-lookup"><span data-stu-id="7a640-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="7a640-113">Dans le didacticiel suivant, vous allez définir des valeurs pour UpdateMethod, DeleteMethod et InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="7a640-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="7a640-114">Vous pouvez [télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="7a640-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="7a640-115">Le code téléchargeable fonctionne avec Visual Studio 2012 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="7a640-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="7a640-116">Elle utilise le modèle Visual Studio 2012, qui est légèrement différent de celle du modèle de Visual Studio 2017 présentée dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="7a640-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="7a640-117">Dans le didacticiel, vous exécutez l’application dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a640-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="7a640-118">Vous pouvez également déployer l’application sur un fournisseur d’hébergement et rendez-le accessible sur internet.</span><span class="sxs-lookup"><span data-stu-id="7a640-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="7a640-119">Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un</span><span class="sxs-lookup"><span data-stu-id="7a640-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="7a640-120">[compte d’essai Azure gratuit](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="7a640-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="7a640-121">Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur Azure App Service Web Apps, consultez le [le déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) série.</span><span class="sxs-lookup"><span data-stu-id="7a640-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="7a640-122">Ce didacticiel montre également comment utiliser des Migrations Entity Framework Code First pour déployer votre base de données SQL Server vers Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7a640-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7a640-123">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="7a640-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="7a640-124">Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="7a640-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="7a640-125">Ce didacticiel fonctionne également avec Visual Studio 2012 et Visual Studio 2013, mais il existe des différences dans le modèle de projet et d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7a640-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="7a640-126">Vous allez générer</span><span class="sxs-lookup"><span data-stu-id="7a640-126">What you'll build</span></span>

<span data-ttu-id="7a640-127">Dans ce didacticiel, vous allez :</span><span class="sxs-lookup"><span data-stu-id="7a640-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="7a640-128">Générer des objets de données qui reflètent une université avec étudiants inscrits aux cours</span><span class="sxs-lookup"><span data-stu-id="7a640-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="7a640-129">Créer des tables de base de données à partir des objets</span><span class="sxs-lookup"><span data-stu-id="7a640-129">Build database tables from the objects</span></span>
* <span data-ttu-id="7a640-130">Remplir la base de données de test</span><span class="sxs-lookup"><span data-stu-id="7a640-130">Populate the database with test data</span></span>
* <span data-ttu-id="7a640-131">Afficher des données dans un formulaire web</span><span class="sxs-lookup"><span data-stu-id="7a640-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="7a640-132">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="7a640-132">Create the project</span></span>

1. <span data-ttu-id="7a640-133">Dans Visual Studio 2017, créez un **Application Web ASP.NET (.NET Framework)** projet appelé **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="7a640-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![créer le projet](retrieving-data/_static/image19.png)

2. <span data-ttu-id="7a640-135">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="7a640-135">Select **OK**.</span></span> <span data-ttu-id="7a640-136">La boîte de dialogue Sélectionner un modèle s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7a640-136">The dialog box to select a template appears.</span></span>

   ![Sélectionnez les formulaires web](retrieving-data/_static/image3.png)

3. <span data-ttu-id="7a640-138">Sélectionnez le **Web Forms** modèle.</span><span class="sxs-lookup"><span data-stu-id="7a640-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="7a640-139">Si nécessaire, définissez l’authentification sur **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="7a640-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="7a640-140">Sélectionnez **OK** pour créer le projet.</span><span class="sxs-lookup"><span data-stu-id="7a640-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="7a640-141">Modifier l’apparence du site</span><span class="sxs-lookup"><span data-stu-id="7a640-141">Modify site appearance</span></span>

   <span data-ttu-id="7a640-142">Apporter quelques modifications pour personnaliser l’apparence du site.</span><span class="sxs-lookup"><span data-stu-id="7a640-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="7a640-143">Ouvrez le fichier Site.Master.</span><span class="sxs-lookup"><span data-stu-id="7a640-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="7a640-144">Modifier le titre à afficher **Contoso University** et non **mon Application ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="7a640-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="7a640-145">Modifier le texte d’en-tête à partir de **nom de l’Application** à **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="7a640-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="7a640-146">Modifier les liens d’en-tête de navigation pour les adapter de site.</span><span class="sxs-lookup"><span data-stu-id="7a640-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="7a640-147">Supprimer les liens pour **sur** et **Contact** et, au lieu de cela, lier à un **étudiants** page, que vous allez créer.</span><span class="sxs-lookup"><span data-stu-id="7a640-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="7a640-148">Enregistrez Site.Master.</span><span class="sxs-lookup"><span data-stu-id="7a640-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="7a640-149">Ajoutez un formulaire web pour afficher les données des étudiants</span><span class="sxs-lookup"><span data-stu-id="7a640-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="7a640-150">Dans **l’Explorateur de solutions**, cliquez sur votre projet, sélectionnez **ajouter** , puis **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="7a640-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="7a640-151">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **Web Form avec Page maître** modèle et nommez-le **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="7a640-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![créer la page](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="7a640-153">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7a640-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="7a640-154">Pour la page maître du formulaire web, sélectionnez **Site.Master**.</span><span class="sxs-lookup"><span data-stu-id="7a640-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="7a640-155">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="7a640-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="7a640-156">Ajouter le modèle de données</span><span class="sxs-lookup"><span data-stu-id="7a640-156">Add the data model</span></span>

<span data-ttu-id="7a640-157">Dans le **modèles** dossier, ajoutez une classe nommée **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="7a640-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="7a640-158">Avec le bouton droit **modèles**, sélectionnez **ajouter**, puis **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="7a640-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="7a640-159">La boîte de dialogue **Ajouter un nouvel élément** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7a640-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="7a640-160">Dans le menu de navigation de gauche, sélectionnez **Code**, puis **classe**.</span><span class="sxs-lookup"><span data-stu-id="7a640-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![créer la classe de modèle](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="7a640-162">Nommez la classe **UniversityModels.cs** et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="7a640-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="7a640-163">Dans ce fichier, définissez la `SchoolContext`, `Student`, `Enrollment`, et `Course` classes comme suit :</span><span class="sxs-lookup"><span data-stu-id="7a640-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="7a640-164">Le `SchoolContext` dérive de la classe `DbContext`, qui gère la connexion de base de données et les modifications dans les données.</span><span class="sxs-lookup"><span data-stu-id="7a640-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="7a640-165">Dans le `Student` class, notez les attributs appliqués à la `FirstName`, `LastName`, et `Year` propriétés.</span><span class="sxs-lookup"><span data-stu-id="7a640-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="7a640-166">Ce didacticiel utilise ces attributs pour la validation de données.</span><span class="sxs-lookup"><span data-stu-id="7a640-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="7a640-167">Pour simplifier le code, que ces propriétés sont marquées avec des attributs de validation des données.</span><span class="sxs-lookup"><span data-stu-id="7a640-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="7a640-168">Dans un projet réel, vous devez appliquer des attributs de validation à toutes les propriétés ayant besoin de validation.</span><span class="sxs-lookup"><span data-stu-id="7a640-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="7a640-169">Enregistrer UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="7a640-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="7a640-170">Configurer la base de données basée sur des classes</span><span class="sxs-lookup"><span data-stu-id="7a640-170">Set up the database based on classes</span></span>

<span data-ttu-id="7a640-171">Ce didacticiel utilise [Migrations Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) pour créer des objets et tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="7a640-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="7a640-172">Ces tables stockent des informations sur les étudiants et leurs cours.</span><span class="sxs-lookup"><span data-stu-id="7a640-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="7a640-173">Sélectionnez **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="7a640-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="7a640-174">Dans **Console du Gestionnaire de Package**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="7a640-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="7a640-175">Si la commande se termine correctement, un message indiquant que les migrations qui ont été activées s’affiche.</span><span class="sxs-lookup"><span data-stu-id="7a640-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Activer les migrations](retrieving-data/_static/image8.png)

      <span data-ttu-id="7a640-177">Notez qu’un fichier nommé *Configuration.cs* a été créé.</span><span class="sxs-lookup"><span data-stu-id="7a640-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="7a640-178">Le `Configuration` classe a un `Seed` (méthode), qui permettre préremplir les tables de base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="7a640-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="7a640-179">Préremplir la base de données</span><span class="sxs-lookup"><span data-stu-id="7a640-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="7a640-180">Ouvrez Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="7a640-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="7a640-181">Ajoutez le code suivant à la méthode `Seed` .</span><span class="sxs-lookup"><span data-stu-id="7a640-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="7a640-182">En outre, ajouter un `using` instruction pour la `ContosoUniversityModelBinding. Models` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="7a640-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="7a640-183">Enregistrer Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="7a640-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="7a640-184">Dans la Console du Gestionnaire de Package, exécutez la commande **ajouter-migration initial**.</span><span class="sxs-lookup"><span data-stu-id="7a640-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="7a640-185">Exécutez la commande **mise à jour la base de données**.</span><span class="sxs-lookup"><span data-stu-id="7a640-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="7a640-186">Si vous recevez une exception lors de l’exécution de cette commande, le `StudentID` et `CourseID` valeurs peuvent être différentes de la `Seed` valeurs de la méthode.</span><span class="sxs-lookup"><span data-stu-id="7a640-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="7a640-187">Ouvrez ces tables de base de données et rechercher les valeurs existantes pour `StudentID` et `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="7a640-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="7a640-188">Ajoutez ces valeurs pour le code pour l’amorçage le `Enrollments` table.</span><span class="sxs-lookup"><span data-stu-id="7a640-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="7a640-189">Ajouter un contrôle GridView</span><span class="sxs-lookup"><span data-stu-id="7a640-189">Add a GridView control</span></span>

<span data-ttu-id="7a640-190">Avec les données de base de données remplis, vous êtes maintenant prêt à récupérer ces données et les afficher.</span><span class="sxs-lookup"><span data-stu-id="7a640-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="7a640-191">Ouvrez Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="7a640-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="7a640-192">Recherchez le `MainContent` espace réservé.</span><span class="sxs-lookup"><span data-stu-id="7a640-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="7a640-193">Dans cet espace réservé, ajoutez un **GridView** contrôle qui contient ce code.</span><span class="sxs-lookup"><span data-stu-id="7a640-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="7a640-194">Points à noter :</span><span class="sxs-lookup"><span data-stu-id="7a640-194">Things to note:</span></span>
   * <span data-ttu-id="7a640-195">Notez que la valeur définie pour le `SelectMethod` propriété dans l’élément de GridView.</span><span class="sxs-lookup"><span data-stu-id="7a640-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="7a640-196">Cette valeur spécifie la méthode utilisée pour récupérer les données de GridView, que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="7a640-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="7a640-197">Le `ItemType` propriété est définie sur la `Student` classe créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="7a640-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="7a640-198">Ce paramètre vous permet de référencer les propriétés de la classe dans le balisage.</span><span class="sxs-lookup"><span data-stu-id="7a640-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="7a640-199">Par exemple, le `Student` classe a une collection nommée `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="7a640-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="7a640-200">Vous pouvez utiliser `Item.Enrollments` pour récupérer cette collection, puis utilisez [syntaxe LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) pour récupérer chaque étudiant d’inscrits somme de crédits.</span><span class="sxs-lookup"><span data-stu-id="7a640-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="7a640-201">Enregistrer Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="7a640-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="7a640-202">Ajoutez du code pour récupérer des données</span><span class="sxs-lookup"><span data-stu-id="7a640-202">Add code to retrieve data</span></span>

   <span data-ttu-id="7a640-203">Dans le fichier de code-behind Students.aspx, ajoutez la méthode spécifiée pour le `SelectMethod` valeur.</span><span class="sxs-lookup"><span data-stu-id="7a640-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="7a640-204">Ouvrez Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="7a640-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="7a640-205">Ajouter `using` instructions pour le `ContosoUniversityModelBinding. Models` et `System.Data.Entity` espaces de noms.</span><span class="sxs-lookup"><span data-stu-id="7a640-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="7a640-206">Ajoutez la méthode que vous avez spécifié pour `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="7a640-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="7a640-207">Le `Include` clause améliore les performances des requêtes mais n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="7a640-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="7a640-208">Sans le `Include` clause, les données est récupérée à l’aide de [ *le chargement différé*](https://en.wikipedia.org/wiki/Lazy_loading), ce qui implique l’envoi d’une requête distincte pour la base de données chaque fois liés sont extraites.</span><span class="sxs-lookup"><span data-stu-id="7a640-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="7a640-209">Avec le `Include` clause, les données est récupérée à l’aide de *chargement hâtif*, ce qui signifie qu’une requête de base de données unique récupère toutes les données liées.</span><span class="sxs-lookup"><span data-stu-id="7a640-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="7a640-210">Si les données associées n’est pas utilisées, le chargement hâtif est moins efficace, car davantage de données est récupérée.</span><span class="sxs-lookup"><span data-stu-id="7a640-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="7a640-211">Toutefois, dans ce cas, le chargement hâtif vous donne les meilleures performances car les données associées sont affichées pour chaque enregistrement.</span><span class="sxs-lookup"><span data-stu-id="7a640-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="7a640-212">Pour plus d’informations sur les performances lors du chargement des données associées, consultez le **Lazy, Eager et explicite du chargement de données associées** section dans le [lecture des données associées avec Entity Framework dans ASP.NET Application MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span><span class="sxs-lookup"><span data-stu-id="7a640-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="7a640-213">Par défaut, les données sont triées par les valeurs de la propriété marquée comme clé.</span><span class="sxs-lookup"><span data-stu-id="7a640-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="7a640-214">Vous pouvez ajouter un `OrderBy` clause pour spécifier une valeur de tri différent.</span><span class="sxs-lookup"><span data-stu-id="7a640-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="7a640-215">Dans cet exemple, la valeur par défaut `StudentID` propriété est utilisée pour le tri.</span><span class="sxs-lookup"><span data-stu-id="7a640-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="7a640-216">Dans le [tri, pagination et filtrage des données](sorting-paging-and-filtering-data.md) article, l’utilisateur est activé pour sélectionner une colonne de tri.</span><span class="sxs-lookup"><span data-stu-id="7a640-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="7a640-217">Enregistrer Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="7a640-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="7a640-218">Exécutez votre application</span><span class="sxs-lookup"><span data-stu-id="7a640-218">Run your application</span></span> 

<span data-ttu-id="7a640-219">Exécuter votre application web (**F5**) et accédez à la **étudiants** page, qui affiche les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="7a640-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![afficher les données](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="7a640-221">Génération automatique des méthodes de liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="7a640-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="7a640-222">Lorsque vous travaillez via cette série de didacticiels, vous pouvez simplement copier le code à votre projet à partir du didacticiel.</span><span class="sxs-lookup"><span data-stu-id="7a640-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="7a640-223">Toutefois, l’un des inconvénients de cette approche sont que vous ne pouvez pas avoir connaissance de la fonctionnalité fournie par Visual Studio pour générer automatiquement le code pour les méthodes de liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="7a640-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="7a640-224">Lorsque vous travaillez sur vos propres projets, génération de code automatique peut gagner du temps et aide que vous avez une idée de la façon d’implémenter une opération.</span><span class="sxs-lookup"><span data-stu-id="7a640-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="7a640-225">Cette section décrit la fonctionnalité de génération de code automatique.</span><span class="sxs-lookup"><span data-stu-id="7a640-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="7a640-226">Cette section est uniquement informatif et ne contient-elle pas de code, que vous devez implémenter dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="7a640-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="7a640-227">Lors de la définition d’une valeur pour le `SelectMethod`, `UpdateMethod`, `InsertMethod`, ou `DeleteMethod` propriétés dans le code de balisage, vous pouvez sélectionner le **créer une nouvelle méthode** option.</span><span class="sxs-lookup"><span data-stu-id="7a640-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![Créez une méthode](retrieving-data/_static/image18.png)

<span data-ttu-id="7a640-229">Visual Studio crée une méthode dans le code-behind avec la signature appropriée mais génère également du code d’implémentation pour effectuer l’opération.</span><span class="sxs-lookup"><span data-stu-id="7a640-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="7a640-230">Si vous définissez tout d’abord le `ItemType` propriété avant d’utiliser la génération de code automatique des fonctionnalités, les utilisations de code généré type pour les opérations.</span><span class="sxs-lookup"><span data-stu-id="7a640-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="7a640-231">Par exemple, lors de la définition du `UpdateMethod` propriété, le code suivant est générée automatiquement :</span><span class="sxs-lookup"><span data-stu-id="7a640-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="7a640-232">Là encore, ce code n’a pas besoin être ajouté à votre projet.</span><span class="sxs-lookup"><span data-stu-id="7a640-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="7a640-233">Dans le didacticiel suivant, vous allez implémenter des méthodes pour la mise à jour, la suppression et l’ajout de nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="7a640-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="7a640-234">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="7a640-234">Summary</span></span>

<span data-ttu-id="7a640-235">Dans ce didacticiel, vous créé des classes de modèle de données et généré une base de données à partir de ces classes.</span><span class="sxs-lookup"><span data-stu-id="7a640-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="7a640-236">Vous avez rempli les tables de base de données avec des données de test.</span><span class="sxs-lookup"><span data-stu-id="7a640-236">You filled the database tables with test data.</span></span> <span data-ttu-id="7a640-237">Votre liaison de modèle permet de récupérer des données à partir de la base de données, puis affiche les données dans un GridView.</span><span class="sxs-lookup"><span data-stu-id="7a640-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="7a640-238">Dans la prochaine [didacticiel](updating-deleting-and-creating-data.md) dans cette série, vous allez activer la mise à jour, la suppression et la création de données.</span><span class="sxs-lookup"><span data-stu-id="7a640-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7a640-239">Next</span><span class="sxs-lookup"><span data-stu-id="7a640-239">Next</span></span>](updating-deleting-and-creating-data.md)
