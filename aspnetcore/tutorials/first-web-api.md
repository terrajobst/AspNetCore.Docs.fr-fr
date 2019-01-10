---
title: 'Tutoriel : Créer une API web avec ASP.NET Core MVC'
author: rick-anderson
description: Générer une API web avec ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: tutorials/first-web-api
ms.openlocfilehash: c2b4dcddd5332330cd6e6abe7d3a12697cde845e
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53382002"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="0a527-103">Tutoriel : Créer une API web avec ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="0a527-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="0a527-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="0a527-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="0a527-105">Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a527-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="0a527-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="0a527-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0a527-107">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="0a527-107">Create a web API project.</span></span>
> * <span data-ttu-id="0a527-108">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="0a527-108">Add a model class.</span></span>
> * <span data-ttu-id="0a527-109">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0a527-109">Create the database context.</span></span>
> * <span data-ttu-id="0a527-110">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0a527-110">Register the database context.</span></span>
> * <span data-ttu-id="0a527-111">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="0a527-111">Add a controller.</span></span>
> * <span data-ttu-id="0a527-112">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="0a527-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="0a527-113">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="0a527-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="0a527-114">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="0a527-114">Specify return values.</span></span>
> * <span data-ttu-id="0a527-115">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="0a527-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="0a527-116">Appeler l’API web avec jQuery.</span><span class="sxs-lookup"><span data-stu-id="0a527-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="0a527-117">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="0a527-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="0a527-118">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="0a527-118">Overview</span></span>

<span data-ttu-id="0a527-119">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="0a527-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="0a527-120">API</span><span class="sxs-lookup"><span data-stu-id="0a527-120">API</span></span> | <span data-ttu-id="0a527-121">Description</span><span class="sxs-lookup"><span data-stu-id="0a527-121">Description</span></span> | <span data-ttu-id="0a527-122">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="0a527-122">Request body</span></span> | <span data-ttu-id="0a527-123">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="0a527-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="0a527-124">GET /api/todo</span><span class="sxs-lookup"><span data-stu-id="0a527-124">GET /api/todo</span></span> | <span data-ttu-id="0a527-125">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="0a527-125">Get all to-do items</span></span> | <span data-ttu-id="0a527-126">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0a527-126">None</span></span> | <span data-ttu-id="0a527-127">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="0a527-127">Array of to-do items</span></span>|
|<span data-ttu-id="0a527-128">GET /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="0a527-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="0a527-129">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="0a527-129">Get an item by ID</span></span> | <span data-ttu-id="0a527-130">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0a527-130">None</span></span> | <span data-ttu-id="0a527-131">Tâche</span><span class="sxs-lookup"><span data-stu-id="0a527-131">To-do item</span></span>|
|<span data-ttu-id="0a527-132">POST /api/todo</span><span class="sxs-lookup"><span data-stu-id="0a527-132">POST /api/todo</span></span> | <span data-ttu-id="0a527-133">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="0a527-133">Add a new item</span></span> | <span data-ttu-id="0a527-134">Tâche</span><span class="sxs-lookup"><span data-stu-id="0a527-134">To-do item</span></span> | <span data-ttu-id="0a527-135">Tâche</span><span class="sxs-lookup"><span data-stu-id="0a527-135">To-do item</span></span> |
|<span data-ttu-id="0a527-136">PUT /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="0a527-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="0a527-137">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0a527-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="0a527-138">Tâche</span><span class="sxs-lookup"><span data-stu-id="0a527-138">To-do item</span></span> | <span data-ttu-id="0a527-139">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0a527-139">None</span></span> |
|<span data-ttu-id="0a527-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0a527-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="0a527-141">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="0a527-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="0a527-142">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0a527-142">None</span></span> | <span data-ttu-id="0a527-143">Aucun.</span><span class="sxs-lookup"><span data-stu-id="0a527-143">None</span></span>|

<span data-ttu-id="0a527-144">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="0a527-144">The following diagram shows the design of the app.</span></span>

