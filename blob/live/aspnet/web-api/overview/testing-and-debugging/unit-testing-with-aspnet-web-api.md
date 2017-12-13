---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 de tests unitaires | Documents Microsoft
author: tfitzmac
description: "Ce guide et l’application montrent comment créer des tests unitaires simple pour votre application Web API 2. Ce didacticiel montre comment inclure un projet de test unitaire..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="4a587-104">ASP.NET Web API 2 de tests unitaires</span><span class="sxs-lookup"><span data-stu-id="4a587-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="4a587-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4a587-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="4a587-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="4a587-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="4a587-107">Ce guide et l’application montrent comment créer des tests unitaires simple pour votre application Web API 2.</span><span class="sxs-lookup"><span data-stu-id="4a587-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="4a587-108">Ce didacticiel montre comment inclure un projet de test unitaire dans votre solution et écrire des méthodes de test qui vérifient les valeurs retournées à partir d’une méthode de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="4a587-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="4a587-109">Ce didacticiel suppose que vous êtes familiarisé avec les concepts de base de l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4a587-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="4a587-110">Pour un didacticiel d’introduction, consultez [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4a587-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="4a587-111">Les tests unitaires dans cette rubrique sont volontairement limitées à des scénarios de données simple.</span><span class="sxs-lookup"><span data-stu-id="4a587-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="4a587-112">Pour des scénarios plus avancés de données de tests unitaires, consultez [simulation Entity Framework lorsque unité test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="4a587-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4a587-113">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="4a587-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="4a587-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4a587-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="4a587-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="4a587-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="4a587-116">Dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="4a587-116">In this topic</span></span>

<span data-ttu-id="4a587-117">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="4a587-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="4a587-118">Composants requis</span><span class="sxs-lookup"><span data-stu-id="4a587-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="4a587-119">Télécharger le code</span><span class="sxs-lookup"><span data-stu-id="4a587-119">Download code</span></span>](#download)
- [<span data-ttu-id="4a587-120">Création d’application avec un projet de test unitaire</span><span class="sxs-lookup"><span data-stu-id="4a587-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="4a587-121">Ajouter le projet de test unitaire lors de la création de l’application</span><span class="sxs-lookup"><span data-stu-id="4a587-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="4a587-122">Ajouter un projet de test unitaire à une application existante</span><span class="sxs-lookup"><span data-stu-id="4a587-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="4a587-123">Configurer l’application Web API 2</span><span class="sxs-lookup"><span data-stu-id="4a587-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="4a587-124">Installer les packages NuGet dans le projet de test</span><span class="sxs-lookup"><span data-stu-id="4a587-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="4a587-125">Créer des tests</span><span class="sxs-lookup"><span data-stu-id="4a587-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="4a587-126">Exécuter des tests</span><span class="sxs-lookup"><span data-stu-id="4a587-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="4a587-127">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="4a587-127">Prerequisites</span></span>

<span data-ttu-id="4a587-128">Visual Studio 2017 Community, Professional ou l’édition entreprise</span><span class="sxs-lookup"><span data-stu-id="4a587-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="4a587-129">Télécharger le code</span><span class="sxs-lookup"><span data-stu-id="4a587-129">Download code</span></span>

<span data-ttu-id="4a587-130">Téléchargez le [projet achevé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="4a587-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="4a587-131">Le projet téléchargeable comprend des tests unitaires de code pour cette rubrique et la [simulation Entity Framework lorsque unité test ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) rubrique.</span><span class="sxs-lookup"><span data-stu-id="4a587-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="4a587-132">Création d’application avec un projet de test unitaire</span><span class="sxs-lookup"><span data-stu-id="4a587-132">Create application with unit test project</span></span>

<span data-ttu-id="4a587-133">Vous pouvez créer un projet de test unitaire lors de la création de votre application ou ajouter un projet de test unitaire à une application existante.</span><span class="sxs-lookup"><span data-stu-id="4a587-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="4a587-134">Ce didacticiel montre les deux méthodes pour la création d’un projet de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="4a587-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="4a587-135">Pour suivre ce didacticiel, vous pouvez utiliser des méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="4a587-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="4a587-136">Ajouter le projet de test unitaire lors de la création de l’application</span><span class="sxs-lookup"><span data-stu-id="4a587-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="4a587-137">Créer une Application Web ASP.NET nommée **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="4a587-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![créer le projet](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="4a587-139">Dans la fenêtre Nouveau projet ASP.NET, sélectionnez le **vide** modèle et ajouter des dossiers et les références de base de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="4a587-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="4a587-140">Sélectionnez le **ajouter des tests unitaires** option.</span><span class="sxs-lookup"><span data-stu-id="4a587-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="4a587-141">Le projet de test unitaire est automatiquement nommé **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="4a587-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="4a587-142">Vous pouvez conserver ce nom.</span><span class="sxs-lookup"><span data-stu-id="4a587-142">You can keep this name.</span></span>

![créer un projet de test unitaire](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="4a587-144">Après avoir créé l’application, vous verrez qu’il contient deux projets.</span><span class="sxs-lookup"><span data-stu-id="4a587-144">After creating the application, you will see it contains two projects.</span></span>

![deux projets](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="4a587-146">Ajouter un projet de test unitaire à une application existante</span><span class="sxs-lookup"><span data-stu-id="4a587-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="4a587-147">Si vous n’avez pas créé le projet de test unitaire quand vous avez créé votre application, vous pouvez ajouter un à tout moment.</span><span class="sxs-lookup"><span data-stu-id="4a587-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="4a587-148">Par exemple, supposons que vous disposez déjà d’une application nommée StoreApp, et que vous souhaitez ajouter des tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="4a587-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="4a587-149">Pour ajouter un projet de test unitaire, avec le bouton droit de votre solution et sélectionnez **ajouter** et **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="4a587-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Ajouter un nouveau projet à la solution](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="4a587-151">Sélectionnez **Test** dans le volet gauche, sélectionnez **projet de Test unitaire** pour le type de projet.</span><span class="sxs-lookup"><span data-stu-id="4a587-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="4a587-152">Nommez le projet **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="4a587-152">Name the project **StoreApp.Tests**.</span></span>

![ajouter le projet de test unitaire](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="4a587-154">Vous verrez le projet de test unitaire dans votre solution.</span><span class="sxs-lookup"><span data-stu-id="4a587-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="4a587-155">Dans le projet de test unitaire, ajoutez une référence de projet au projet d’origine.</span><span class="sxs-lookup"><span data-stu-id="4a587-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="4a587-156">Configurer l’application Web API 2</span><span class="sxs-lookup"><span data-stu-id="4a587-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="4a587-157">Dans votre projet StoreApp, ajoutez un fichier de classe dans le **modèles** dossier nommé **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="4a587-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="4a587-158">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="4a587-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="4a587-159">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="4a587-159">Build the solution.</span></span>

<span data-ttu-id="4a587-160">Cliquez sur le dossier Controllers, puis sélectionnez **ajouter** et **un nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="4a587-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="4a587-161">Sélectionnez **vide de l’API Web 2 contrôleur -**.</span><span class="sxs-lookup"><span data-stu-id="4a587-161">Select **Web API 2 Controller - Empty**.</span></span>

![Ajouter un nouveau contrôleur](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="4a587-163">Définissez le nom du contrôleur sur **SimpleProductController**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="4a587-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![Spécifiez le contrôleur](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="4a587-165">Remplacez le code existant par le code ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4a587-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="4a587-166">Pour simplifier cet exemple, les données sont stockées dans une liste au lieu d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="4a587-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="4a587-167">La liste définie dans cette classe représente les données de production.</span><span class="sxs-lookup"><span data-stu-id="4a587-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="4a587-168">Notez que le contrôleur inclut un constructeur qui prend comme paramètre une liste d’objets de produit.</span><span class="sxs-lookup"><span data-stu-id="4a587-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="4a587-169">Ce constructeur vous permet de passer des données de test lors de tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="4a587-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="4a587-170">Le contrôleur inclut également deux **async** méthodes pour illustrer les méthodes asynchrones de tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="4a587-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="4a587-171">Ces méthodes asynchrones ont été implémentées en appelant **Task.FromResult** afin de réduire le code superflu, mais normalement les méthodes inclurait opérations gourmandes en ressources.</span><span class="sxs-lookup"><span data-stu-id="4a587-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="4a587-172">La méthode GetProduct retourne une instance de la **IHttpActionResult** interface.</span><span class="sxs-lookup"><span data-stu-id="4a587-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="4a587-173">IHttpActionResult est une des nouvelles fonctionnalités de l’API Web 2, et il simplifie le développement de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="4a587-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="4a587-174">Les classes qui implémentent l’interface IHttpActionResult se trouvent dans le [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) espace de noms.</span><span class="sxs-lookup"><span data-stu-id="4a587-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="4a587-175">Ces classes représentent des réponses possibles à partir d’une demande d’action, et elles correspondent aux codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="4a587-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="4a587-176">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="4a587-176">Build the solution.</span></span>

<span data-ttu-id="4a587-177">Vous êtes maintenant prêt à configurer le projet de test.</span><span class="sxs-lookup"><span data-stu-id="4a587-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="4a587-178">Installer les packages NuGet dans le projet de test</span><span class="sxs-lookup"><span data-stu-id="4a587-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="4a587-179">Lorsque vous utilisez le modèle vide pour créer une application, le projet de test unitaire (StoreApp.Tests) n’inclut pas tous les packages NuGet installées.</span><span class="sxs-lookup"><span data-stu-id="4a587-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="4a587-180">Autres modèles, par exemple, le modèle de l’API Web, incluent des packages NuGet dans le projet de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="4a587-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="4a587-181">Pour ce didacticiel, vous devez inclure le package Microsoft ASP.NET Web API 2 Core au projet de test.</span><span class="sxs-lookup"><span data-stu-id="4a587-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="4a587-182">Cliquez sur le projet StoreApp.Tests et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4a587-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="4a587-183">Vous devez sélectionner le projet StoreApp.Tests pour ajouter les packages à ce projet.</span><span class="sxs-lookup"><span data-stu-id="4a587-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![gérer les packages](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="4a587-185">Rechercher et installer Microsoft ASP.NET Web API 2 Core package.</span><span class="sxs-lookup"><span data-stu-id="4a587-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![installer le package de base des api web](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="4a587-187">Fermez la fenêtre Gérer les Packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="4a587-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="4a587-188">Créer des tests</span><span class="sxs-lookup"><span data-stu-id="4a587-188">Create tests</span></span>

<span data-ttu-id="4a587-189">Par défaut, votre projet de test inclut un fichier de test vide nommé UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="4a587-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="4a587-190">Ce fichier montre les attributs que vous permet de créer des méthodes de test.</span><span class="sxs-lookup"><span data-stu-id="4a587-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="4a587-191">Pour vos tests unitaires, vous pouvez utiliser ce fichier ou créer votre propre fichier.</span><span class="sxs-lookup"><span data-stu-id="4a587-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="4a587-193">Pour ce didacticiel, vous allez créer votre propre classe de test.</span><span class="sxs-lookup"><span data-stu-id="4a587-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="4a587-194">Vous pouvez supprimer le fichier UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="4a587-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="4a587-195">Ajoutez une classe nommée **TestSimpleProductController.cs**et remplacez le code par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="4a587-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="4a587-196">Exécuter les tests</span><span class="sxs-lookup"><span data-stu-id="4a587-196">Run tests</span></span>

<span data-ttu-id="4a587-197">Vous êtes maintenant prêt à exécuter les tests.</span><span class="sxs-lookup"><span data-stu-id="4a587-197">You are now ready to run the tests.</span></span> <span data-ttu-id="4a587-198">Tous les de la méthode qui sont marqués avec le **TestMethod** attribut sera testé.</span><span class="sxs-lookup"><span data-stu-id="4a587-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="4a587-199">À partir de la **Test** élément de menu, exécutez les tests.</span><span class="sxs-lookup"><span data-stu-id="4a587-199">From the **Test** menu item, run the tests.</span></span>

![exécuter des tests](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="4a587-201">Ouvrez le **l’Explorateur de tests** fenêtre et notez les résultats des tests.</span><span class="sxs-lookup"><span data-stu-id="4a587-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![résultats des tests](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="4a587-203">Résumé</span><span class="sxs-lookup"><span data-stu-id="4a587-203">Summary</span></span>

<span data-ttu-id="4a587-204">Vous avez terminé ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="4a587-204">You have completed this tutorial.</span></span> <span data-ttu-id="4a587-205">Les données dans ce didacticiel a été intentionnellement simplifiées pour vous concentrer sur les conditions de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="4a587-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="4a587-206">Pour des scénarios plus avancés de données de tests unitaires, consultez [simulation Entity Framework lorsque unité test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="4a587-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
