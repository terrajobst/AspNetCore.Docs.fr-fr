---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulation d’Entity Framework lors de l’API Web ASP.NET 2 de tests unitaires | Microsoft Docs
author: tfitzmac
description: Ce guide et l’application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise Entity Framework. Il montre comment modifier le...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8945f913abe8fb8397d07a5994000fff2348f1f7
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795377"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="d5330-104">Simulation d’Entity Framework lors de l’API Web ASP.NET 2 de tests unitaires</span><span class="sxs-lookup"><span data-stu-id="d5330-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="d5330-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d5330-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="d5330-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="d5330-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="d5330-107">Ce guide et l’application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d5330-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="d5330-108">Il montre comment modifier le contrôleur généré automatiquement pour activer la transmission d’un objet de contexte pour le test et comment créer des objets de test qui fonctionnent avec Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d5330-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="d5330-109">Pour une introduction aux tests unitaires avec l’API Web ASP.NET, consultez [Unit Testing avec ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d5330-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="d5330-110">Ce didacticiel suppose que vous êtes familiarisé avec les concepts de base de l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d5330-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="d5330-111">Pour un didacticiel d’introduction, consultez [mise en route avec ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d5330-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d5330-112">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="d5330-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="d5330-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="d5330-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="d5330-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="d5330-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="d5330-115">Dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="d5330-115">In this topic</span></span>

