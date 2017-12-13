---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: "Accès aux données et les modèles ASP.NET MVC 4 | Documents Microsoft"
author: rick-anderson
description: "Remarque : Cet atelier pratique suppose que vous avez une connaissance élémentaire d’ASP.NET MVC. Si vous n’avez pas utilisé ASP.NET MVC avant, nous vous recommandons de vous pencher sur ASP.NET MVC 4..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: bf4bb5c6f9ad8339c3597b0d6666c7077ac476e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="dd1cb-104">Accès aux données et les modèles ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="dd1cb-104">ASP.NET MVC 4 Models and Data Access</span></span>
====================
<span data-ttu-id="dd1cb-105">par [Web Camps équipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-106">Cet atelier pratique suppose que vous avez une connaissance élémentaire des **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="dd1cb-107">Si vous n’avez pas utilisé **ASP.NET MVC** auparavant, nous vous recommandons de dépasser **notions de base ASP.NET MVC 4** atelier pratique.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-107">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="dd1cb-108">Ce laboratoire présente les améliorations et nouvelles fonctionnalités décrites précédemment en appliquant les modifications mineures à un exemple d’application Web dans le dossier Source.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-108">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="dd1cb-109">Tous les exemples de code et des extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-109">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="dd1cb-110">Dans **notions de base ASP.NET MVC** atelier pratique, vous avez été passant données codées en dur à partir des contrôleurs pour les modèles d’affichage.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-110">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="dd1cb-111">Toutefois, pour générer une application Web réelle, vous pouvez souhaiter utiliser une base de données réel.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-111">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="dd1cb-112">Cet atelier pratique vous montrerai d’utiliser un moteur de base de données afin de stocker et récupérer les données nécessaires pour l’application de magasin de musique.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-112">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="dd1cb-113">Pour ce faire, vous démarrez avec une base de données, créer l’Entity Data Model à partir de celui-ci.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-113">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="dd1cb-114">Tout au long de cet atelier, vous répondra le **Database First** approche, ainsi que les **Code First** approche.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-114">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="dd1cb-115">Toutefois, vous pouvez également utiliser le **Model First** approche, créer le même modèle à l’aide des outils, puis générer la base de données à partir de celui-ci.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-115">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="dd1cb-116">![Vs premier de la base de données. Modèle premier](aspnet-mvc-4-models-and-data-access/_static/image1.png "vs première base de données. Tout d’abord de modèle")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-116">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="dd1cb-117">*Vs premier de la base de données. Tout d’abord de modèle*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-117">*Database First vs. Model First*</span></span>

<span data-ttu-id="dd1cb-118">Après avoir généré le modèle, vous allez apporter les ajustements appropriés dans le StoreController pour fournir des affichages du magasin avec les données provenant de la base de données, au lieu d’utiliser des données codées en dur.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-118">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="dd1cb-119">Vous devrez pas modifiez les modèles d’affichage, car le StoreController retournera les ViewModel mêmes pour les modèles d’affichage, bien que cette fois les données proviennent la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-119">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="dd1cb-120">**La première approche de Code**</span><span class="sxs-lookup"><span data-stu-id="dd1cb-120">**The Code First Approach**</span></span>

<span data-ttu-id="dd1cb-121">L’approche Code First permet de définir le modèle à partir du code sans générer des classes qui sont généralement associés avec l’infrastructure.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-121">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="dd1cb-122">Dans le code en premier lieu, les objets de modèle sont définis avec POCOs, &quot;Plain Old CLR Objects&quot;.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-122">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="dd1cb-123">POCOs sont des classes de bruts simples qui n’ont aucun héritage et n’implémentent pas d’interfaces.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-123">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="dd1cb-124">Nous pouvons générer automatiquement la base de données à partir de celles-ci, ou nous pouvons utiliser une base de données et générer le mappage de classe à partir du code.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-124">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="dd1cb-125">Les avantages de l’utilisation de cette approche est que le modèle reste indépendant de l’infrastructure de persistance (dans ce cas, Entity Framework), comme les classes POCOs ne sont pas associées à l’infrastructure de mappage.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-125">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-126">Ce laboratoire repose sur ASP.NET MVC 4 et une version de l’application d’exemple de magasin de musique personnalisé et réduite pour s’ajuster à uniquement les fonctionnalités présentées dans cet atelier pratique.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-126">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="dd1cb-127">Si vous souhaitez Explorer l’ensemble **magasin de musique** application du didacticiel vous le trouverez dans [magasin de musique MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-127">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="dd1cb-128">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="dd1cb-128">Prerequisites</span></span>

<span data-ttu-id="dd1cb-129">Vous devez disposer des éléments suivants pour effectuer ce laboratoire :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="dd1cb-130">[Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lecture [annexe A](#AppendixA) pour obtenir des instructions sur l’installation).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="dd1cb-131">Installation</span><span class="sxs-lookup"><span data-stu-id="dd1cb-131">Setup</span></span>

<span data-ttu-id="dd1cb-132">**L’installation des extraits de Code**</span><span class="sxs-lookup"><span data-stu-id="dd1cb-132">**Installing Code Snippets**</span></span>

<span data-ttu-id="dd1cb-133">Pour plus de commodité, une grande partie du code que vous gérez le long de cet atelier est disponible sous les extraits de code Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-133">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="dd1cb-134">Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-134">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="dd1cb-135">Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et vous souhaitez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide de l’annexe c :](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-135">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="dd1cb-136">Exercices</span><span class="sxs-lookup"><span data-stu-id="dd1cb-136">Exercises</span></span>

<span data-ttu-id="dd1cb-137">Cet atelier pratique est constitué par les exercices suivants :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-137">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="dd1cb-138">Exercice 1 : Ajout d’une base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-138">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="dd1cb-139">Exercice 2 : Création d’une base de données à l’aide de Code First</span><span class="sxs-lookup"><span data-stu-id="dd1cb-139">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="dd1cb-140">Exercice 3 : Interrogation de la base de données avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="dd1cb-140">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="dd1cb-141">Chaque exercice est accompagné par un **fin** dossier qui contient la solution résultante, vous devez obtenir après avoir effectué les exercices.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-141">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="dd1cb-142">Si vous avez besoin d’aide fonctionne via les exercices, vous pouvez utiliser cette solution comme guide.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-142">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="dd1cb-143">Durée estimée pour effectuer ce laboratoire : **35 minutes**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-143">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="dd1cb-144">Exercice 1 : Ajout d’une base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-144">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="dd1cb-145">Dans cet exercice, vous allez apprendre à ajouter une base de données avec les tables de l’application MusicStore à la solution pour pouvoir utiliser ses données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-145">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="dd1cb-146">Une fois la base de données est généré avec le modèle et ajouté à la solution, vous allez modifier la classe StoreController pour fournir le modèle d’affichage avec les données provenant de la base de données, au lieu d’utiliser des valeurs codées en dur.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-146">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="dd1cb-147">Tâche 1 : ajout d’une base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-147">Task 1 - Adding a Database</span></span>

<span data-ttu-id="dd1cb-148">Dans cette tâche, vous allez ajouter une base de données déjà créée avec les tables principales de l’application MusicStore à la solution.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-148">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="dd1cb-149">Ouvrez le **commencer** solution situé dans **début/AddingADatabaseDBFirst-Ex1/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-149">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

    1. <span data-ttu-id="dd1cb-150">Vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-150">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dd1cb-151">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-151">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="dd1cb-152">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-152">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="dd1cb-153">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-153">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-154">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-154">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dd1cb-155">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-155">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dd1cb-156">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-156">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dd1cb-157">Ajouter **MvcMusicStore** fichier de base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-157">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="dd1cb-158">Dans cet atelier pratique, vous allez utiliser une base de données déjà créée appelé **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-158">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="dd1cb-159">Pour ce faire, cliquez sur **application\_données** dossier, pointez sur **ajouter** puis cliquez sur **élément existant**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-159">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="dd1cb-160">Accédez à **\Source\Assets** et sélectionnez le **MvcMusicStore.mdf** fichier.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-160">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="dd1cb-161">![Ajout d’un élément existant](aspnet-mvc-4-models-and-data-access/_static/image2.png "Ajout d’un élément existant")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-161">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="dd1cb-162">*Ajout d’un élément existant*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-162">*Adding an Existing Item*</span></span>

    <span data-ttu-id="dd1cb-163">![Fichier de base de données MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "fichier de base de données MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-163">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="dd1cb-164">*Fichier de base de données MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-164">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="dd1cb-165">La base de données a été ajouté au projet.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-165">The database has been added to the project.</span></span> <span data-ttu-id="dd1cb-166">Même lorsque la base de données se trouve à l’intérieur de la solution, vous pouvez interroger et mettre à jour comme il était hébergé sur un serveur de base de données différente.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-166">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="dd1cb-167">![MvcMusicStore de base de données dans l’Explorateur de solutions](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore de base de données dans l’Explorateur de solutions")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-167">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="dd1cb-168">*MvcMusicStore de base de données dans l’Explorateur de solutions*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-168">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="dd1cb-169">Vérifiez la connexion à la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-169">Verify the connection to the database.</span></span> <span data-ttu-id="dd1cb-170">Pour ce faire, double-cliquez sur **MvcMusicStore.mdf** pour établir une connexion.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-170">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="dd1cb-171">![Connexion à MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "la connexion à MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-171">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="dd1cb-172">*Connexion à MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-172">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="dd1cb-173">Tâche 2 : création d’un modèle de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-173">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="dd1cb-174">Dans cette tâche, vous allez créer un modèle de données pour interagir avec la base de données ajouté dans la tâche précédente.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-174">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="dd1cb-175">Créer un modèle de données qui représente la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-175">Create a data model that will represent the database.</span></span> <span data-ttu-id="dd1cb-176">Pour ce faire, dans le droit de l’Explorateur de solutions le **modèles** dossier, pointez sur **ajouter** puis cliquez sur **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-176">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="dd1cb-177">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez le **données** modèle, puis le **ADO.NET Entity Data Model** élément.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-177">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="dd1cb-178">Modifier le nom du modèle de données pour **StoreDB.edmx** et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-178">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="dd1cb-179">![Ajout de StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Ajout StoreDB ADO.NET Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-179">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="dd1cb-180">*Ajout de StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-180">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="dd1cb-181">Le **Assistant Entity Data Model** s’affiche.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-181">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="dd1cb-182">Cet Assistant vous guidera tout au long de la création de la couche de modèle.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-182">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="dd1cb-183">Étant donné que le modèle doit être créé en fonction de la recentyl de base de données existante ajouté, sélectionnez **générer à partir de la base de données** et cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-183">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="dd1cb-184">![Choisir le contenu du modèle](aspnet-mvc-4-models-and-data-access/_static/image7.png "en choisissant le contenu du modèle")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-184">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="dd1cb-185">*Choisir le contenu du modèle*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-185">*Choosing the model content*</span></span>
3. <span data-ttu-id="dd1cb-186">Étant donné que vous générez un modèle à partir d’une base de données, vous devez spécifier la connexion à utiliser.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-186">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="dd1cb-187">Cliquez sur **nouvelle connexion**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-187">Click **New Connection**.</span></span>
4. <span data-ttu-id="dd1cb-188">Sélectionnez **fichier de base de données Microsoft SQL Server** et cliquez sur **continuer**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-188">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="dd1cb-189">![Choisir les sources de données](aspnet-mvc-4-models-and-data-access/_static/image8.png "choisir les sources de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-189">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="dd1cb-190">*Choisissez de dialogue source de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-190">*Choose data source dialog*</span></span>
5. <span data-ttu-id="dd1cb-191">Cliquez sur **Parcourir** et sélectionnez la base de données **MvcMusicStore.mdf** vous avez localisé à le **application\_données** dossier puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-191">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="dd1cb-192">![Propriétés de connexion](aspnet-mvc-4-models-and-data-access/_static/image9.png "propriétés de connexion")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-192">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="dd1cb-193">*Propriétés de connexion*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-193">*Connection properties*</span></span>
6. <span data-ttu-id="dd1cb-194">La classe générée doit avoir le même nom que la chaîne de connexion d’entité, donc modifier son nom à **MusicStoreEntities** et cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-194">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="dd1cb-195">![Choix de la connexion de données](aspnet-mvc-4-models-and-data-access/_static/image10.png "choix de la connexion de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-195">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="dd1cb-196">*Choix de la connexion de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-196">*Choosing the data connection*</span></span>
7. <span data-ttu-id="dd1cb-197">Choisissez les objets de base de données à utiliser.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-197">Choose the database objects to use.</span></span> <span data-ttu-id="dd1cb-198">Comme le modèle d’entité utilisera uniquement les tables de la base de données, sélectionnez le **Tables** option et vous assurer que le **inclure les colonnes clés étrangères dans le modèle** et **mettre au pluriel ou au singulier généré noms d’objets** options sont également sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-198">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="dd1cb-199">Modifier le modèle Namespace pour **MvcMusicStore.Model** et cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-199">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="dd1cb-200">![Choisir les objets de base de données](aspnet-mvc-4-models-and-data-access/_static/image11.png "en choisissant les objets de base de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-200">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="dd1cb-201">*Choisir les objets de base de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-201">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-202">Si une boîte de dialogue Avertissement de sécurité s’affiche, cliquez sur **OK** pour exécuter le modèle et de générer les classes pour les entités du modèle.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-202">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="dd1cb-203">Un diagramme d’entité pour la base de données s’affiche pendant la création d’une classe distincte qui mappe chaque table à la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-203">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="dd1cb-204">Par exemple, le **Albums** table est représentée par une **Album** (classe), où chaque colonne dans la table mappera à une propriété de classe.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-204">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="dd1cb-205">Cela vous permettra d’interroger et manipuler des objets qui représentent des lignes dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-205">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="dd1cb-206">![Diagramme de l’entité](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagramme d’entité")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-206">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="dd1cb-207">*Diagramme d’entité*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-207">*Entity diagram*</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-208">Les modèles T4 (.tt) exécuter du code pour générer les classes d’entités et remplacement les classes existantes portant le même nom.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-208">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="dd1cb-209">Dans cet exemple, les classes &quot;Album&quot;, &quot;Genre&quot; et &quot;artiste&quot; ont été remplacés par le code généré.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-209">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="dd1cb-210">Tâche 3 : génération de l’Application</span><span class="sxs-lookup"><span data-stu-id="dd1cb-210">Task 3 - Building the Application</span></span>

<span data-ttu-id="dd1cb-211">Dans cette tâche, vous allez vérifier que, bien que la génération du modèle ont supprimé le **Album**, **Genre** et **artiste** classes de modèle, le projet est généré avec succès à l’aide de les nouvelles classes de modèle de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-211">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="dd1cb-212">Générez le projet en sélectionnant le **générer** élément de menu, puis **MvcMusicStore de Build**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-212">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="dd1cb-213">![Génération du projet](aspnet-mvc-4-models-and-data-access/_static/image13.png "génération du projet")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-213">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="dd1cb-214">*Génération du projet*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-214">*Building the project*</span></span>
2. <span data-ttu-id="dd1cb-215">Le projet est généré avec succès.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-215">The project builds successfully.</span></span> <span data-ttu-id="dd1cb-216">Pourquoi toujours fonctionne-t-il ?</span><span class="sxs-lookup"><span data-stu-id="dd1cb-216">Why does it still work?</span></span> <span data-ttu-id="dd1cb-217">Cela fonctionne parce que les tables de base de données ont des champs qui incluent les propriétés que vous utilisiez dans les classes supprimés **Album** et **Genre**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-217">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="dd1cb-218">![Réussi des builds](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds a réussi")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-218">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="dd1cb-219">*Réussi des builds*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-219">*Builds succeeded*</span></span>
3. <span data-ttu-id="dd1cb-220">Tandis que le concepteur affiche les entités sous forme de diagramme, elles sont vraiment des classes c#.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-220">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="dd1cb-221">Développez le **StoreDB.edmx** nœud dans l’Explorateur de solutions, puis **StoreDB.tt**, vous verrez les nouvelles entités générées.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-221">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="dd1cb-222">![Les fichiers générés](aspnet-mvc-4-models-and-data-access/_static/image15.png "les fichiers générés")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-222">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="dd1cb-223">*Fichiers générés*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-223">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="dd1cb-224">Tâche 4 : interrogation de la base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-224">Task 4 - Querying the Database</span></span>

<span data-ttu-id="dd1cb-225">Dans cette tâche, la classe StoreController met à jour, afin que, au lieu d’utiliser les données codées en dur, il interrogera la base de données pour récupérer les informations.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-225">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="dd1cb-226">Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la **MusicStoreEntities** classe, nommée **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="dd1cb-226">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="dd1cb-227">(Code d’extrait de code - *modèles et accès aux données - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-227">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="dd1cb-228">Le **MusicStoreEntities** classe expose une propriété de collection pour chaque table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-228">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="dd1cb-229">Mise à jour **Parcourir** méthode d’action pour récupérer un Genre avec tous les **Albums**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-229">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="dd1cb-230">(Code d’extrait de code - *modèles et accès aux données - Ex1 magasin Parcourir*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-230">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="dd1cb-231">Vous utilisez une fonctionnalité de .NET appelée **LINQ** (language integrated query) pour écrire des expressions de requête fortement typée par rapport à ces regroupements - qui exécutera le code par rapport à la base de données et retourner des objets que vous pouvez programmer par rapport aux.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-231">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="dd1cb-232">Pour plus d’informations à propos de LINQ, visitez le [site msdn](https://msdn.microsoft.com/en-us/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-232">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/en-us/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="dd1cb-233">Mise à jour **Index** pour récupérer tous les genres des méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="dd1cb-234">(Code d’extrait de code - *modèles et accès aux données - Index de magasin Ex1*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="dd1cb-235">Mise à jour **Index** méthode d’action pour récupérer tous les genres et de transformer une liste à la collection.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="dd1cb-236">(Code d’extrait de code - *modèles et accès aux données - Ex1 magasin GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="dd1cb-237">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="dd1cb-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="dd1cb-238">Dans cette tâche, vous allez vérifier que la page d’Index du magasin s’affichent désormais les Genres stockées dans la base de données au lieu de ceux qui sont codées.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="dd1cb-239">Il est inutile de modifier le modèle d’affichage, car le **StoreController** retourne les mêmes entités comme avant, bien que cette fois les données proviennent la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="dd1cb-240">Régénérez la solution et appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="dd1cb-241">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-241">The project starts in the Home page.</span></span> <span data-ttu-id="dd1cb-242">Vérifiez que le menu de **Genres** n’est plus une liste codée en dur, et les données sont récupérées directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="dd1cb-244">*Genres de navigation à partir de la base de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="dd1cb-245">Maintenant, accédez à n’importe quel genre et vérifier que les albums sont remplis à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="dd1cb-246">![Exploration des Albums à partir de la base de données](aspnet-mvc-4-models-and-data-access/_static/image17.png "Albums de navigation à partir de la base de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="dd1cb-247">*Exploration des Albums à partir de la base de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="dd1cb-248">Exercice 2 : Création d’une base de données à l’aide de Code tout d’abord</span><span class="sxs-lookup"><span data-stu-id="dd1cb-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="dd1cb-249">Dans cet exercice, vous allez apprendre comment utiliser l’approche Code First pour créer une base de données avec les tables de l’application MusicStore et comment accéder à ses données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="dd1cb-250">Une fois que le modèle est généré, vous allez modifier le StoreController pour fournir le modèle d’affichage avec les données provenant de la base de données, au lieu d’utiliser des valeurs codées en dur.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-251">Si vous avez terminé l’exercice 1 et que vous avez déjà travaillé avec la première base de données approche, vous allez maintenant apprendre à obtenir les mêmes résultats avec un autre processus.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="dd1cb-252">Les tâches qui sont en commun avec exercice 1 ont été marquées pour faciliter la lecture.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="dd1cb-253">Si vous n’avez pas effectué exercice 1, mais pour en savoir l’approche Code First, vous pouvez démarrer à partir de cet exercice et obtenir une couverture complète de la rubrique.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="dd1cb-254">Tâche 1 - remplissage exemples de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="dd1cb-255">Dans cette tâche, vous allez renseigner la base de données avec des exemples de données lors de sa création exécuter initialement à l’aide de Code en premier.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="dd1cb-256">Ouvrez le **commencer** solution situé dans **début/CreatingADatabaseCodeFirst-Ex2/Source/** dossier.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="dd1cb-257">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="dd1cb-258">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dd1cb-259">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="dd1cb-260">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="dd1cb-261">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-262">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dd1cb-263">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dd1cb-264">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dd1cb-265">Ajouter le **SampleData.cs** de fichiers à la **modèles** dossier.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="dd1cb-266">Pour ce faire, cliquez sur **modèles** dossier, pointez sur **ajouter** puis cliquez sur **élément existant**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="dd1cb-267">Accédez à **\Source\Assets** et sélectionnez le **SampleData.cs** fichier.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="dd1cb-268">![Exemples de données remplissent code](aspnet-mvc-4-models-and-data-access/_static/image18.png "code remplissent les exemples de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="dd1cb-269">*Exemples de données remplissent le code*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="dd1cb-270">Ouvrez le **Global.asax.cs** de fichiers et ajoutez le code suivant *à l’aide de* instructions.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="dd1cb-271">(Code d’extrait de code - *modèles et accès aux données - Ex2 les instructions Using Asax Global*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="dd1cb-272">Dans le **Application\_Start()** méthode ajoute la ligne suivante pour définir l’initialiseur de base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="dd1cb-273">(Code d’extrait de code - *modèles et accès aux données - Ex2 Global Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="dd1cb-274">Tâche 2 : configuration de la connexion à la base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="dd1cb-275">Maintenant que vous avez déjà ajouté une base de données à notre projet, vous permet d’écrire dans le **Web.config** la chaîne de connexion de fichiers.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="dd1cb-276">Ajouter une chaîne de connexion à **Web.config**. Pour ce faire, ouvrez **Web.config** à la racine du projet et de remplacer la chaîne de connexion nommée DefaultConnection par cette ligne dans le  **&lt;connectionStrings&gt;**  section :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="dd1cb-277">![Emplacement du fichier Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "emplacement du fichier Web.config")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="dd1cb-278">*Emplacement du fichier Web.config*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-278">*Web.config file location*</span></span>


    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="dd1cb-279">Tâche 3 : utilisation du modèle</span><span class="sxs-lookup"><span data-stu-id="dd1cb-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="dd1cb-280">Maintenant que vous avez déjà configuré la connexion à la base de données, vous allez lier le modèle avec les tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="dd1cb-281">Dans cette tâche, vous allez créer une classe qui sera liée à la base de données Code First.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="dd1cb-282">N’oubliez pas qu’il existe une classe de modèle POCO existante qui doit être modifiée.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-282">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-283">Si vous terminé l’exercice 1, vous noterez que cette étape a été effectuée par un Assistant.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="dd1cb-284">Vous créerez manuellement en procédant comme Code First, les classes qui seront liés à des entités de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="dd1cb-285">Ouvrez la classe de modèle POCO **Genre** de **modèles** dossier de projet et d’inclure un code.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="dd1cb-286">Utiliser une propriété de type int avec le nom **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="dd1cb-287">(Code d’extrait de code - *modèles et accès aux données - Genre de premier Code Ex2*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="dd1cb-288">Pour utiliser des conventions de Code First, la Genre de classe doit avoir une propriété de clé primaire sera automatiquement détectée.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-288">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="dd1cb-289">Vous pouvez en savoir plus sur les Conventions de premier Code de cette [article msdn](https://msdn.microsoft.com/en-us/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-289">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/en-us/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="dd1cb-290">Maintenant, ouvrez la classe de modèle POCO **Album** de **modèles** dossier de projet et inclure les clés étrangères, créer des propriétés avec les noms **GenreId** et  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-290">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="dd1cb-291">Cette classe possède déjà le **GenreId** pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-291">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="dd1cb-292">(Code d’extrait de code - *modèles et accès aux données - Ex2 Code premier Album*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-292">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="dd1cb-293">Ouvrez la classe de modèle POCO **artiste** et inclure le **ArtistId** propriété.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-293">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="dd1cb-294">(Code d’extrait de code - *modèles et accès aux données - artiste premier Code de Ex2*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-294">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="dd1cb-295">Cliquez sur le **modèles** dossier du projet et sélectionnez **ajouter | Classe**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-295">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="dd1cb-296">Nommez le fichier **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-296">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="dd1cb-297">Ensuite, cliquez sur **ajouter.**</span><span class="sxs-lookup"><span data-stu-id="dd1cb-297">Then, click **Add.**</span></span>

    <span data-ttu-id="dd1cb-298">![Ajout d’une classe](aspnet-mvc-4-models-and-data-access/_static/image20.png "Ajout d’une classe")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-298">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="dd1cb-299">*Ajouter un nouvel élément*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-299">*Adding a new item*</span></span>

    <span data-ttu-id="dd1cb-300">![Ajout d’un classe2](aspnet-mvc-4-models-and-data-access/_static/image21.png "ajoutant un classe2")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-300">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="dd1cb-301">*Ajout d’une classe*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-301">*Adding a class*</span></span>
5. <span data-ttu-id="dd1cb-302">Ouvrez la classe que vous venez de créer, **MusicStoreEntities.cs**et incluez les espaces de noms **System.Data.Entity** et **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-302">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="dd1cb-303">Remplacez la déclaration de classe pour étendre le **DbContext** classe : déclarer un public **DBSet** et remplacez **OnModelCreating** (méthode).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-303">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="dd1cb-304">Après cette étape, vous obtiendrez une classe de domaine qui établit un lien votre modèle avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-304">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="dd1cb-305">Pour ce faire, remplacez le code de classe avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-305">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="dd1cb-306">(Code d’extrait de code - *modèles et accès aux données - MusicStoreEntities premier Code de Ex2*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-306">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="dd1cb-307">Avec Entity Framework **DbContext** et **DBSet** vous serez en mesure d’interroger la classe POCO Genre.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-307">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="dd1cb-308">En étendant **OnModelCreating** (méthode), vous spécifiez dans le **code** comment Genre est mappée à une table de base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-308">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="dd1cb-309">Vous trouverez plus d’informations sur DBContext et DBSet dans cet article msdn : [lien](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-309">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="dd1cb-310">Tâche 4 : interrogation de la base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-310">Task 4 - Querying the Database</span></span>

<span data-ttu-id="dd1cb-311">Dans cette tâche, la classe StoreController met à jour, afin que, au lieu d’utiliser les données codées en dur, il sera le récupérer à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-311">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-312">Cette tâche est en commun avec l’exercice 1.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-312">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="dd1cb-313">Si vous avez terminé l’exercice 1 vous noterez que ces étapes sont identiques dans les deux approches (tout d’abord la base de données ou Code first).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-313">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="dd1cb-314">Elles sont différentes dans la façon dont les données sont liées avec le modèle, mais l’accès à des entités de données est encore transparent à partir du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-314">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="dd1cb-315">Ouvrez **Controllers\StoreController.cs** et ajoutez le champ suivant à la classe pour contenir une instance de la **MusicStoreEntities** classe, nommée **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="dd1cb-315">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="dd1cb-316">(Code d’extrait de code - *modèles et accès aux données - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-316">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="dd1cb-317">Le **MusicStoreEntities** classe expose une propriété de collection pour chaque table dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-317">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="dd1cb-318">Mise à jour **Parcourir** méthode d’action pour récupérer un Genre avec tous les **Albums**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-318">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="dd1cb-319">(Code d’extrait de code - *modèles et accès aux données - Ex2 magasin Parcourir*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-319">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="dd1cb-320">Vous utilisez une fonctionnalité de .NET appelée **LINQ** (language integrated query) pour écrire des expressions de requête fortement typée par rapport à ces regroupements - qui exécutera le code par rapport à la base de données et retourner des objets que vous pouvez programmer par rapport aux.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-320">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="dd1cb-321">Pour plus d’informations à propos de LINQ, visitez le [site msdn](https://msdn.microsoft.com/en-us/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-321">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/en-us/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="dd1cb-322">Mise à jour **Index** pour récupérer tous les genres des méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-322">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="dd1cb-323">(Code d’extrait de code - *modèles et accès aux données - Index de magasin Ex2*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-323">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="dd1cb-324">Mise à jour **Index** méthode d’action pour récupérer tous les genres et de transformer une liste à la collection.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-324">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="dd1cb-325">(Code d’extrait de code - *modèles et accès aux données - Ex2 magasin GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-325">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="dd1cb-326">Tâche 5 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="dd1cb-326">Task 5 - Running the Application</span></span>

<span data-ttu-id="dd1cb-327">Dans cette tâche, vous allez vérifier que la page d’Index du magasin s’affichent désormais les Genres stockées dans la base de données au lieu de ceux qui sont codées.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-327">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="dd1cb-328">Il est inutile de modifier le modèle d’affichage, car le **StoreController** retourne le même **StoreIndexViewModel** comme avant, mais cette fois les données proviennent de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-328">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="dd1cb-329">Régénérez la solution et appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-329">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="dd1cb-330">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-330">The project starts in the Home page.</span></span> <span data-ttu-id="dd1cb-331">Vérifiez que le menu de **Genres** n’est plus une liste codée en dur, et les données sont récupérées directement à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-331">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="dd1cb-333">*Genres de navigation à partir de la base de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-333">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="dd1cb-334">Maintenant, accédez à n’importe quel genre et vérifier que les albums sont remplis à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-334">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="dd1cb-335">![Exploration des Albums à partir de la base de données](aspnet-mvc-4-models-and-data-access/_static/image23.png "Albums de navigation à partir de la base de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-335">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="dd1cb-336">*Exploration des Albums à partir de la base de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-336">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="dd1cb-337">Exercice 3 : Interrogation de la base de données avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="dd1cb-337">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="dd1cb-338">Dans cet exercice, vous allez apprendre comment interroger la base de données à l’aide de paramètres et comment utiliser la mise en forme du résultat de requête, une fonctionnalité qui permet de réduire la base de données nombre accède à la récupération des données de manière plus efficace.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-338">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-339">Pour plus d’informations sur la mise en forme du résultat de requête, visitez [article msdn](https://msdn.microsoft.com/en-us/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-339">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/en-us/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="dd1cb-340">Tâche 1 - modification StoreController pour récupérer des Albums à partir de la base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-340">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="dd1cb-341">Dans cette tâche, vous allez modifier le **StoreController** classe pour accéder à la base de données pour récupérer des albums d’un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-341">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="dd1cb-342">Ouvrez le **commencer** solution situé dans le **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** dossier si vous souhaitez utiliser l’approche Code ou **Source\ Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** dossier si vous souhaitez utiliser l’approche de base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-342">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="dd1cb-343">Dans le cas contraire, vous pouvez continuer à utiliser le **fin** solution obtenue par la fin de l’exercice précédent.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-343">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="dd1cb-344">Si vous avez ouvert le **commencer** solution, vous devez télécharger des packages NuGet manquants avant de poursuivre.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-344">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dd1cb-345">Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-345">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="dd1cb-346">Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-346">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="dd1cb-347">Enfin, générez la solution en cliquant sur **générer** | **générer la Solution**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-347">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-348">Un des avantages de l’utilisation de NuGet est que vous ne devez expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-348">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dd1cb-349">Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques requises à la première fois que vous exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-349">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dd1cb-350">C’est pourquoi vous devez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-350">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dd1cb-351">Ouvrez le **StoreController** classe pour modifier le **Parcourir** méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-351">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="dd1cb-352">Pour ce faire, dans le **l’Explorateur de solutions**, développez le **contrôleurs** et double-cliquez sur **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-352">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="dd1cb-353">Modifier la **Parcourir** méthode d’action pour récupérer les albums pour un genre spécifique.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-353">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="dd1cb-354">Pour ce faire, remplacez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-354">To do this, replace the following code:</span></span>

    <span data-ttu-id="dd1cb-355">(Code d’extrait de code - *modèles et accès aux données - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-355">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="dd1cb-356">Pour remplir une collection de l’entité, vous devez utiliser le **Include** méthode pour spécifier à extraire les albums trop.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-356">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="dd1cb-357">Vous pouvez utiliser le. **Single()** extension dans LINQ, car dans ce cas qu’un seul genre est attendu pour un album.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-357">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="dd1cb-358">Le **Single()** méthode prend une expression Lambda en tant que paramètre, qui dans ce cas spécifie un seul objet Genre telles que son nom correspond à la valeur définie.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-358">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
    > 
    > <span data-ttu-id="dd1cb-359">Vous tirera parti d’une fonctionnalité qui permet d’indiquer d’autres entités associées, vous souhaitez également chargées lorsque l’objet de Genre est récupéré.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-359">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="dd1cb-360">Cette fonctionnalité est appelée **mise en forme du résultat de requête**et vous permet de réduire le nombre de fois que nécessaire pour accéder à la base de données pour récupérer des informations.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-360">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="dd1cb-361">Dans ce scénario, vous devez au préalable les Albums pour le Genre que vous récupérez.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-361">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
    > 
    > <span data-ttu-id="dd1cb-362">La requête inclut **Genres.Include (&quot;Albums&quot;)** pour indiquer que vous voulez également les albums associés.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-362">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="dd1cb-363">Cela entraîne une application plus efficace, car il récupère les données de Genre et Album dans une requête de base de données unique.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-363">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="dd1cb-364">Tâche 2 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="dd1cb-364">Task 2 - Running the Application</span></span>

<span data-ttu-id="dd1cb-365">Dans cette tâche, vous exécutez l’application et récupérer des albums d’un genre spécifique à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-365">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="dd1cb-366">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-366">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="dd1cb-367">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-367">The project starts in the Home page.</span></span> <span data-ttu-id="dd1cb-368">Modifier l’URL pour **/magasin/Parcourir ? genre = Pop** pour vérifier que les résultats sont récupérées à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-368">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="dd1cb-369">![Navigation par genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "navigation par genre")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-369">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="dd1cb-370">*Navigation magasin/Parcourir ? genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-370">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="dd1cb-371">Tâche 3 : l’accès à des Albums par Id</span><span class="sxs-lookup"><span data-stu-id="dd1cb-371">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="dd1cb-372">Dans cette tâche, vous recommence la procédure précédente pour obtenir des albums par leur Id.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-372">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="dd1cb-373">Fermez le navigateur si nécessaire, pour revenir à Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-373">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="dd1cb-374">Ouvrez le **StoreController** classe pour modifier le **détails** méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-374">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="dd1cb-375">Pour ce faire, dans le **l’Explorateur de solutions**, développez le **contrôleurs** et double-cliquez sur **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-375">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="dd1cb-376">Modifier la **détails** méthode d’action pour récupérer les détails des albums selon leur **Id**. Pour ce faire, remplacez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-376">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="dd1cb-377">(Code d’extrait de code - *modèles et accès aux données - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="dd1cb-377">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="dd1cb-378">Tâche 4 : exécution de l’Application</span><span class="sxs-lookup"><span data-stu-id="dd1cb-378">Task 4 - Running the Application</span></span>

<span data-ttu-id="dd1cb-379">Dans cette tâche, vous exécutez l’Application dans un navigateur web et obtenir des détails de l’album par leur Id.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-379">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="dd1cb-380">Appuyez sur **F5** pour exécuter l’Application.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-380">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="dd1cb-381">Le projet de démarrage dans la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-381">The project starts in the Home page.</span></span> <span data-ttu-id="dd1cb-382">Modifier l’URL pour **/Store/Details/51** ou parcourir les genres et sélectionnez un album pour vérifier que les résultats sont récupérées à partir de la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-382">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="dd1cb-383">![Exploration des détails](aspnet-mvc-4-models-and-data-access/_static/image25.png "exploration des détails")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-383">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="dd1cb-384">*Navigation /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-384">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="dd1cb-385">En outre, vous pouvez déployer cette application à Sites Web Windows Azure suit [annexe b : publication une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-385">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="dd1cb-386">Résumé</span><span class="sxs-lookup"><span data-stu-id="dd1cb-386">Summary</span></span>

<span data-ttu-id="dd1cb-387">En fin de cet atelier pratique que vous avez appris les notions de base de l’accès aux données et les modèles ASP.NET MVC, à l’aide de la **Database First** approche, ainsi que les **Code First** approche :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-387">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="dd1cb-388">L’ajout d’une base de données à la solution pour pouvoir utiliser ses données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-388">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="dd1cb-389">Comment mettre à jour les contrôleurs pour fournir des modèles d’affichage avec les données provenant de la base de données et non codées en dur</span><span class="sxs-lookup"><span data-stu-id="dd1cb-389">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="dd1cb-390">Comment interroger la base de données à l’aide de paramètres</span><span class="sxs-lookup"><span data-stu-id="dd1cb-390">How to query the database using parameters</span></span>
- <span data-ttu-id="dd1cb-391">L’utilisation de la requête résultat mise en forme, une fonctionnalité qui permet de réduire le nombre d’accès de la base de données, la récupération des données de manière plus efficace.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-391">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="dd1cb-392">Comment utiliser les approches de base de données et le Code First dans Microsoft Entity Framework pour lier la base de données avec le modèle</span><span class="sxs-lookup"><span data-stu-id="dd1cb-392">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="dd1cb-393">Annexe a : installation de Visual Studio Express 2012 pour le Web</span><span class="sxs-lookup"><span data-stu-id="dd1cb-393">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="dd1cb-394">Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="dd1cb-394">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="dd1cb-395">Les instructions suivantes vous guident à travers les étapes requises pour installer *Visual studio Express 2012 pour le Web* à l’aide de *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-395">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="dd1cb-396">Accédez à [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-396">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="dd1cb-397">Sinon, si vous avez déjà installé Web Platform Installer, vous pouvez ouvrir il et recherchez le produit &quot; *Visual Studio Express 2012 pour le Web avec Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-397">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="dd1cb-398">Cliquez sur **installer maintenant**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-398">Click on **Install Now**.</span></span> <span data-ttu-id="dd1cb-399">Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-399">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="dd1cb-400">Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-400">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="dd1cb-401">![Installer Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "installer Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-401">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="dd1cb-402">*Installer Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-402">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="dd1cb-403">Lisez les termes et les licences de tous les produits et cliquez sur **J’accepte** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-403">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accepter les termes du contrat de licence](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="dd1cb-405">*Accepter les termes du contrat de licence*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-405">*Accepting the license terms*</span></span>
5. <span data-ttu-id="dd1cb-406">Attendez que le processus de téléchargement et l’installation se termine.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-406">Wait until the downloading and installation process completes.</span></span>

    ![Progression de l'installation](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="dd1cb-408">*Progression de l’installation*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-408">*Installation progress*</span></span>
6. <span data-ttu-id="dd1cb-409">Une fois l’installation terminée, cliquez sur **Terminer**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-409">When the installation completes, click **Finish**.</span></span>

    ![Installation est terminée](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="dd1cb-411">*Installation est terminée*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-411">*Installation completed*</span></span>
7. <span data-ttu-id="dd1cb-412">Cliquez sur **Exit** fermer Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-412">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="dd1cb-413">Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-413">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express pour la vignette du Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="dd1cb-415">*VS Express pour la vignette du Web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-415">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dd1cb-416">Annexe b : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dd1cb-416">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="dd1cb-417">Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail de gestion Windows Azure et de publier l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-417">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="dd1cb-418">Tâche 1 - Création d’un Site Web à partir de Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dd1cb-418">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="dd1cb-419">Accédez à la [portail de gestion Windows Azure](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-419">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-420">Avec Windows Azure, vous pouvez héberger des Sites Web ASP.NET 10 gratuitement et puis faire évoluer à mesure que votre trafic augmente.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-420">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="dd1cb-421">Vous pouvez vous inscrire [ici](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-421">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="dd1cb-422">![Ouvrez une session sur le portail Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "connectez-vous au portail Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-422">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="dd1cb-423">*Ouvrez une session sur le portail de gestion Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-423">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="dd1cb-424">Cliquez sur **nouveau** sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-424">Click **New** on the command bar.</span></span>

    <span data-ttu-id="dd1cb-425">![Création d’un Site Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "création d’un Site Web")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-425">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="dd1cb-426">*Création d’un Site Web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-426">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="dd1cb-427">Cliquez sur **de calcul** | **Site Web**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-427">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="dd1cb-428">Puis sélectionnez **création rapide** option.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-428">Then select **Quick Create** option.</span></span> <span data-ttu-id="dd1cb-429">Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-429">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-430">Un Site Web de Windows Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-430">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="dd1cb-431">L’option Création rapide vous permet de déployer une application web complète pour le Site Web Windows Azure à partir d’à l’extérieur du portail.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-431">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="dd1cb-432">Il n’inclut pas les étapes de configuration d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-432">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="dd1cb-433">![Création d’un Site Web à l’aide de la création rapide](aspnet-mvc-4-models-and-data-access/_static/image33.png "création d’un Site Web à l’aide de la création rapide")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-433">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="dd1cb-434">*Création d’un Site Web à l’aide de la création rapide*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-434">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="dd1cb-435">Attendez que la nouvelle **Site Web** est créé.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-435">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="dd1cb-436">Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-436">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="dd1cb-437">Vérifiez que le nouveau Site Web fonctionne.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-437">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="dd1cb-438">![Navigation vers le nouveau site web](aspnet-mvc-4-models-and-data-access/_static/image34.png "exploration vers le nouveau site web")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-438">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="dd1cb-439">*Navigation vers le nouveau site web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-439">*Browsing to the new web site*</span></span>

    <span data-ttu-id="dd1cb-440">![Site Web en cours d’exécution](aspnet-mvc-4-models-and-data-access/_static/image35.png "site Web en cours d’exécution")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-440">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="dd1cb-441">*Site Web en cours d’exécution*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-441">*Web site running*</span></span>
6. <span data-ttu-id="dd1cb-442">Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-442">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="dd1cb-443">![Ouvrir les pages de gestion du site web](aspnet-mvc-4-models-and-data-access/_static/image36.png "ouvrir les pages de gestion de site web")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-443">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="dd1cb-444">*Ouvrir les pages de gestion de Site Web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-444">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="dd1cb-445">Dans le **tableau de bord** sous le **coup de œil rapide** , cliquez sur le **télécharger le profil de publication** lien.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-445">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-446">Le *le profil de publication* contient toutes les informations nécessaires pour publier une application web sur un site Web de Windows Azure pour chaque méthode de publication activée.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-446">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="dd1cb-447">Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour se connecter à et de s’authentifier auprès de chacun des points de terminaison pour laquelle une méthode de publication est activée.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-447">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="dd1cb-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge la lecture de publier les profils pour automatiser la configuration de ces programmes pour publication d’applications web pour les sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="dd1cb-449">![Profil de publication de téléchargement du site web](aspnet-mvc-4-models-and-data-access/_static/image37.png "profil de publication de téléchargement du site web")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-449">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="dd1cb-450">*Profil de publication de téléchargement du Site Web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-450">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="dd1cb-451">Téléchargez le fichier de profil de publication à un emplacement connu.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-451">Download the publish profile file to a known location.</span></span> <span data-ttu-id="dd1cb-452">Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web à un Sites Web Windows Azure à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-452">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="dd1cb-453">![L’enregistrement du fichier de profil de publication](aspnet-mvc-4-models-and-data-access/_static/image38.png "l’enregistrement du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-453">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="dd1cb-454">*L’enregistrement du fichier de profil de publication*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-454">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="dd1cb-455">Tâche 2 : configuration du serveur de base de données</span><span class="sxs-lookup"><span data-stu-id="dd1cb-455">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="dd1cb-456">Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-456">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="dd1cb-457">Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-457">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="dd1cb-458">Vous devez un serveur de base de données SQL pour stocker la base de données de l’application.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-458">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="dd1cb-459">Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Windows Azure à **bases de données Sql** | **serveurs** | **du serveur Tableau de bord**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-459">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="dd1cb-460">Si vous ne disposez pas d’un serveur créé, vous pouvez créer un à l’aide de la **ajouter** bouton sur la barre de commandes.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-460">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="dd1cb-461">Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et un mot de passe**, comme vous allez l’utiliser dans les tâches suivantes.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-461">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="dd1cb-462">Ne créez pas encore, la base de données telle qu’elle sera créée dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-462">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="dd1cb-463">![Tableau de bord de serveur SQL de base de données](aspnet-mvc-4-models-and-data-access/_static/image39.png "tableau de bord de serveur SQL de base de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-463">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="dd1cb-464">*Tableau de bord de serveur SQL de base de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-464">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="dd1cb-465">Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-465">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="dd1cb-466">Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP du Client actuel** et le coller dans le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) bouton.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-466">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Ajout d’adresse IP du Client](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="dd1cb-468">*Ajout d’adresse IP du Client*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-468">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="dd1cb-469">Une fois la **adresse IP du Client** est ajouté aux adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-469">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmer les modifications](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="dd1cb-471">*Confirmer les modifications*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-471">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dd1cb-472">Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy</span><span class="sxs-lookup"><span data-stu-id="dd1cb-472">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="dd1cb-473">Revenez à la solution ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-473">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="dd1cb-474">Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-474">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="dd1cb-475">![Publication de l’Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "publication de l’Application")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-475">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="dd1cb-476">*Publier le site web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-476">*Publishing the web site*</span></span>
2. <span data-ttu-id="dd1cb-477">Importer le profil de publication que vous avez enregistré dans la première tâche.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-477">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="dd1cb-478">![L’importation du profil de publication](aspnet-mvc-4-models-and-data-access/_static/image44.png "l’importation du profil de publication")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-478">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="dd1cb-479">*Importation du profil de publication*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-479">*Importing publish profile*</span></span>
3. <span data-ttu-id="dd1cb-480">Cliquez sur **valider la connexion**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-480">Click **Validate Connection**.</span></span> <span data-ttu-id="dd1cb-481">Une fois la Validation terminée. Cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-481">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dd1cb-482">La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-482">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="dd1cb-483">![Validation de la connexion](aspnet-mvc-4-models-and-data-access/_static/image45.png "validation de la connexion")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-483">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="dd1cb-484">*Validation de la connexion*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-484">*Validating connection*</span></span>
4. <span data-ttu-id="dd1cb-485">Dans le **paramètres** sous le **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-485">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="dd1cb-486">![Configuration de déploiement Web](aspnet-mvc-4-models-and-data-access/_static/image46.png "configuration de déploiement Web")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-486">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="dd1cb-487">*Configuration de déploiement Web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-487">*Web deploy configuration*</span></span>
5. <span data-ttu-id="dd1cb-488">Configurer la connexion de base de données comme suit :</span><span class="sxs-lookup"><span data-stu-id="dd1cb-488">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="dd1cb-489">Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-489">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="dd1cb-490">Dans **nom d’utilisateur** tapez le nom de connexion de votre administrateur de serveur.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-490">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="dd1cb-491">Dans **mot de passe** votre mot de passe du compte de connexion administrateur serveur.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-491">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="dd1cb-492">Tapez un nouveau nom de base de données.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-492">Type a new database name.</span></span>

    <span data-ttu-id="dd1cb-493">![Configuration de chaîne de connexion de destination](aspnet-mvc-4-models-and-data-access/_static/image47.png "configuration de chaîne de connexion de destination")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-493">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="dd1cb-494">*Configuration de chaîne de connexion de destination*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-494">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="dd1cb-495">Cliquez ensuite sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-495">Then click **OK**.</span></span> <span data-ttu-id="dd1cb-496">Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-496">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="dd1cb-497">![Création de la base de données](aspnet-mvc-4-models-and-data-access/_static/image48.png "création de la chaîne de la base de données")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-497">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="dd1cb-498">*Création de la base de données*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-498">*Creating the database*</span></span>
7. <span data-ttu-id="dd1cb-499">La chaîne de connexion que vous allez utiliser pour se connecter à la base de données SQL dans Windows Azure est indiquée dans la zone de texte par défaut de connexion.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-499">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="dd1cb-500">Cliquez ensuite sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-500">Then click **Next**.</span></span>

    <span data-ttu-id="dd1cb-501">![Chaîne de connexion pointant vers la base de données SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "chaîne de connexion pointant vers la base de données SQL")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-501">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="dd1cb-502">*Chaîne de connexion pointant vers la base de données SQL*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-502">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="dd1cb-503">Dans le **aperçu** , cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-503">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="dd1cb-504">![Publication de l’application web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publication de l’application web")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-504">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="dd1cb-505">*Publication de l’application web*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-505">*Publishing the web application*</span></span>
9. <span data-ttu-id="dd1cb-506">Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-506">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="dd1cb-507">Annexe c : à l’aide d’extraits de Code</span><span class="sxs-lookup"><span data-stu-id="dd1cb-507">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="dd1cb-508">Avec des extraits de code, vous avez tout le code que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-508">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="dd1cb-509">Le document lab vous indique exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-509">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="dd1cb-510">![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](aspnet-mvc-4-models-and-data-access/_static/image51.png "des extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-510">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="dd1cb-511">*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-511">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="dd1cb-512">***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***</span><span class="sxs-lookup"><span data-stu-id="dd1cb-512">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="dd1cb-513">Placez le curseur où vous souhaitez insérer le code.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-513">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="dd1cb-514">Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-514">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="dd1cb-515">Observez comment IntelliSense affiche les noms des extraits de code de mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-515">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="dd1cb-516">Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entier est sélectionné).</span><span class="sxs-lookup"><span data-stu-id="dd1cb-516">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="dd1cb-517">Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-517">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="dd1cb-518">![Commencez à taper le nom de l’extrait de code](aspnet-mvc-4-models-and-data-access/_static/image52.png "commencez à taper le nom de l’extrait de code")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-518">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="dd1cb-519">*Commencez à taper le nom de l’extrait de code*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-519">*Start typing the snippet name*</span></span>

<span data-ttu-id="dd1cb-520">![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](aspnet-mvc-4-models-and-data-access/_static/image53.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-520">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="dd1cb-521">*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-521">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="dd1cb-522">![Appuyez sur Tab à nouveau et l’extrait de code sont développés](aspnet-mvc-4-models-and-data-access/_static/image54.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-522">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="dd1cb-523">*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-523">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="dd1cb-524">***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-524">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="dd1cb-525">Clic droit où vous souhaitez insérer l’extrait de code.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-525">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="dd1cb-526">Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-526">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="dd1cb-527">Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus.</span><span class="sxs-lookup"><span data-stu-id="dd1cb-527">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="dd1cb-528">![Avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](aspnet-mvc-4-models-and-data-access/_static/image55.png "avec le bouton sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-528">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="dd1cb-529">*Avec le bouton droit sur lequel vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-529">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="dd1cb-530">![Sélectionnez l’extrait de code approprié dans la liste, en cliquant dessus](aspnet-mvc-4-models-and-data-access/_static/image56.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")</span><span class="sxs-lookup"><span data-stu-id="dd1cb-530">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="dd1cb-531">*Sélectionnez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*</span><span class="sxs-lookup"><span data-stu-id="dd1cb-531">*Pick the relevant snippet from the list, by clicking on it*</span></span>
