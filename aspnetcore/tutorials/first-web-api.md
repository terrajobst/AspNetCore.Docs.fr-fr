---
title: 'Tutoriel : Créer une API web avec ASP.NET Core'
author: rick-anderson
description: Apprendre à créer une API web avec ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 855d05fa2b9c1a7572212c40adbe61bb396f4bac
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819836"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="e7637-103">Tutoriel : Créer une API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7637-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="e7637-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e7637-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e7637-105">Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7637-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e7637-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="e7637-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e7637-107">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="e7637-107">Create a web API project.</span></span>
> * <span data-ttu-id="e7637-108">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="e7637-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e7637-109">Générer automatiquement des modèles pour un contrôleur avec des méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="e7637-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="e7637-110">Configurer le routage, les chemins d’URL et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="e7637-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="e7637-111">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="e7637-111">Call the web API with Postman.</span></span>

<span data-ttu-id="e7637-112">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="e7637-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="e7637-113">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e7637-113">Overview</span></span>

<span data-ttu-id="e7637-114">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="e7637-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e7637-115">API</span><span class="sxs-lookup"><span data-stu-id="e7637-115">API</span></span> | <span data-ttu-id="e7637-116">Description</span><span class="sxs-lookup"><span data-stu-id="e7637-116">Description</span></span> | <span data-ttu-id="e7637-117">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="e7637-117">Request body</span></span> | <span data-ttu-id="e7637-118">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="e7637-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="e7637-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e7637-119">GET /api/TodoItems</span></span> | <span data-ttu-id="e7637-120">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="e7637-120">Get all to-do items</span></span> | <span data-ttu-id="e7637-121">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-121">None</span></span> | <span data-ttu-id="e7637-122">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="e7637-122">Array of to-do items</span></span>|
|<span data-ttu-id="e7637-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="e7637-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="e7637-124">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="e7637-124">Get an item by ID</span></span> | <span data-ttu-id="e7637-125">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-125">None</span></span> | <span data-ttu-id="e7637-126">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-126">To-do item</span></span>|
|<span data-ttu-id="e7637-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e7637-127">POST /api/TodoItems</span></span> | <span data-ttu-id="e7637-128">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="e7637-128">Add a new item</span></span> | <span data-ttu-id="e7637-129">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-129">To-do item</span></span> | <span data-ttu-id="e7637-130">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-130">To-do item</span></span> |
|<span data-ttu-id="e7637-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="e7637-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="e7637-132">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e7637-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e7637-133">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-133">To-do item</span></span> | <span data-ttu-id="e7637-134">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-134">None</span></span> |
|<span data-ttu-id="e7637-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e7637-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="e7637-136">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e7637-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e7637-137">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-137">None</span></span> | <span data-ttu-id="e7637-138">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-138">None</span></span>|

<span data-ttu-id="e7637-139">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-139">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e7637-145">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e7637-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-148">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e7637-149">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="e7637-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-151">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="e7637-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e7637-152">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e7637-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e7637-153">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e7637-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e7637-154">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="e7637-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="e7637-155">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e7637-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="e7637-156">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="e7637-156">**Don't** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7637-159">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e7637-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e7637-160">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e7637-161">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e7637-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="e7637-162">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e7637-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="e7637-163">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="e7637-163">The preceding commands:</span></span>

  * <span data-ttu-id="e7637-164">Créent un projet API Web et l’ouvrent dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e7637-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="e7637-165">Ajoutent les packages NuGet qui sont requis dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="e7637-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-166">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7637-167">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="e7637-167">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e7637-169">Sélectionnez **.NET Core** > **Application** > **API** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e7637-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="e7637-171">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, sélectionnez *.NET Core 3.0* comme **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="e7637-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="e7637-172">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e7637-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="e7637-174">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="e7637-174">Test the API</span></span>