<span data-ttu-id="d5330-116">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="d5330-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d5330-117">Composants requis</span><span class="sxs-lookup"><span data-stu-id="d5330-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="d5330-118">Télécharger le code</span><span class="sxs-lookup"><span data-stu-id="d5330-118">Download code</span></span>](#download)
- [<span data-ttu-id="d5330-119">Créer des applications avec le projet de test unitaire</span><span class="sxs-lookup"><span data-stu-id="d5330-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="d5330-120">Créer la classe de modèle</span><span class="sxs-lookup"><span data-stu-id="d5330-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="d5330-121">Ajouter le contrôleur</span><span class="sxs-lookup"><span data-stu-id="d5330-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="d5330-122">Ajouter l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="d5330-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="d5330-123">Installer les packages NuGet dans le projet de test</span><span class="sxs-lookup"><span data-stu-id="d5330-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="d5330-124">Créer le contexte de test</span><span class="sxs-lookup"><span data-stu-id="d5330-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="d5330-125">Créer des tests</span><span class="sxs-lookup"><span data-stu-id="d5330-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="d5330-126">Exécuter des tests</span><span class="sxs-lookup"><span data-stu-id="d5330-126">Run tests</span></span>](#runtests)

<span data-ttu-id="d5330-127">Si vous avez déjà effectué les étapes décrites dans [Unit Testing avec ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), vous pouvez passer à la section [ajouter le contrôleur de](#controller).</span><span class="sxs-lookup"><span data-stu-id="d5330-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="d5330-128">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d5330-128">Prerequisites</span></span>

<span data-ttu-id="d5330-129">Édition Visual de Studio 2017 Community, Professional ou Enterprise</span><span class="sxs-lookup"><span data-stu-id="d5330-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="d5330-130">Télécharger le code</span><span class="sxs-lookup"><span data-stu-id="d5330-130">Download code</span></span>

<span data-ttu-id="d5330-131">Téléchargez le [projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="d5330-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="d5330-132">Le projet téléchargeable inclut le code de test unitaire pour cette rubrique et pour le [Unit Test API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) rubrique.</span><span class="sxs-lookup"><span data-stu-id="d5330-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="d5330-133">Créer des applications avec le projet de test unitaire</span><span class="sxs-lookup"><span data-stu-id="d5330-133">Create application with unit test project</span></span>

<span data-ttu-id="d5330-134">Vous pouvez créer un projet de test unitaire lors de la création de votre application ou ajouter un projet de test unitaire à une application existante.</span><span class="sxs-lookup"><span data-stu-id="d5330-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="d5330-135">Ce didacticiel montre la création d’un projet de test unitaire lors de la création de l’application.</span><span class="sxs-lookup"><span data-stu-id="d5330-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="d5330-136">Créer une Application Web ASP.NET nommé **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="d5330-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="d5330-137">Dans la fenêtre Nouveau projet ASP.NET, sélectionnez le **vide** modèle et ajouter des dossiers et les références principales pour l’API Web.</span><span class="sxs-lookup"><span data-stu-id="d5330-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="d5330-138">Sélectionnez le **ajouter des tests unitaires** option.</span><span class="sxs-lookup"><span data-stu-id="d5330-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="d5330-139">Le projet de test unitaire est automatiquement nommé **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="d5330-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="d5330-140">Vous pouvez conserver ce nom.</span><span class="sxs-lookup"><span data-stu-id="d5330-140">You can keep this name.</span></span>

![créer le projet de test unitaire](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="d5330-142">Après avoir créé l’application, vous constaterez qu’il contient deux projets : **StoreApp** et **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="d5330-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="d5330-143">Créer la classe de modèle</span><span class="sxs-lookup"><span data-stu-id="d5330-143">Create the model class</span></span>

<span data-ttu-id="d5330-144">Dans votre projet StoreApp, ajoutez un fichier de classe pour le **modèles** dossier nommé **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="d5330-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="d5330-145">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="d5330-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="d5330-146">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="d5330-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="d5330-147">Ajouter le contrôleur</span><span class="sxs-lookup"><span data-stu-id="d5330-147">Add the controller</span></span>

<span data-ttu-id="d5330-148">Cliquez sur le dossier contrôleurs et sélectionnez **ajouter** et **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="d5330-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="d5330-149">Sélectionnez le contrôleur Web API 2 avec actions, à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d5330-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Ajouter un nouveau contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="d5330-151">Définissez les valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="d5330-151">Set the following values:</span></span>

- <span data-ttu-id="d5330-152">Nom du contrôleur : **ProductController**</span><span class="sxs-lookup"><span data-stu-id="d5330-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="d5330-153">Classe de modèle : **produit**</span><span class="sxs-lookup"><span data-stu-id="d5330-153">Model class: **Product**</span></span>
- <span data-ttu-id="d5330-154">Classe de contexte de données : [Sélectionnez **nouveau contexte de données** bouton qui remplit les valeurs indiqués ci-dessous]</span><span class="sxs-lookup"><span data-stu-id="d5330-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![spécifier le contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="d5330-156">Cliquez sur **ajouter** pour créer le contrôleur avec le code généré automatiquement.</span><span class="sxs-lookup"><span data-stu-id="d5330-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="d5330-157">Le code inclut des méthodes pour créer, récupérer, la mise à jour et suppression d’instances de la classe Product.</span><span class="sxs-lookup"><span data-stu-id="d5330-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="d5330-158">Le code suivant illustre la méthode pour ajouter un produit.</span><span class="sxs-lookup"><span data-stu-id="d5330-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="d5330-159">Notez que la méthode retourne une instance de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="d5330-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="d5330-160">IHttpActionResult est une des nouvelles fonctionnalités dans Web API 2, et il simplifie le développement de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="d5330-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="d5330-161">Dans la section suivante, vous personnaliserez le code généré pour faciliter le passage d’objets de test au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d5330-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="d5330-162">Ajouter l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="d5330-162">Add dependency injection</span></span>

<span data-ttu-id="d5330-163">Actuellement, la classe ProductController est codé en dur pour utiliser une instance de la classe StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="d5330-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="d5330-164">Vous allez utiliser un modèle appelé l’injection de dépendances pour modifier votre application et de supprimer cette dépendance codée en dur.</span><span class="sxs-lookup"><span data-stu-id="d5330-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="d5330-165">En divisant cette dépendance, vous pouvez passer dans un objet factice lors du test.</span><span class="sxs-lookup"><span data-stu-id="d5330-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="d5330-166">Cliquez sur le **modèles** dossier et ajoutez une nouvelle interface nommée **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="d5330-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="d5330-167">Remplacez le code par celui-ci :</span><span class="sxs-lookup"><span data-stu-id="d5330-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="d5330-168">Ouvrez le fichier StoreAppContext.cs et apportez les modifications suivantes en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="d5330-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="d5330-169">Les modifications importantes à noter sont :</span><span class="sxs-lookup"><span data-stu-id="d5330-169">The important changes to note are:</span></span>

- <span data-ttu-id="d5330-170">StoreAppContext classe implémente l’interface de IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="d5330-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="d5330-171">MarkAsModified méthode est implémentée.</span><span class="sxs-lookup"><span data-stu-id="d5330-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="d5330-172">Ouvrez le fichier ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="d5330-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="d5330-173">Modifier le code existant pour faire correspondre le code en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="d5330-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="d5330-174">Ces modifications rompre la dépendance sur StoreAppContext et permettent à d’autres classes passer un objet différent pour la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="d5330-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="d5330-175">Cette modification vous permettra de transmettre un contexte de test au cours des tests unitaires.</span><span class="sxs-lookup"><span data-stu-id="d5330-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="d5330-176">Il existe une modification plus, que vous devez apporter dans ProductController.</span><span class="sxs-lookup"><span data-stu-id="d5330-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="d5330-177">Dans le **PutProduct** (méthode), remplacez la ligne qui définit l’état d’entité à modifié par un appel à la méthode MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="d5330-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="d5330-178">Générez la solution.</span><span class="sxs-lookup"><span data-stu-id="d5330-178">Build the solution.</span></span>

<span data-ttu-id="d5330-179">Vous êtes maintenant prêt à configurer le projet de test.</span><span class="sxs-lookup"><span data-stu-id="d5330-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="d5330-180">Installer les packages NuGet dans le projet de test</span><span class="sxs-lookup"><span data-stu-id="d5330-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="d5330-181">Lorsque vous utilisez le modèle vide pour créer une application, le projet de test unitaire (StoreApp.Tests) n’inclut pas tous les packages NuGet installés.</span><span class="sxs-lookup"><span data-stu-id="d5330-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="d5330-182">Autres modèles, tels que le modèle API Web, incluent certains packages NuGet dans le projet de test unitaire.</span><span class="sxs-lookup"><span data-stu-id="d5330-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="d5330-183">Pour ce didacticiel, vous devez inclure le package d’Entity Framework et le package de Microsoft ASP.NET Web API 2 Core au projet de test.</span><span class="sxs-lookup"><span data-stu-id="d5330-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="d5330-184">Cliquez sur le projet StoreApp.Tests et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d5330-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="d5330-185">Vous devez sélectionner le projet StoreApp.Tests pour ajouter les packages à ce projet.</span><span class="sxs-lookup"><span data-stu-id="d5330-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![gérer les packages](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="d5330-187">À partir des packages en ligne, rechercher et installer le package EntityFramework (version 6.0 ou version ultérieure).</span><span class="sxs-lookup"><span data-stu-id="d5330-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="d5330-188">S’il apparaît que le package EntityFramework est déjà installé, il pouvez que vous avez sélectionné le projet StoreApp plutôt qu’au projet StoreApp.Tests.</span><span class="sxs-lookup"><span data-stu-id="d5330-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Ajouter Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="d5330-190">Rechercher et installer le package Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="d5330-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![installer le package de core api web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="d5330-192">Fermez la fenêtre Gérer les Packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="d5330-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="d5330-193">Créer le contexte de test</span><span class="sxs-lookup"><span data-stu-id="d5330-193">Create test context</span></span>

<span data-ttu-id="d5330-194">Ajoutez une classe nommée **TestDbSet** au projet de test.</span><span class="sxs-lookup"><span data-stu-id="d5330-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="d5330-195">Cette classe sert de classe de base pour votre jeu de données de test.</span><span class="sxs-lookup"><span data-stu-id="d5330-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="d5330-196">Remplacez le code par celui-ci :</span><span class="sxs-lookup"><span data-stu-id="d5330-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="d5330-197">Ajoutez une classe nommée **TestProductDbSet** au projet de test qui contient le code suivant.</span><span class="sxs-lookup"><span data-stu-id="d5330-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="d5330-198">Ajoutez une classe nommée **TestStoreAppContext** et remplacez le code existant par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="d5330-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="d5330-199">Créer des tests</span><span class="sxs-lookup"><span data-stu-id="d5330-199">Create tests</span></span>

<span data-ttu-id="d5330-200">Par défaut, votre projet de test inclut un fichier de test vide nommé **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="d5330-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="d5330-201">Ce fichier montre les attributs que vous permet de créer des méthodes de test.</span><span class="sxs-lookup"><span data-stu-id="d5330-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="d5330-202">Pour ce didacticiel, vous pouvez supprimer ce fichier, car vous allez ajouter une nouvelle classe de test.</span><span class="sxs-lookup"><span data-stu-id="d5330-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="d5330-203">Ajoutez une classe nommée **TestProductController** au projet de test.</span><span class="sxs-lookup"><span data-stu-id="d5330-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="d5330-204">Remplacez le code par celui-ci :</span><span class="sxs-lookup"><span data-stu-id="d5330-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="d5330-205">Exécuter les tests</span><span class="sxs-lookup"><span data-stu-id="d5330-205">Run tests</span></span>

<span data-ttu-id="d5330-206">Vous êtes maintenant prêt à exécuter les tests.</span><span class="sxs-lookup"><span data-stu-id="d5330-206">You are now ready to run the tests.</span></span> <span data-ttu-id="d5330-207">Tous de la méthode qui sont marqués avec le **TestMethod** attribut sera testé.</span><span class="sxs-lookup"><span data-stu-id="d5330-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="d5330-208">À partir de la **Test** élément de menu, exécuter les tests.</span><span class="sxs-lookup"><span data-stu-id="d5330-208">From the **Test** menu item, run the tests.</span></span>

![exécuter des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="d5330-210">Ouvrez le **Explorateur de tests** fenêtre et notez les résultats des tests.</span><span class="sxs-lookup"><span data-stu-id="d5330-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![résultats des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