![Le client, représenté par une zone située à gauche, soumet une requête et reçoit une réponse de l’application, représentée par une zone dessinée sur la droite.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="0a527-149">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="0a527-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a527-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a527-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a527-151">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="0a527-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0a527-152">Sélectionnez le modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0a527-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0a527-153">Nommez le projet *TodoApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="0a527-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="0a527-154">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, choisissez la version ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a527-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="0a527-155">Sélectionnez le modèle **API** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="0a527-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="0a527-156">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="0a527-156">Do **not** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a527-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a527-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a527-159">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="0a527-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="0a527-160">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="0a527-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="0a527-161">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0a527-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="0a527-162">Ces commandes créent un projet d’API web et ouvrent une nouvelle instance de Visual Studio Code dans le nouveau dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="0a527-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="0a527-163">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="0a527-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a527-164">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0a527-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a527-165">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="0a527-165">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="0a527-167">Sélectionnez **Application .NET Core** > **API web ASP.NET Core** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="0a527-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="0a527-169">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut \**.NET Core 2.2* pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="0a527-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="0a527-170">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="0a527-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="0a527-172">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="0a527-172">Test the API</span></span>

<span data-ttu-id="0a527-173">Le modèle de projet crée une API `values`.</span><span class="sxs-lookup"><span data-stu-id="0a527-173">The project template creates a `values` API.</span></span> <span data-ttu-id="0a527-174">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="0a527-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a527-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a527-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a527-176">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="0a527-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="0a527-177">Visual Studio lance un navigateur et accède à `https://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="0a527-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="0a527-178">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="0a527-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="0a527-179">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="0a527-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a527-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a527-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0a527-181">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="0a527-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="0a527-182">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="0a527-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a527-183">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0a527-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0a527-184">Sélectionnez **Exécuter** > **Démarrer avec débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="0a527-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="0a527-185">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="0a527-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="0a527-186">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="0a527-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="0a527-187">Ajoutez `/api/values` à l’URL (définissez-la sur `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="0a527-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="0a527-188">Le code JSON suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="0a527-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="0a527-189">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="0a527-189">Add a model class</span></span>

<span data-ttu-id="0a527-190">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="0a527-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="0a527-191">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="0a527-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a527-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a527-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a527-193">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="0a527-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="0a527-194">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="0a527-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0a527-195">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="0a527-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="0a527-196">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0a527-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0a527-197">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="0a527-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="0a527-198">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a527-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a527-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a527-200">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="0a527-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="0a527-201">Ajoutez une classe `TodoItem` au dossier *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a527-202">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0a527-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a527-203">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="0a527-203">Right-click the project.</span></span> <span data-ttu-id="0a527-204">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="0a527-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="0a527-205">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="0a527-205">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="0a527-207">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="0a527-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="0a527-208">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="0a527-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="0a527-209">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="0a527-210">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="0a527-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="0a527-211">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="0a527-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="0a527-212">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0a527-212">Add a database context</span></span>

<span data-ttu-id="0a527-213">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="0a527-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="0a527-214">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="0a527-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a527-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a527-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a527-216">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="0a527-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="0a527-217">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="0a527-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a527-218">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a527-218">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a527-219">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="0a527-219">Add a `TodoContext` class to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a527-220">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0a527-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a527-221">Ajoutez une classe `TodoContext` dans le dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="0a527-221">Add a `TodoContext` class in the *Models* folder:</span></span>

---

* <span data-ttu-id="0a527-222">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-222">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="0a527-223">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0a527-223">Register the database context</span></span>

<span data-ttu-id="0a527-224">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0a527-224">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0a527-225">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="0a527-225">The container provides the service to controllers.</span></span>

<span data-ttu-id="0a527-226">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-226">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="0a527-227">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="0a527-227">The preceding code:</span></span>

* <span data-ttu-id="0a527-228">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="0a527-228">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="0a527-229">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="0a527-229">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="0a527-230">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="0a527-230">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="0a527-231">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="0a527-231">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a527-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a527-232">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a527-233">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="0a527-233">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="0a527-234">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="0a527-234">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="0a527-235">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="0a527-235">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="0a527-236">Nommez la classe *TodoController* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="0a527-236">Name the class *TodoController*, and select **Add**.</span></span>

  ![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0a527-238">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0a527-238">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0a527-239">Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="0a527-239">In the *Controllers* folder, create a class named `TodoController`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0a527-240">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="0a527-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0a527-241">Dans le dossier *Contrôleurs*, ajoutez la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="0a527-241">In the *Controllers* folder, add the class `TodoController`.</span></span>

---

* <span data-ttu-id="0a527-242">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-242">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="0a527-243">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="0a527-243">The preceding code:</span></span>

* <span data-ttu-id="0a527-244">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="0a527-244">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="0a527-245">Décorez la classe avec l’attribut [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="0a527-245">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="0a527-246">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="0a527-246">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="0a527-247">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez [Annotation avec attribut ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="0a527-247">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="0a527-248">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0a527-248">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="0a527-249">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0a527-249">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="0a527-250">Ajoute un élément nommé `Item1` à la base de données si celle-ci est vide.</span><span class="sxs-lookup"><span data-stu-id="0a527-250">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="0a527-251">Ce code se trouvant dans le constructeur, il s’exécute chaque fois qu’une nouvelle requête HTTP existe.</span><span class="sxs-lookup"><span data-stu-id="0a527-251">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="0a527-252">Si vous supprimez tous les éléments, le constructeur recrée `Item1` au prochain appel d’une méthode d’API.</span><span class="sxs-lookup"><span data-stu-id="0a527-252">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="0a527-253">Ainsi, il peut vous sembler à tort que la suppression n’a pas fonctionné.</span><span class="sxs-lookup"><span data-stu-id="0a527-253">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="0a527-254">Ajouter des méthodes Get</span><span class="sxs-lookup"><span data-stu-id="0a527-254">Add Get methods</span></span>

<span data-ttu-id="0a527-255">Pour fournir une API qui récupère les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="0a527-255">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="0a527-256">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="0a527-256">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="0a527-257">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="0a527-257">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="0a527-258">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="0a527-258">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="0a527-259">La réponse HTTP suivante est générée par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="0a527-259">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="0a527-260">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="0a527-260">Routing and URL paths</span></span>

<span data-ttu-id="0a527-261">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0a527-261">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="0a527-262">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="0a527-262">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="0a527-263">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="0a527-263">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="0a527-264">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="0a527-264">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="0a527-265">Pour cet exemple, le nom de classe du contrôleur étant **Todo**Controller, le nom du contrôleur est « todo ».</span><span class="sxs-lookup"><span data-stu-id="0a527-265">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="0a527-266">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="0a527-266">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="0a527-267">Si l’attribut `[HttpGet]` a un modèle de route (par exemple`[HttpGet("/products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="0a527-267">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="0a527-268">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="0a527-268">This sample doesn't use a template.</span></span> <span data-ttu-id="0a527-269">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="0a527-269">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="0a527-270">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="0a527-270">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="0a527-271">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="0a527-271">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="0a527-272">Le paramètre `Name = "GetTodo"` crée une route nommée.</span><span class="sxs-lookup"><span data-stu-id="0a527-272">The `Name = "GetTodo"` parameter creates a named route.</span></span> <span data-ttu-id="0a527-273">Vous verrez plus tard comment l’application peut utiliser le nom de la route pour créer un lien HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a527-273">You'll see later how the app can use the name to create an HTTP link using the route name.</span></span>

## <a name="return-values"></a><span data-ttu-id="0a527-274">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="0a527-274">Return values</span></span>

<span data-ttu-id="0a527-275">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="0a527-275">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="0a527-276">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="0a527-276">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="0a527-277">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="0a527-277">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="0a527-278">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="0a527-278">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="0a527-279">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a527-279">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="0a527-280">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="0a527-280">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="0a527-281">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="0a527-281">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="0a527-282">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="0a527-282">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="0a527-283">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="0a527-283">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="0a527-284">Tester la méthode GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="0a527-284">Test the GetTodoItems method</span></span>

<span data-ttu-id="0a527-285">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="0a527-285">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="0a527-286">Installez [Postman](https://www.getpostman.com/apps).</span><span class="sxs-lookup"><span data-stu-id="0a527-286">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="0a527-287">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="0a527-287">Start the web app.</span></span>
* <span data-ttu-id="0a527-288">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="0a527-288">Start Postman.</span></span>
* <span data-ttu-id="0a527-289">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="0a527-289">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="0a527-290">À partir de **Fichier > Paramètres** (onglet \**Général*), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="0a527-290">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="0a527-291">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0a527-291">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="0a527-292">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="0a527-292">Create a new request.</span></span>
  * <span data-ttu-id="0a527-293">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="0a527-293">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="0a527-294">Définissez l’URL de la requête sur `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0a527-294">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="0a527-295">Par exemple, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="0a527-295">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="0a527-296">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="0a527-296">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="0a527-297">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="0a527-297">Select **Send**.</span></span>

![Postman avec requête Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="0a527-299">Ajouter une méthode Create</span><span class="sxs-lookup"><span data-stu-id="0a527-299">Add a Create method</span></span>

<span data-ttu-id="0a527-300">Ajoutez la méthode `PostTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="0a527-300">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="0a527-301">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="0a527-301">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="0a527-302">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a527-302">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="0a527-303">La méthode `CreatedAtRoute` :</span><span class="sxs-lookup"><span data-stu-id="0a527-303">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="0a527-304">Retourne une réponse 201.</span><span class="sxs-lookup"><span data-stu-id="0a527-304">Returns a 201 response.</span></span> <span data-ttu-id="0a527-305">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="0a527-305">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="0a527-306">Ajoute un en-tête Location à la réponse.</span><span class="sxs-lookup"><span data-stu-id="0a527-306">Adds a Location header to the response.</span></span> <span data-ttu-id="0a527-307">L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="0a527-307">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="0a527-308">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="0a527-308">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="0a527-309">Utilise l’itinéraire nommé « GetTodo » pour créer l’URL.</span><span class="sxs-lookup"><span data-stu-id="0a527-309">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="0a527-310">L’itinéraire nommé « GetTodo » est défini dans `GetTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="0a527-310">The "GetTodo" named route is defined in `GetTodoItem`:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="0a527-311">Tester la méthode PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="0a527-311">Test the PostTodoItem method</span></span>

* <span data-ttu-id="0a527-312">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="0a527-312">Build the project.</span></span>
* <span data-ttu-id="0a527-313">Dans Postman, définissez la méthode HTTP sur `POST`.</span><span class="sxs-lookup"><span data-stu-id="0a527-313">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="0a527-314">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="0a527-314">Select the **Body** tab.</span></span>
* <span data-ttu-id="0a527-315">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="0a527-315">Select the **raw** radio button.</span></span>
* <span data-ttu-id="0a527-316">Définissez le type sur **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="0a527-316">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="0a527-317">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="0a527-317">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="0a527-318">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="0a527-318">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/create.png)

  <span data-ttu-id="0a527-320">Si vous obtenez une erreur 405 Méthode non autorisée, il est probable que le projet n’ait pas été compilé après l’ajout de la méthode `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="0a527-320">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="0a527-321">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="0a527-321">Test the location header URI</span></span>

* <span data-ttu-id="0a527-322">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="0a527-322">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="0a527-323">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="0a527-323">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="0a527-325">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="0a527-325">Set the method to GET.</span></span>
* <span data-ttu-id="0a527-326">Collez l’URI (par exemple, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="0a527-326">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="0a527-327">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="0a527-327">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="0a527-328">Ajouter une méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="0a527-328">Add a PutTodoItem method</span></span>

<span data-ttu-id="0a527-329">Ajoutez la méthode `PutTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="0a527-329">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="0a527-330">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0a527-330">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="0a527-331">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0a527-331">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="0a527-332">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="0a527-332">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="0a527-333">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="0a527-333">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="0a527-334">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="0a527-334">Test the PutTodoItem method</span></span>

<span data-ttu-id="0a527-335">Mettez à jour la tâche dont l’id est 1 en définissant son nom sur « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="0a527-335">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="0a527-336">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="0a527-336">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="0a527-338">Ajouter une méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="0a527-338">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="0a527-339">Ajoutez la méthode `DeleteTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="0a527-339">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="0a527-340">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="0a527-340">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="0a527-341">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="0a527-341">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="0a527-342">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="0a527-342">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="0a527-343">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="0a527-343">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="0a527-344">Définissez l’URI de l’objet à supprimer, par exemple `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="0a527-344">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="0a527-345">Sélectionnez **Send**.</span><span class="sxs-lookup"><span data-stu-id="0a527-345">Select **Send**</span></span>

<span data-ttu-id="0a527-346">L’exemple d’application vous permet de supprimer tous les éléments, mais quand le dernier élément est supprimé, un nouveau est créé par le constructeur de classe de modèle au prochain appel de l’API.</span><span class="sxs-lookup"><span data-stu-id="0a527-346">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="0a527-347">Appeler l’API avec jQuery</span><span class="sxs-lookup"><span data-stu-id="0a527-347">Call the API with jQuery</span></span>

<span data-ttu-id="0a527-348">Dans cette section, une page HTML qui utilise jQuery pour appeler l’API web est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="0a527-348">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="0a527-349">jQuery lance la requête et met à jour la page avec les détails de la réponse de l’API.</span><span class="sxs-lookup"><span data-stu-id="0a527-349">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="0a527-350">Configurez l’application pour [traiter les fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [activer le mappage de fichier par défaut](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) :</span><span class="sxs-lookup"><span data-stu-id="0a527-350">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="0a527-351">Créez un dossier *wwwroot* dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="0a527-351">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="0a527-352">Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0a527-352">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="0a527-353">Remplacez son contenu par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-353">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="0a527-354">Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="0a527-354">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="0a527-355">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a527-355">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="0a527-356">Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :</span><span class="sxs-lookup"><span data-stu-id="0a527-356">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="0a527-357">Ouvrez *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0a527-357">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="0a527-358">Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.</span><span class="sxs-lookup"><span data-stu-id="0a527-358">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="0a527-359">Il existe plusieurs façons d’obtenir jQuery.</span><span class="sxs-lookup"><span data-stu-id="0a527-359">There are several ways to get jQuery.</span></span> <span data-ttu-id="0a527-360">Dans l’extrait précédent, la bibliothèque est chargée à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="0a527-360">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="0a527-361">Cet exemple appelle toutes les méthodes CRUD de l’API.</span><span class="sxs-lookup"><span data-stu-id="0a527-361">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="0a527-362">Les explications suivantes traitent des appels à l’API.</span><span class="sxs-lookup"><span data-stu-id="0a527-362">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="0a527-363">Obtenir une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="0a527-363">Get a list of to-do items</span></span>

<span data-ttu-id="0a527-364">La fonction JQuery [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `GET` à l’API, qui retourne du code JSON représentant un tableau de tâches.</span><span class="sxs-lookup"><span data-stu-id="0a527-364">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="0a527-365">La fonction de rappel `success` est appelée si la requête réussit.</span><span class="sxs-lookup"><span data-stu-id="0a527-365">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="0a527-366">Dans le rappel, le DOM est mis à jour avec les informations des tâches.</span><span class="sxs-lookup"><span data-stu-id="0a527-366">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="0a527-367">Ajouter une tâche</span><span class="sxs-lookup"><span data-stu-id="0a527-367">Add a to-do item</span></span>

<span data-ttu-id="0a527-368">La fonction [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `POST` dont le corps indique la tâche.</span><span class="sxs-lookup"><span data-stu-id="0a527-368">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="0a527-369">Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour spécifier le type de média qui est reçu et envoyé.</span><span class="sxs-lookup"><span data-stu-id="0a527-369">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="0a527-370">La tâche est convertie au format JSON à l’aide de [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="0a527-370">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="0a527-371">Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.</span><span class="sxs-lookup"><span data-stu-id="0a527-371">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="0a527-372">Mettre à jour une tâche</span><span class="sxs-lookup"><span data-stu-id="0a527-372">Update a to-do item</span></span>

<span data-ttu-id="0a527-373">La mise à jour d’une tâche est similaire à l’ajout d’une tâche.</span><span class="sxs-lookup"><span data-stu-id="0a527-373">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="0a527-374">L’identificateur unique de la tâche est ajouté à l’`url` et le `type` est `PUT`.</span><span class="sxs-lookup"><span data-stu-id="0a527-374">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="0a527-375">Supprimer une tâche</span><span class="sxs-lookup"><span data-stu-id="0a527-375">Delete a to-do item</span></span>

<span data-ttu-id="0a527-376">Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="0a527-376">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="0a527-377">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0a527-377">Additional resources</span></span>

<span data-ttu-id="0a527-378">[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="0a527-378">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="0a527-379">Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0a527-379">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="0a527-380">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="0a527-380">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="0a527-381">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="0a527-381">Next steps</span></span>

<span data-ttu-id="0a527-382">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="0a527-382">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0a527-383">Créer un projet d’API web</span><span class="sxs-lookup"><span data-stu-id="0a527-383">Create a web api project.</span></span>
> * <span data-ttu-id="0a527-384">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="0a527-384">Add a model class.</span></span>
> * <span data-ttu-id="0a527-385">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0a527-385">Create the database context.</span></span>
> * <span data-ttu-id="0a527-386">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="0a527-386">Register the database context.</span></span>
> * <span data-ttu-id="0a527-387">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="0a527-387">Add a controller.</span></span>
> * <span data-ttu-id="0a527-388">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="0a527-388">Add CRUD methods.</span></span>
> * <span data-ttu-id="0a527-389">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="0a527-389">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="0a527-390">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="0a527-390">Specify return values.</span></span>
> * <span data-ttu-id="0a527-391">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="0a527-391">Call the web API with Postman.</span></span>
> * <span data-ttu-id="0a527-392">Appeler l’API web avec jQuery</span><span class="sxs-lookup"><span data-stu-id="0a527-392">Call the web api with jQuery.</span></span>

<span data-ttu-id="0a527-393">Passez au tutoriel suivant pour apprendre à générer des pages d’aide d’API :</span><span class="sxs-lookup"><span data-stu-id="0a527-393">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