<span data-ttu-id="e7637-175">Le modèle de projet crée une API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="e7637-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="e7637-176">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e7637-178">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e7637-179">Visual Studio lance un navigateur et accède à `https://localhost:<port>/WeatherForecast`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e7637-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e7637-180">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e7637-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e7637-181">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e7637-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e7637-183">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e7637-184">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="e7637-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-185">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e7637-186">Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e7637-187">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e7637-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e7637-188">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="e7637-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e7637-189">Ajoutez `/WeatherForecast` à l’URL (définissez-la sur `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="e7637-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="e7637-190">Un code JSON similaire au suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="e7637-190">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="e7637-191">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="e7637-191">Add a model class</span></span>

<span data-ttu-id="e7637-192">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e7637-193">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="e7637-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-195">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e7637-196">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e7637-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e7637-197">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="e7637-198">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e7637-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e7637-199">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e7637-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e7637-200">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7637-202">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="e7637-203">Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-204">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7637-205">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-205">Right-click the project.</span></span> <span data-ttu-id="e7637-206">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e7637-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e7637-207">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-207">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e7637-209">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="e7637-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e7637-210">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="e7637-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="e7637-211">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e7637-212">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e7637-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e7637-213">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="e7637-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e7637-214">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e7637-214">Add a database context</span></span>

<span data-ttu-id="e7637-215">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e7637-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e7637-216">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e7637-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="e7637-218">Ajouter Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e7637-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="e7637-219">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution**.</span><span class="sxs-lookup"><span data-stu-id="e7637-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="e7637-220">Cochez la case **Inclure la préversion**.</span><span class="sxs-lookup"><span data-stu-id="e7637-220">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="e7637-221">Sélectionnez l’onglet **Parcourir**, puis entrez **Microsoft.EntityFrameworkCore.SqlServer** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="e7637-221">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="e7637-222">Sélectionnez **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** dans le volet gauche.</span><span class="sxs-lookup"><span data-stu-id="e7637-222">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="e7637-223">Cochez la case **Projet** dans le volet droit, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="e7637-223">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="e7637-224">Utilisez les instructions précédentes pour ajouter le package NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="e7637-224">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Package Manager](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="e7637-226">Ajouter le contexte de base de données TodoContext</span><span class="sxs-lookup"><span data-stu-id="e7637-226">Add the TodoContext database context</span></span>

* <span data-ttu-id="e7637-227">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e7637-227">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e7637-228">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e7637-228">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e7637-229">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e7637-230">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-230">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e7637-231">Entrez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-231">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e7637-232">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e7637-232">Register the database context</span></span>

<span data-ttu-id="e7637-233">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e7637-233">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e7637-234">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="e7637-234">The container provides the service to controllers.</span></span>

<span data-ttu-id="e7637-235">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-235">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="e7637-236">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e7637-236">The preceding code:</span></span>

* <span data-ttu-id="e7637-237">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="e7637-237">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e7637-238">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e7637-238">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e7637-239">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e7637-239">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="e7637-240">Générer automatiquement des modèles pour un contrôleur</span><span class="sxs-lookup"><span data-stu-id="e7637-240">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-241">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-241">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-242">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="e7637-242">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e7637-243">Sélectionnez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="e7637-243">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e7637-244">Sélectionnez **Contrôleur d’API avec actions, utilisant Entity Framework**, puis **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e7637-244">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="e7637-245">Dans la boîte de dialogue **Contrôleur d’API avec actions, utilisant Entity Framework** :</span><span class="sxs-lookup"><span data-stu-id="e7637-245">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="e7637-246">Sélectionnez **TodoItem (TodoAPI.Models)** dans **Classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="e7637-246">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="e7637-247">Sélectionnez **TodoContext (TodoAPI.Models)** dans **Classe du contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="e7637-247">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="e7637-248">Sélectionnez **Ajouter**</span><span class="sxs-lookup"><span data-stu-id="e7637-248">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e7637-249">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e7637-250">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e7637-250">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="e7637-251">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="e7637-251">The preceding commands:</span></span>

* <span data-ttu-id="e7637-252">Ajoutent les packages NuGet nécessaires à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="e7637-252">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="e7637-253">Installent le moteur de génération de modèles automatique (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="e7637-253">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="e7637-254">Génèrent automatiquement des modèles pour `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="e7637-254">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="e7637-255">Le code généré :</span><span class="sxs-lookup"><span data-stu-id="e7637-255">The generated code:</span></span>

* <span data-ttu-id="e7637-256">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="e7637-256">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e7637-257">Décore la classe avec l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="e7637-257">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="e7637-258">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="e7637-258">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e7637-259">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="e7637-259">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e7637-260">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e7637-260">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e7637-261">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e7637-261">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="e7637-262">Examiner la méthode de création de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-262">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="e7637-263">Remplacez l’instruction return dans `PostTodoItem` pour utiliser l’opérateur [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="e7637-263">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="e7637-264">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="e7637-264">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e7637-265">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7637-265">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e7637-266">La méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> :</span><span class="sxs-lookup"><span data-stu-id="e7637-266">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="e7637-267">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="e7637-267">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="e7637-268">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e7637-268">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e7637-269">Ajoute un en-tête [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) à la réponse.</span><span class="sxs-lookup"><span data-stu-id="e7637-269">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="e7637-270">L’en-tête `Location` spécifie l’[URI](https://developer.mozilla.org/docs/Glossary/URI) de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="e7637-270">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="e7637-271">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e7637-271">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e7637-272">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="e7637-272">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e7637-273">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="e7637-273">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="e7637-274">Installer Postman</span><span class="sxs-lookup"><span data-stu-id="e7637-274">Install Postman</span></span>

<span data-ttu-id="e7637-275">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="e7637-275">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e7637-276">Installez [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e7637-276">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="e7637-277">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="e7637-277">Start the web app.</span></span>
* <span data-ttu-id="e7637-278">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="e7637-278">Start Postman.</span></span>
* <span data-ttu-id="e7637-279">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e7637-279">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="e7637-280">À partir de **Fichier > Paramètres** (onglet \**Général*), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e7637-280">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="e7637-281">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e7637-281">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="e7637-282">Tester PostTodoItem avec Postman</span><span class="sxs-lookup"><span data-stu-id="e7637-282">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="e7637-283">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e7637-283">Create a new request.</span></span>
* <span data-ttu-id="e7637-284">Affectez `POST` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7637-284">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e7637-285">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="e7637-285">Select the **Body** tab.</span></span>
* <span data-ttu-id="e7637-286">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="e7637-286">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e7637-287">Définissez le type sur **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="e7637-287">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e7637-288">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="e7637-288">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e7637-289">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="e7637-289">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="e7637-291">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="e7637-291">Test the location header URI</span></span>

* <span data-ttu-id="e7637-292">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="e7637-292">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e7637-293">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="e7637-293">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="e7637-295">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="e7637-295">Set the method to GET.</span></span>
* <span data-ttu-id="e7637-296">Collez l’URI (par exemple, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="e7637-296">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="e7637-297">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="e7637-297">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="e7637-298">Examiner les méthodes GET</span><span class="sxs-lookup"><span data-stu-id="e7637-298">Examine the GET methods</span></span>

<span data-ttu-id="e7637-299">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="e7637-299">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="e7637-300">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur ou de Postman.</span><span class="sxs-lookup"><span data-stu-id="e7637-300">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="e7637-301">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e7637-301">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="e7637-302">Une réponse semblable à la suivante est produite par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="e7637-302">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="e7637-303">Tester Get avec Postman</span><span class="sxs-lookup"><span data-stu-id="e7637-303">Test Get with Postman</span></span>

* <span data-ttu-id="e7637-304">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e7637-304">Create a new request.</span></span>
* <span data-ttu-id="e7637-305">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="e7637-305">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="e7637-306">Définissez l’URL de la requête sur `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e7637-306">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e7637-307">Par exemple, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="e7637-307">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e7637-308">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="e7637-308">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e7637-309">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="e7637-309">Select **Send**.</span></span>

<span data-ttu-id="e7637-310">Cette application utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e7637-310">This app uses an in-memory database.</span></span> <span data-ttu-id="e7637-311">Si l’application est arrêtée et démarrée, la requête GET précédente ne retourne aucune donnée.</span><span class="sxs-lookup"><span data-stu-id="e7637-311">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="e7637-312">Si aucune donnée n’est retournée, publiez ([POST](#post)) les données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-312">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="e7637-313">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="e7637-313">Routing and URL paths</span></span>

<span data-ttu-id="e7637-314">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e7637-314">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e7637-315">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="e7637-315">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e7637-316">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e7637-316">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="e7637-317">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="e7637-317">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e7637-318">Pour cet exemple, le nom de la classe du contrôleur étant **TodoItems**Controller, le nom du contrôleur est « TodoItems ».</span><span class="sxs-lookup"><span data-stu-id="e7637-318">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="e7637-319">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e7637-319">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e7637-320">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="e7637-320">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e7637-321">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="e7637-321">This sample doesn't use a template.</span></span> <span data-ttu-id="e7637-322">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e7637-322">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e7637-323">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="e7637-323">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e7637-324">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="e7637-324">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e7637-325">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="e7637-325">Return values</span></span>

<span data-ttu-id="e7637-326">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="e7637-326">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e7637-327">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="e7637-327">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e7637-328">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="e7637-328">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e7637-329">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="e7637-329">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e7637-330">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7637-330">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e7637-331">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="e7637-331">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e7637-332">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="e7637-332">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="e7637-333">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="e7637-333">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e7637-334">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e7637-334">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="e7637-335">Méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-335">The PutTodoItem method</span></span>

<span data-ttu-id="e7637-336">Examinez la méthode `PutTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="e7637-336">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="e7637-337">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e7637-337">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e7637-338">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e7637-338">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e7637-339">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="e7637-339">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e7637-340">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="e7637-340">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e7637-341">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="e7637-341">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e7637-342">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-342">Test the PutTodoItem method</span></span>

<span data-ttu-id="e7637-343">Cet exemple utilise une base de données en mémoire qui doit être initialisée à chaque démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-343">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="e7637-344">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e7637-344">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e7637-345">Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e7637-345">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e7637-346">Mettez à jour la tâche dont l’ID = 1 et nommez-la « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="e7637-346">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e7637-347">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="e7637-347">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="e7637-349">Méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="e7637-350">Examinez la méthode `DeleteTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="e7637-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="e7637-351">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e7637-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e7637-352">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e7637-353">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="e7637-353">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e7637-354">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="e7637-354">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e7637-355">Définissez l’URI de l’objet à supprimer, par exemple `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="e7637-355">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="e7637-356">Sélectionnez **Send**.</span><span class="sxs-lookup"><span data-stu-id="e7637-356">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="e7637-357">Appeler l’API à partir de jQuery</span><span class="sxs-lookup"><span data-stu-id="e7637-357">Call the API from jQuery</span></span>

<span data-ttu-id="e7637-358">Consultez le [tutoriel : Appeler une API web ASP.NET Core avec jQuery](xref:tutorials/web-api-jquery).</span><span class="sxs-lookup"><span data-stu-id="e7637-358">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e7637-359">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="e7637-359">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e7637-360">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="e7637-360">Create a web API project.</span></span>
> * <span data-ttu-id="e7637-361">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="e7637-361">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e7637-362">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="e7637-362">Add a controller.</span></span>
> * <span data-ttu-id="e7637-363">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="e7637-363">Add CRUD methods.</span></span>
> * <span data-ttu-id="e7637-364">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="e7637-364">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="e7637-365">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="e7637-365">Specify return values.</span></span>
> * <span data-ttu-id="e7637-366">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="e7637-366">Call the web API with Postman.</span></span>
> * <span data-ttu-id="e7637-367">Appeler l’API web avec jQuery.</span><span class="sxs-lookup"><span data-stu-id="e7637-367">Call the web API with jQuery.</span></span>

<span data-ttu-id="e7637-368">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e7637-368">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="e7637-369">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="e7637-369">Overview</span></span>

<span data-ttu-id="e7637-370">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="e7637-370">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e7637-371">API</span><span class="sxs-lookup"><span data-stu-id="e7637-371">API</span></span> | <span data-ttu-id="e7637-372">Description</span><span class="sxs-lookup"><span data-stu-id="e7637-372">Description</span></span> | <span data-ttu-id="e7637-373">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="e7637-373">Request body</span></span> | <span data-ttu-id="e7637-374">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="e7637-374">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="e7637-375">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e7637-375">GET /api/TodoItems</span></span> | <span data-ttu-id="e7637-376">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="e7637-376">Get all to-do items</span></span> | <span data-ttu-id="e7637-377">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-377">None</span></span> | <span data-ttu-id="e7637-378">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="e7637-378">Array of to-do items</span></span>|
|<span data-ttu-id="e7637-379">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="e7637-379">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="e7637-380">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="e7637-380">Get an item by ID</span></span> | <span data-ttu-id="e7637-381">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-381">None</span></span> | <span data-ttu-id="e7637-382">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-382">To-do item</span></span>|
|<span data-ttu-id="e7637-383">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e7637-383">POST /api/TodoItems</span></span> | <span data-ttu-id="e7637-384">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="e7637-384">Add a new item</span></span> | <span data-ttu-id="e7637-385">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-385">To-do item</span></span> | <span data-ttu-id="e7637-386">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-386">To-do item</span></span> |
|<span data-ttu-id="e7637-387">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="e7637-387">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="e7637-388">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e7637-388">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e7637-389">Tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-389">To-do item</span></span> | <span data-ttu-id="e7637-390">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-390">None</span></span> |
|<span data-ttu-id="e7637-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e7637-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="e7637-392">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e7637-392">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e7637-393">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-393">None</span></span> | <span data-ttu-id="e7637-394">Aucun.</span><span class="sxs-lookup"><span data-stu-id="e7637-394">None</span></span>|

<span data-ttu-id="e7637-395">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-395">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e7637-401">Prérequis</span><span class="sxs-lookup"><span data-stu-id="e7637-401">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-402">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-402">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-403">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-403">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-404">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-404">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e7637-405">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="e7637-405">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-407">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="e7637-407">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e7637-408">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e7637-408">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e7637-409">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e7637-409">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e7637-410">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 2.2** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="e7637-410">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="e7637-411">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e7637-411">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="e7637-412">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="e7637-412">**Don't** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-414">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-414">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7637-415">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="e7637-415">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e7637-416">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-416">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e7637-417">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e7637-417">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="e7637-418">Ces commandes créent un projet d’API web et ouvrent une nouvelle instance de Visual Studio Code dans le nouveau dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-418">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="e7637-419">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e7637-419">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-420">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7637-421">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="e7637-421">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e7637-423">Sélectionnez **.NET Core** > **Application** > **API** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e7637-423">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="e7637-425">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut \* *.NET Core 2.2* pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="e7637-425">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="e7637-426">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e7637-426">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="e7637-428">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="e7637-428">Test the API</span></span>

<span data-ttu-id="e7637-429">Le modèle de projet crée une API `values`.</span><span class="sxs-lookup"><span data-stu-id="e7637-429">The project template creates a `values` API.</span></span> <span data-ttu-id="e7637-430">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-430">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-431">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-431">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e7637-432">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-432">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e7637-433">Visual Studio lance un navigateur et accède à `https://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e7637-433">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e7637-434">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e7637-434">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e7637-435">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="e7637-435">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-436">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-436">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e7637-437">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-437">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e7637-438">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="e7637-438">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-439">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e7637-440">Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-440">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e7637-441">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="e7637-441">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e7637-442">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="e7637-442">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e7637-443">Ajoutez `/api/values` à l’URL (définissez-la sur `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="e7637-443">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="e7637-444">Le code JSON suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="e7637-444">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="e7637-445">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="e7637-445">Add a model class</span></span>

<span data-ttu-id="e7637-446">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-446">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e7637-447">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="e7637-447">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-448">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-448">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-449">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-449">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e7637-450">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e7637-450">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e7637-451">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-451">Name the folder *Models*.</span></span>

* <span data-ttu-id="e7637-452">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e7637-452">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e7637-453">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e7637-453">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e7637-454">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-454">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7637-455">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7637-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7637-456">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-456">Add a folder named *Models*.</span></span>

* <span data-ttu-id="e7637-457">Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-457">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7637-458">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7637-459">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-459">Right-click the project.</span></span> <span data-ttu-id="e7637-460">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="e7637-460">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e7637-461">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-461">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e7637-463">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="e7637-463">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e7637-464">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="e7637-464">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="e7637-465">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-465">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e7637-466">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="e7637-466">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e7637-467">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="e7637-467">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e7637-468">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e7637-468">Add a database context</span></span>

<span data-ttu-id="e7637-469">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="e7637-469">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e7637-470">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e7637-470">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-471">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-471">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-472">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e7637-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e7637-473">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e7637-473">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e7637-474">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-474">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e7637-475">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="e7637-475">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e7637-476">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-476">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e7637-477">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="e7637-477">Register the database context</span></span>

<span data-ttu-id="e7637-478">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e7637-478">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e7637-479">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="e7637-479">The container provides the service to controllers.</span></span>

<span data-ttu-id="e7637-480">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-480">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="e7637-481">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e7637-481">The preceding code:</span></span>

* <span data-ttu-id="e7637-482">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="e7637-482">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e7637-483">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e7637-483">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e7637-484">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="e7637-484">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="e7637-485">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="e7637-485">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-487">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="e7637-487">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e7637-488">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="e7637-488">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="e7637-489">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="e7637-489">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="e7637-490">Nommez la classe *TodoController* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e7637-490">Name the class *TodoController*, and select **Add**.</span></span>

  ![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e7637-492">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-492">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e7637-493">Dans le dossier *Controllers*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e7637-493">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="e7637-494">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-494">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e7637-495">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="e7637-495">The preceding code:</span></span>

* <span data-ttu-id="e7637-496">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="e7637-496">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e7637-497">Décore la classe avec l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="e7637-497">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="e7637-498">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="e7637-498">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e7637-499">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="e7637-499">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e7637-500">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e7637-500">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e7637-501">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e7637-501">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="e7637-502">Ajoute un élément nommé `Item1` à la base de données si celle-ci est vide.</span><span class="sxs-lookup"><span data-stu-id="e7637-502">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="e7637-503">Ce code se trouvant dans le constructeur, il s’exécute chaque fois qu’une nouvelle requête HTTP existe.</span><span class="sxs-lookup"><span data-stu-id="e7637-503">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="e7637-504">Si vous supprimez tous les éléments, le constructeur recrée `Item1` au prochain appel d’une méthode d’API.</span><span class="sxs-lookup"><span data-stu-id="e7637-504">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="e7637-505">Ainsi, il peut vous sembler à tort que la suppression n’a pas fonctionné.</span><span class="sxs-lookup"><span data-stu-id="e7637-505">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="e7637-506">Ajouter des méthodes Get</span><span class="sxs-lookup"><span data-stu-id="e7637-506">Add Get methods</span></span>

<span data-ttu-id="e7637-507">Pour fournir une API qui récupère les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="e7637-507">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="e7637-508">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="e7637-508">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e7637-509">Arrêtez l’application si elle est toujours en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="e7637-509">Stop the app if it's still running.</span></span> <span data-ttu-id="e7637-510">Ensuite, réexécutez-la pour inclure les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="e7637-510">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="e7637-511">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="e7637-511">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="e7637-512">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="e7637-512">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="e7637-513">La réponse HTTP suivante est générée par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="e7637-513">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="e7637-514">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="e7637-514">Routing and URL paths</span></span>

<span data-ttu-id="e7637-515">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e7637-515">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e7637-516">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="e7637-516">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e7637-517">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e7637-517">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="e7637-518">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="e7637-518">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e7637-519">Pour cet exemple, le nom de classe du contrôleur étant **Todo**Controller, le nom du contrôleur est « todo ».</span><span class="sxs-lookup"><span data-stu-id="e7637-519">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="e7637-520">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e7637-520">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e7637-521">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="e7637-521">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e7637-522">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="e7637-522">This sample doesn't use a template.</span></span> <span data-ttu-id="e7637-523">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e7637-523">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e7637-524">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="e7637-524">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e7637-525">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="e7637-525">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e7637-526">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="e7637-526">Return values</span></span>

<span data-ttu-id="e7637-527">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="e7637-527">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e7637-528">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="e7637-528">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e7637-529">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="e7637-529">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e7637-530">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="e7637-530">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e7637-531">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7637-531">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e7637-532">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="e7637-532">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e7637-533">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="e7637-533">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="e7637-534">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="e7637-534">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e7637-535">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e7637-535">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="e7637-536">Tester la méthode GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="e7637-536">Test the GetTodoItems method</span></span>

<span data-ttu-id="e7637-537">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="e7637-537">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e7637-538">Installez [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e7637-538">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="e7637-539">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="e7637-539">Start the web app.</span></span>
* <span data-ttu-id="e7637-540">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="e7637-540">Start Postman.</span></span>
* <span data-ttu-id="e7637-541">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e7637-541">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7637-542">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7637-542">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7637-543">À partir de **Fichier** > **Paramètres** (onglet **Général**), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e7637-543">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e7637-544">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e7637-544">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e7637-545">À partir de **Postman** > **Préférences** (onglet **Général**), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="e7637-545">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="e7637-546">Vous pouvez également sélectionner la clé et sélectionner **Paramètres**, puis désactiver la vérification du certificat SSL.</span><span class="sxs-lookup"><span data-stu-id="e7637-546">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="e7637-547">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e7637-547">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="e7637-548">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="e7637-548">Create a new request.</span></span>
  * <span data-ttu-id="e7637-549">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="e7637-549">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="e7637-550">Définissez l’URL de la requête sur `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="e7637-550">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="e7637-551">Par exemple, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="e7637-551">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="e7637-552">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="e7637-552">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e7637-553">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="e7637-553">Select **Send**.</span></span>

![Postman avec requête Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="e7637-555">Ajouter une méthode Create</span><span class="sxs-lookup"><span data-stu-id="e7637-555">Add a Create method</span></span>

<span data-ttu-id="e7637-556">Ajoutez la méthode `PostTodoItem` suivante à *Controllers/TodoController.cs* :</span><span class="sxs-lookup"><span data-stu-id="e7637-556">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="e7637-557">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="e7637-557">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e7637-558">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="e7637-558">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e7637-559">La méthode `CreatedAtAction` :</span><span class="sxs-lookup"><span data-stu-id="e7637-559">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="e7637-560">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="e7637-560">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="e7637-561">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e7637-561">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e7637-562">Ajoute un en-tête `Location` à la réponse.</span><span class="sxs-lookup"><span data-stu-id="e7637-562">Adds a `Location` header to the response.</span></span> <span data-ttu-id="e7637-563">L’en-tête `Location` spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="e7637-563">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e7637-564">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e7637-564">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e7637-565">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="e7637-565">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e7637-566">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="e7637-566">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="e7637-567">Tester la méthode PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-567">Test the PostTodoItem method</span></span>

* <span data-ttu-id="e7637-568">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-568">Build the project.</span></span>
* <span data-ttu-id="e7637-569">Dans Postman, définissez la méthode HTTP sur `POST`.</span><span class="sxs-lookup"><span data-stu-id="e7637-569">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e7637-570">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="e7637-570">Select the **Body** tab.</span></span>
* <span data-ttu-id="e7637-571">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="e7637-571">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e7637-572">Définissez le type sur **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="e7637-572">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e7637-573">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="e7637-573">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e7637-574">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="e7637-574">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/create.png)

  <span data-ttu-id="e7637-576">Si vous obtenez une erreur 405 Méthode non autorisée, il est probable que le projet n’ait pas été compilé après l’ajout de la méthode `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e7637-576">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="e7637-577">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="e7637-577">Test the location header URI</span></span>

* <span data-ttu-id="e7637-578">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="e7637-578">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e7637-579">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="e7637-579">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="e7637-581">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="e7637-581">Set the method to GET.</span></span>
* <span data-ttu-id="e7637-582">Collez l’URI (par exemple, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="e7637-582">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="e7637-583">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="e7637-583">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="e7637-584">Ajouter une méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-584">Add a PutTodoItem method</span></span>

<span data-ttu-id="e7637-585">Ajoutez la méthode `PutTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="e7637-585">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="e7637-586">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e7637-586">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e7637-587">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e7637-587">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e7637-588">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="e7637-588">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e7637-589">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="e7637-589">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e7637-590">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="e7637-590">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e7637-591">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-591">Test the PutTodoItem method</span></span>

<span data-ttu-id="e7637-592">Cet exemple utilise une base de données en mémoire qui doit être initialisée à chaque démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-592">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="e7637-593">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e7637-593">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e7637-594">Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="e7637-594">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e7637-595">Mettez à jour la tâche dont l’ID = 1 et nommez-la « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="e7637-595">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e7637-596">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="e7637-596">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="e7637-598">Ajouter une méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-598">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="e7637-599">Ajoutez la méthode `DeleteTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="e7637-599">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="e7637-600">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e7637-600">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e7637-601">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="e7637-601">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e7637-602">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="e7637-602">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e7637-603">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="e7637-603">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e7637-604">Définissez l’URI de l’objet à supprimer, par exemple `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="e7637-604">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="e7637-605">Sélectionnez **Send**.</span><span class="sxs-lookup"><span data-stu-id="e7637-605">Select **Send**</span></span>

<span data-ttu-id="e7637-606">L’exemple d’application vous permet de supprimer tous les éléments.</span><span class="sxs-lookup"><span data-stu-id="e7637-606">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="e7637-607">Toutefois, quand le dernier élément est supprimé, un autre est créé par le constructeur de classe de modèle au prochain appel de l’API.</span><span class="sxs-lookup"><span data-stu-id="e7637-607">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="e7637-608">Appeler l’API avec jQuery</span><span class="sxs-lookup"><span data-stu-id="e7637-608">Call the API with jQuery</span></span>

<span data-ttu-id="e7637-609">Dans cette section, une page HTML qui utilise jQuery pour appeler l’API web est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="e7637-609">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="e7637-610">jQuery lance la requête et met à jour la page avec les détails de la réponse de l’API.</span><span class="sxs-lookup"><span data-stu-id="e7637-610">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="e7637-611">Configurez l’application pour [traiter les fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [activer le mappage de fichier par défaut](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en mettant à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="e7637-612">Créez un dossier *wwwroot* dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="e7637-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="e7637-613">Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="e7637-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="e7637-614">Remplacez son contenu par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="e7637-615">Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="e7637-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="e7637-616">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e7637-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="e7637-617">Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :</span><span class="sxs-lookup"><span data-stu-id="e7637-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="e7637-618">Ouvrez *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e7637-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="e7637-619">Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.</span><span class="sxs-lookup"><span data-stu-id="e7637-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="e7637-620">Il existe plusieurs façons d’obtenir jQuery.</span><span class="sxs-lookup"><span data-stu-id="e7637-620">There are several ways to get jQuery.</span></span> <span data-ttu-id="e7637-621">Dans l’extrait précédent, la bibliothèque est chargée à partir d’un CDN.</span><span class="sxs-lookup"><span data-stu-id="e7637-621">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="e7637-622">Cet exemple appelle toutes les méthodes CRUD de l’API.</span><span class="sxs-lookup"><span data-stu-id="e7637-622">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="e7637-623">Les explications suivantes traitent des appels à l’API.</span><span class="sxs-lookup"><span data-stu-id="e7637-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="e7637-624">Obtenir une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="e7637-624">Get a list of to-do items</span></span>

<span data-ttu-id="e7637-625">La fonction JQuery [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `GET` à l’API, qui retourne du code JSON représentant un tableau de tâches.</span><span class="sxs-lookup"><span data-stu-id="e7637-625">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="e7637-626">La fonction de rappel `success` est appelée si la requête réussit.</span><span class="sxs-lookup"><span data-stu-id="e7637-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="e7637-627">Dans le rappel, le DOM est mis à jour avec les informations des tâches.</span><span class="sxs-lookup"><span data-stu-id="e7637-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="e7637-628">Ajouter une tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-628">Add a to-do item</span></span>

<span data-ttu-id="e7637-629">La fonction [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête `POST` dont le corps indique la tâche.</span><span class="sxs-lookup"><span data-stu-id="e7637-629">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="e7637-630">Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour spécifier le type de média qui est reçu et envoyé.</span><span class="sxs-lookup"><span data-stu-id="e7637-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="e7637-631">La tâche est convertie au format JSON à l’aide de [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="e7637-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="e7637-632">Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.</span><span class="sxs-lookup"><span data-stu-id="e7637-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="e7637-633">Mettre à jour une tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-633">Update a to-do item</span></span>

<span data-ttu-id="e7637-634">La mise à jour d’une tâche est similaire à l’ajout d’une tâche.</span><span class="sxs-lookup"><span data-stu-id="e7637-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="e7637-635">L’identificateur unique de la tâche est ajouté à l’`url` et le `type` est `PUT`.</span><span class="sxs-lookup"><span data-stu-id="e7637-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="e7637-636">Supprimer une tâche</span><span class="sxs-lookup"><span data-stu-id="e7637-636">Delete a to-do item</span></span>

<span data-ttu-id="e7637-637">Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="e7637-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e7637-638">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e7637-638">Additional resources</span></span>

<span data-ttu-id="e7637-639">[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="e7637-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="e7637-640">Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="e7637-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="e7637-641">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="e7637-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="e7637-642">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="e7637-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
