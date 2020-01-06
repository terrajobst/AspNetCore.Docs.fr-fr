---
title: 'Didacticiel : créer une API Web avec ASP.NET Core'
author: rick-anderson
description: Apprendre à créer une API web avec ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 3bf930d19684e84365f0ff0255fccd2939fb3f39
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354923"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="98434-103">Didacticiel : créer une API Web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98434-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="98434-104">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="98434-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="98434-105">Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="98434-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="98434-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="98434-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="98434-107">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="98434-107">Create a web API project.</span></span>
> * <span data-ttu-id="98434-108">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="98434-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="98434-109">Générer automatiquement des modèles pour un contrôleur avec des méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="98434-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="98434-110">Configurer le routage, les chemins d’URL et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="98434-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="98434-111">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="98434-111">Call the web API with Postman.</span></span>

<span data-ttu-id="98434-112">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="98434-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="98434-113">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="98434-113">Overview</span></span>

<span data-ttu-id="98434-114">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="98434-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="98434-115">API</span><span class="sxs-lookup"><span data-stu-id="98434-115">API</span></span> | <span data-ttu-id="98434-116">Description</span><span class="sxs-lookup"><span data-stu-id="98434-116">Description</span></span> | <span data-ttu-id="98434-117">Corps de demande</span><span class="sxs-lookup"><span data-stu-id="98434-117">Request body</span></span> | <span data-ttu-id="98434-118">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="98434-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="98434-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="98434-119">GET /api/TodoItems</span></span> | <span data-ttu-id="98434-120">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="98434-120">Get all to-do items</span></span> | <span data-ttu-id="98434-121">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-121">None</span></span> | <span data-ttu-id="98434-122">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="98434-122">Array of to-do items</span></span>|
|<span data-ttu-id="98434-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="98434-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="98434-124">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="98434-124">Get an item by ID</span></span> | <span data-ttu-id="98434-125">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-125">None</span></span> | <span data-ttu-id="98434-126">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-126">To-do item</span></span>|
|<span data-ttu-id="98434-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="98434-127">POST /api/TodoItems</span></span> | <span data-ttu-id="98434-128">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="98434-128">Add a new item</span></span> | <span data-ttu-id="98434-129">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-129">To-do item</span></span> | <span data-ttu-id="98434-130">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-130">To-do item</span></span> |
|<span data-ttu-id="98434-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="98434-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="98434-132">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="98434-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="98434-133">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-133">To-do item</span></span> | <span data-ttu-id="98434-134">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-134">None</span></span> |
|<span data-ttu-id="98434-135">SUPPRIMER le &nbsp;/api/TodoItems/{id} &nbsp;</span><span class="sxs-lookup"><span data-stu-id="98434-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="98434-136">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="98434-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="98434-137">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-137">None</span></span> | <span data-ttu-id="98434-138">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-138">None</span></span>|

<span data-ttu-id="98434-139">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-139">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="98434-145">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="98434-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-148">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="98434-149">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="98434-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-151">Dans le menu **fichier** , sélectionnez **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="98434-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="98434-152">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="98434-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="98434-153">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="98434-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="98434-154">Dans la boîte de dialogue **créer une application Web ASP.net Core** , vérifiez que **.net Core** et **ASP.net Core 3,1** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="98434-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="98434-155">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="98434-155">Select the **API** template and click **Create**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="98434-158">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="98434-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="98434-159">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="98434-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="98434-160">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="98434-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="98434-161">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="98434-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="98434-162">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="98434-162">The preceding commands:</span></span>

  * <span data-ttu-id="98434-163">Créent un projet API Web et l’ouvrent dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="98434-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="98434-164">Ajoutent les packages NuGet qui sont requis dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="98434-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-165">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="98434-166">Sélectionnez **fichier** > **nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="98434-166">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="98434-168">Sélectionnez **.net Core** > **application** > **API** > **suivant**.</span><span class="sxs-lookup"><span data-stu-id="98434-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="98434-170">Dans la boîte de dialogue **configurer votre nouvelle API Web ASP.net Core** , sélectionnez **Framework cible** de \* *.net Core 3,1*.</span><span class="sxs-lookup"><span data-stu-id="98434-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="98434-171">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="98434-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="98434-173">Ouvrez un terminal de commande dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="98434-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="98434-174">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="98434-174">Test the API</span></span>

<span data-ttu-id="98434-175">Le modèle de projet crée une API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="98434-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="98434-176">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="98434-178">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="98434-179">Visual Studio lance un navigateur et accède à `https://localhost:<port>/WeatherForecast`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="98434-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="98434-180">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="98434-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="98434-181">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="98434-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="98434-183">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="98434-184">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="98434-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-185">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="98434-186">Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="98434-187">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="98434-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="98434-188">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="98434-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="98434-189">Ajoutez `/WeatherForecast` à l’URL (définissez-la sur `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="98434-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="98434-190">Un code JSON similaire au suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="98434-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="98434-191">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="98434-191">Add a model class</span></span>

<span data-ttu-id="98434-192">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="98434-193">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="98434-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-195">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="98434-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="98434-196">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="98434-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="98434-197">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="98434-198">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="98434-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="98434-199">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="98434-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="98434-200">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="98434-202">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="98434-203">Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-204">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="98434-205">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="98434-205">Right-click the project.</span></span> <span data-ttu-id="98434-206">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="98434-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="98434-207">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-207">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="98434-209">Cliquez avec le bouton droit sur le dossier *Models* , puis sélectionnez **Ajouter** > **nouveau fichier** > **général** > **classe vide**.</span><span class="sxs-lookup"><span data-stu-id="98434-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="98434-210">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="98434-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="98434-211">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="98434-212">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="98434-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="98434-213">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="98434-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="98434-214">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="98434-214">Add a database context</span></span>

<span data-ttu-id="98434-215">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="98434-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="98434-216">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="98434-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="98434-218">Ajouter Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="98434-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="98434-219">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution**.</span><span class="sxs-lookup"><span data-stu-id="98434-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="98434-220">Sélectionnez l’onglet **Parcourir**, puis entrez **Microsoft.EntityFrameworkCore.SqlServer** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="98434-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="98434-221">Dans le volet gauche, sélectionnez **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="98434-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="98434-222">Cochez la case **Projet** dans le volet droit, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="98434-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="98434-223">Utilisez les instructions précédentes pour ajouter le package NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="98434-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Package Manager](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="98434-225">Ajouter le contexte de base de données TodoContext</span><span class="sxs-lookup"><span data-stu-id="98434-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="98434-226">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="98434-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="98434-227">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="98434-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="98434-228">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="98434-229">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="98434-230">Entrez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="98434-231">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="98434-231">Register the database context</span></span>

<span data-ttu-id="98434-232">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="98434-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="98434-233">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="98434-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="98434-234">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="98434-235">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="98434-235">The preceding code:</span></span>

* <span data-ttu-id="98434-236">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="98434-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="98434-237">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="98434-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="98434-238">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="98434-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="98434-239">Générer automatiquement des modèles pour un contrôleur</span><span class="sxs-lookup"><span data-stu-id="98434-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-241">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="98434-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="98434-242">Sélectionnez **ajouter** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="98434-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="98434-243">Sélectionnez **Contrôleur d’API avec actions, utilisant Entity Framework**, puis **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="98434-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="98434-244">Dans la boîte de dialogue **Contrôleur d’API avec actions, utilisant Entity Framework** :</span><span class="sxs-lookup"><span data-stu-id="98434-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="98434-245">Sélectionnez **TodoItem (TodoApi. Models)** dans la **classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="98434-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="98434-246">Sélectionnez **TodoContext (TodoApi. Models)** dans la **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="98434-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="98434-247">Sélectionnez **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="98434-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="98434-248">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="98434-249">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="98434-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="98434-250">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="98434-250">The preceding commands:</span></span>

* <span data-ttu-id="98434-251">Ajoutent les packages NuGet nécessaires à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="98434-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="98434-252">Installent le moteur de génération de modèles automatique (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="98434-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="98434-253">Génèrent automatiquement des modèles pour `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="98434-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="98434-254">Le code généré :</span><span class="sxs-lookup"><span data-stu-id="98434-254">The generated code:</span></span>

* <span data-ttu-id="98434-255">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="98434-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="98434-256">Marque la classe avec l’attribut [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="98434-256">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="98434-257">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="98434-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="98434-258">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="98434-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="98434-259">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="98434-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="98434-260">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="98434-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="98434-261">Examiner la méthode de création de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="98434-262">Remplacez l’instruction return dans `PostTodoItem` pour utiliser l’opérateur [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="98434-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="98434-263">Le code précédent est une méthode HTTP Après, comme indiqué par l’attribut [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="98434-263">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="98434-264">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="98434-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="98434-265">La méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> :</span><span class="sxs-lookup"><span data-stu-id="98434-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="98434-266">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="98434-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="98434-267">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="98434-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="98434-268">Ajoute un en-tête [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) à la réponse.</span><span class="sxs-lookup"><span data-stu-id="98434-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="98434-269">L’en-tête `Location` spécifie l’[URI](https://developer.mozilla.org/docs/Glossary/URI) de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="98434-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="98434-270">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="98434-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="98434-271">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="98434-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="98434-272">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="98434-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="98434-273">Installer Postman</span><span class="sxs-lookup"><span data-stu-id="98434-273">Install Postman</span></span>

<span data-ttu-id="98434-274">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="98434-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="98434-275">Installez [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="98434-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="98434-276">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="98434-276">Start the web app.</span></span>
* <span data-ttu-id="98434-277">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="98434-277">Start Postman.</span></span>
* <span data-ttu-id="98434-278">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="98434-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="98434-279">À partir de **paramètres** de > de **fichier** (onglet**général** ), désactivez la **vérification de certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="98434-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="98434-280">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="98434-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="98434-281">Tester PostTodoItem avec Postman</span><span class="sxs-lookup"><span data-stu-id="98434-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="98434-282">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="98434-282">Create a new request.</span></span>
* <span data-ttu-id="98434-283">Affectez `POST` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="98434-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="98434-284">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="98434-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="98434-285">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="98434-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="98434-286">Définissez le type sur **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="98434-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="98434-287">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="98434-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="98434-288">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-288">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="98434-290">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="98434-290">Test the location header URI</span></span>

* <span data-ttu-id="98434-291">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="98434-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="98434-292">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="98434-292">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="98434-294">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="98434-294">Set the method to GET.</span></span>
* <span data-ttu-id="98434-295">Collez l’URI (par exemple, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="98434-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="98434-296">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="98434-297">Examiner les méthodes GET</span><span class="sxs-lookup"><span data-stu-id="98434-297">Examine the GET methods</span></span>

<span data-ttu-id="98434-298">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="98434-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="98434-299">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur ou de Postman.</span><span class="sxs-lookup"><span data-stu-id="98434-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="98434-300">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="98434-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="98434-301">Une réponse semblable à la suivante est produite par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="98434-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="98434-302">Tester Get avec Postman</span><span class="sxs-lookup"><span data-stu-id="98434-302">Test Get with Postman</span></span>

* <span data-ttu-id="98434-303">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="98434-303">Create a new request.</span></span>
* <span data-ttu-id="98434-304">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="98434-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="98434-305">Définissez l’URL de la requête sur `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="98434-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="98434-306">Par exemple, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="98434-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="98434-307">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="98434-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="98434-308">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-308">Select **Send**.</span></span>

<span data-ttu-id="98434-309">Cette application utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="98434-309">This app uses an in-memory database.</span></span> <span data-ttu-id="98434-310">Si l’application est arrêtée et démarrée, la requête GET précédente ne retourne aucune donnée.</span><span class="sxs-lookup"><span data-stu-id="98434-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="98434-311">Si aucune donnée n’est retournée, publiez ([POST](#post)) les données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="98434-312">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="98434-312">Routing and URL paths</span></span>

<span data-ttu-id="98434-313">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="98434-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="98434-314">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="98434-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="98434-315">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="98434-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="98434-316">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="98434-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="98434-317">Pour cet exemple, le nom de la classe du contrôleur étant **TodoItems**Controller, le nom du contrôleur est « TodoItems ».</span><span class="sxs-lookup"><span data-stu-id="98434-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="98434-318">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="98434-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="98434-319">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="98434-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="98434-320">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="98434-320">This sample doesn't use a template.</span></span> <span data-ttu-id="98434-321">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="98434-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="98434-322">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="98434-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="98434-323">Lorsque `GetTodoItem` est appelé, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="98434-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="98434-324">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="98434-324">Return values</span></span>

<span data-ttu-id="98434-325">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="98434-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="98434-326">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="98434-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="98434-327">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="98434-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="98434-328">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="98434-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="98434-329">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="98434-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="98434-330">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="98434-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="98434-331">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="98434-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="98434-332">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="98434-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="98434-333">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="98434-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="98434-334">Méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-334">The PutTodoItem method</span></span>

<span data-ttu-id="98434-335">Examinez la méthode `PutTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="98434-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="98434-336">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="98434-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="98434-337">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="98434-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="98434-338">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="98434-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="98434-339">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="98434-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="98434-340">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="98434-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="98434-341">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="98434-342">Cet exemple utilise une base de données en mémoire qui doit être initialisée chaque fois que l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="98434-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="98434-343">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="98434-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="98434-344">Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="98434-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="98434-345">Mettez à jour la tâche dont l’ID = 1 et nommez-la « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="98434-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="98434-346">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="98434-346">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="98434-348">Méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="98434-349">Examinez la méthode `DeleteTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="98434-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="98434-350">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-350">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="98434-351">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="98434-351">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="98434-352">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="98434-352">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="98434-353">Définissez l’URI de l’objet à supprimer (par exemple `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="98434-353">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="98434-354">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-354">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="98434-355">Appelez l’API web avec JavaScript</span><span class="sxs-lookup"><span data-stu-id="98434-355">Call the web API with JavaScript</span></span>

<span data-ttu-id="98434-356">Consultez [Didacticiel : appeler une API web ASP.net core avec JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="98434-356">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="98434-357">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="98434-357">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="98434-358">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="98434-358">Create a web API project.</span></span>
> * <span data-ttu-id="98434-359">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="98434-359">Add a model class and a database context.</span></span>
> * <span data-ttu-id="98434-360">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="98434-360">Add a controller.</span></span>
> * <span data-ttu-id="98434-361">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="98434-361">Add CRUD methods.</span></span>
> * <span data-ttu-id="98434-362">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="98434-362">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="98434-363">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="98434-363">Specify return values.</span></span>
> * <span data-ttu-id="98434-364">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="98434-364">Call the web API with Postman.</span></span>
> * <span data-ttu-id="98434-365">Appeler l’API web avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98434-365">Call the web API with JavaScript.</span></span>

<span data-ttu-id="98434-366">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="98434-366">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="98434-367">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="98434-367">Overview</span></span>

<span data-ttu-id="98434-368">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="98434-368">This tutorial creates the following API:</span></span>

|<span data-ttu-id="98434-369">API</span><span class="sxs-lookup"><span data-stu-id="98434-369">API</span></span> | <span data-ttu-id="98434-370">Description</span><span class="sxs-lookup"><span data-stu-id="98434-370">Description</span></span> | <span data-ttu-id="98434-371">Corps de demande</span><span class="sxs-lookup"><span data-stu-id="98434-371">Request body</span></span> | <span data-ttu-id="98434-372">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="98434-372">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="98434-373">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="98434-373">GET /api/TodoItems</span></span> | <span data-ttu-id="98434-374">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="98434-374">Get all to-do items</span></span> | <span data-ttu-id="98434-375">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-375">None</span></span> | <span data-ttu-id="98434-376">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="98434-376">Array of to-do items</span></span>|
|<span data-ttu-id="98434-377">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="98434-377">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="98434-378">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="98434-378">Get an item by ID</span></span> | <span data-ttu-id="98434-379">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-379">None</span></span> | <span data-ttu-id="98434-380">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-380">To-do item</span></span>|
|<span data-ttu-id="98434-381">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="98434-381">POST /api/TodoItems</span></span> | <span data-ttu-id="98434-382">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="98434-382">Add a new item</span></span> | <span data-ttu-id="98434-383">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-383">To-do item</span></span> | <span data-ttu-id="98434-384">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-384">To-do item</span></span> |
|<span data-ttu-id="98434-385">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="98434-385">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="98434-386">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="98434-386">Update an existing item &nbsp;</span></span> | <span data-ttu-id="98434-387">Tâche</span><span class="sxs-lookup"><span data-stu-id="98434-387">To-do item</span></span> | <span data-ttu-id="98434-388">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-388">None</span></span> |
|<span data-ttu-id="98434-389">SUPPRIMER le &nbsp;/api/TodoItems/{id} &nbsp;</span><span class="sxs-lookup"><span data-stu-id="98434-389">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="98434-390">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="98434-390">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="98434-391">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-391">None</span></span> | <span data-ttu-id="98434-392">Aucun</span><span class="sxs-lookup"><span data-stu-id="98434-392">None</span></span>|

<span data-ttu-id="98434-393">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-393">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="98434-399">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="98434-399">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-400">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-400">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-401">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-401">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-402">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-402">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="98434-403">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="98434-403">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-404">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-404">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-405">Dans le menu **fichier** , sélectionnez **nouveau** > **projet**.</span><span class="sxs-lookup"><span data-stu-id="98434-405">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="98434-406">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="98434-406">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="98434-407">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="98434-407">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="98434-408">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 2.2** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="98434-408">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="98434-409">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="98434-409">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="98434-410">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="98434-410">**Don't** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-412">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-412">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="98434-413">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="98434-413">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="98434-414">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="98434-414">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="98434-415">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="98434-415">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="98434-416">Ces commandes créent un projet d’API web et ouvrent une nouvelle instance de Visual Studio Code dans le nouveau dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="98434-416">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="98434-417">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="98434-417">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-418">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-418">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="98434-419">Sélectionnez **fichier** > **nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="98434-419">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="98434-421">Sélectionnez **.net Core** > **application** > **API** > **suivant**.</span><span class="sxs-lookup"><span data-stu-id="98434-421">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="98434-423">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut \* *.NET Core 2.2* pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="98434-423">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="98434-424">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="98434-424">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="98434-426">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="98434-426">Test the API</span></span>

<span data-ttu-id="98434-427">Le modèle de projet crée une API `values`.</span><span class="sxs-lookup"><span data-stu-id="98434-427">The project template creates a `values` API.</span></span> <span data-ttu-id="98434-428">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-428">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-429">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-429">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="98434-430">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-430">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="98434-431">Visual Studio lance un navigateur et accède à `https://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="98434-431">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="98434-432">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="98434-432">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="98434-433">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="98434-433">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-434">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-434">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="98434-435">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-435">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="98434-436">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="98434-436">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-437">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-437">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="98434-438">Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-438">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="98434-439">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="98434-439">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="98434-440">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="98434-440">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="98434-441">Ajoutez `/api/values` à l’URL (définissez-la sur `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="98434-441">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="98434-442">Le code JSON suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="98434-442">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="98434-443">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="98434-443">Add a model class</span></span>

<span data-ttu-id="98434-444">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-444">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="98434-445">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="98434-445">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-446">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-446">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-447">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="98434-447">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="98434-448">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="98434-448">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="98434-449">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-449">Name the folder *Models*.</span></span>

* <span data-ttu-id="98434-450">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="98434-450">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="98434-451">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="98434-451">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="98434-452">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-452">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="98434-453">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="98434-453">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="98434-454">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-454">Add a folder named *Models*.</span></span>

* <span data-ttu-id="98434-455">Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-455">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="98434-456">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-456">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="98434-457">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="98434-457">Right-click the project.</span></span> <span data-ttu-id="98434-458">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="98434-458">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="98434-459">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-459">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="98434-461">Cliquez avec le bouton droit sur le dossier *Models* , puis sélectionnez **Ajouter** > **nouveau fichier** > **général** > **classe vide**.</span><span class="sxs-lookup"><span data-stu-id="98434-461">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="98434-462">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="98434-462">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="98434-463">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-463">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="98434-464">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="98434-464">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="98434-465">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="98434-465">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="98434-466">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="98434-466">Add a database context</span></span>

<span data-ttu-id="98434-467">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="98434-467">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="98434-468">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="98434-468">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-469">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-469">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-470">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="98434-470">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="98434-471">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="98434-471">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="98434-472">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-472">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="98434-473">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="98434-473">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="98434-474">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-474">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="98434-475">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="98434-475">Register the database context</span></span>

<span data-ttu-id="98434-476">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="98434-476">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="98434-477">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="98434-477">The container provides the service to controllers.</span></span>

<span data-ttu-id="98434-478">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-478">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="98434-479">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="98434-479">The preceding code:</span></span>

* <span data-ttu-id="98434-480">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="98434-480">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="98434-481">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="98434-481">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="98434-482">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="98434-482">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="98434-483">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="98434-483">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-484">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-484">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-485">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="98434-485">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="98434-486">Sélectionnez **ajouter** > **nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="98434-486">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="98434-487">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="98434-487">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="98434-488">Nommez la classe *TodoController* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="98434-488">Name the class *TodoController*, and select **Add**.</span></span>

  ![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="98434-490">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-490">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="98434-491">Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="98434-491">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="98434-492">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-492">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="98434-493">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="98434-493">The preceding code:</span></span>

* <span data-ttu-id="98434-494">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="98434-494">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="98434-495">Marque la classe avec l’attribut [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="98434-495">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="98434-496">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="98434-496">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="98434-497">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="98434-497">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="98434-498">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="98434-498">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="98434-499">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="98434-499">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="98434-500">Ajoute un élément nommé `Item1` à la base de données si celle-ci est vide.</span><span class="sxs-lookup"><span data-stu-id="98434-500">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="98434-501">Ce code se trouvant dans le constructeur, il s’exécute chaque fois qu’une nouvelle requête HTTP existe.</span><span class="sxs-lookup"><span data-stu-id="98434-501">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="98434-502">Si vous supprimez tous les éléments, le constructeur recrée `Item1` au prochain appel d’une méthode d’API.</span><span class="sxs-lookup"><span data-stu-id="98434-502">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="98434-503">Ainsi, il peut vous sembler à tort que la suppression n’a pas fonctionné.</span><span class="sxs-lookup"><span data-stu-id="98434-503">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="98434-504">Ajouter des méthodes Get</span><span class="sxs-lookup"><span data-stu-id="98434-504">Add Get methods</span></span>

<span data-ttu-id="98434-505">Pour fournir une API qui récupère les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="98434-505">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="98434-506">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="98434-506">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="98434-507">Arrêtez l’application si elle est toujours en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="98434-507">Stop the app if it's still running.</span></span> <span data-ttu-id="98434-508">Ensuite, réexécutez-la pour inclure les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="98434-508">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="98434-509">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="98434-509">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="98434-510">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="98434-510">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="98434-511">La réponse HTTP suivante est générée par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="98434-511">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="98434-512">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="98434-512">Routing and URL paths</span></span>

<span data-ttu-id="98434-513">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="98434-513">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="98434-514">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="98434-514">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="98434-515">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="98434-515">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="98434-516">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="98434-516">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="98434-517">Pour cet exemple, le nom de classe du contrôleur étant **Todo**Controller, le nom du contrôleur est « todo ».</span><span class="sxs-lookup"><span data-stu-id="98434-517">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="98434-518">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="98434-518">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="98434-519">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="98434-519">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="98434-520">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="98434-520">This sample doesn't use a template.</span></span> <span data-ttu-id="98434-521">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="98434-521">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="98434-522">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="98434-522">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="98434-523">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="98434-523">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="98434-524">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="98434-524">Return values</span></span>

<span data-ttu-id="98434-525">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="98434-525">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="98434-526">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="98434-526">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="98434-527">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="98434-527">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="98434-528">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="98434-528">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="98434-529">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="98434-529">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="98434-530">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="98434-530">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="98434-531">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="98434-531">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="98434-532">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="98434-532">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="98434-533">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="98434-533">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="98434-534">Tester la méthode GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="98434-534">Test the GetTodoItems method</span></span>

<span data-ttu-id="98434-535">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="98434-535">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="98434-536">Installez le [poste de publication](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="98434-536">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="98434-537">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="98434-537">Start the web app.</span></span>
* <span data-ttu-id="98434-538">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="98434-538">Start Postman.</span></span>
* <span data-ttu-id="98434-539">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="98434-539">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="98434-540">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98434-540">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="98434-541">À partir de **paramètres** de > de **fichier** (onglet**général** ), désactivez la **vérification de certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="98434-541">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="98434-542">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="98434-542">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="98434-543">À partir de **Postman** > **Préférences** (onglet **Général**), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="98434-543">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="98434-544">Vous pouvez également sélectionner la clé et sélectionner **Paramètres**, puis désactiver la vérification du certificat SSL.</span><span class="sxs-lookup"><span data-stu-id="98434-544">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="98434-545">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="98434-545">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="98434-546">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="98434-546">Create a new request.</span></span>
  * <span data-ttu-id="98434-547">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="98434-547">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="98434-548">Définissez l’URL de la requête sur `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="98434-548">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="98434-549">Par exemple, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="98434-549">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="98434-550">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="98434-550">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="98434-551">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-551">Select **Send**.</span></span>

![Postman avec requête Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="98434-553">Ajouter une méthode Create</span><span class="sxs-lookup"><span data-stu-id="98434-553">Add a Create method</span></span>

<span data-ttu-id="98434-554">Ajoutez la méthode `PostTodoItem` suivante à *Controllers/TodoController.cs* :</span><span class="sxs-lookup"><span data-stu-id="98434-554">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="98434-555">Le code précédent est une méthode HTTP Après, comme indiqué par l’attribut [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="98434-555">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="98434-556">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="98434-556">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="98434-557">La méthode `CreatedAtAction` :</span><span class="sxs-lookup"><span data-stu-id="98434-557">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="98434-558">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="98434-558">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="98434-559">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="98434-559">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="98434-560">Ajoute un en-tête `Location` à la réponse.</span><span class="sxs-lookup"><span data-stu-id="98434-560">Adds a `Location` header to the response.</span></span> <span data-ttu-id="98434-561">L’en-tête `Location` spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="98434-561">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="98434-562">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="98434-562">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="98434-563">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="98434-563">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="98434-564">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="98434-564">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="98434-565">Tester la méthode PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-565">Test the PostTodoItem method</span></span>

* <span data-ttu-id="98434-566">créer le projet ;</span><span class="sxs-lookup"><span data-stu-id="98434-566">Build the project.</span></span>
* <span data-ttu-id="98434-567">Dans Postman, définissez la méthode HTTP sur `POST`.</span><span class="sxs-lookup"><span data-stu-id="98434-567">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="98434-568">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="98434-568">Select the **Body** tab.</span></span>
* <span data-ttu-id="98434-569">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="98434-569">Select the **raw** radio button.</span></span>
* <span data-ttu-id="98434-570">Définissez le type sur **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="98434-570">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="98434-571">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="98434-571">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="98434-572">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-572">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/create.png)

  <span data-ttu-id="98434-574">Si vous obtenez une erreur 405 Méthode non autorisée, il est probable que le projet n’ait pas été compilé après l’ajout de la méthode `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="98434-574">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="98434-575">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="98434-575">Test the location header URI</span></span>

* <span data-ttu-id="98434-576">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="98434-576">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="98434-577">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="98434-577">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="98434-579">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="98434-579">Set the method to GET.</span></span>
* <span data-ttu-id="98434-580">Collez l’URI (par exemple, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="98434-580">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="98434-581">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-581">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="98434-582">Ajouter une méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-582">Add a PutTodoItem method</span></span>

<span data-ttu-id="98434-583">Ajoutez la méthode `PutTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="98434-583">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="98434-584">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="98434-584">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="98434-585">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="98434-585">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="98434-586">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="98434-586">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="98434-587">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="98434-587">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="98434-588">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="98434-588">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="98434-589">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-589">Test the PutTodoItem method</span></span>

<span data-ttu-id="98434-590">Cet exemple utilise une base de données en mémoire qui doit être initialisée chaque fois que l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="98434-590">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="98434-591">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="98434-591">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="98434-592">Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="98434-592">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="98434-593">Mettez à jour la tâche dont l’id est 1 en définissant son nom sur « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="98434-593">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="98434-594">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="98434-594">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="98434-596">Ajouter une méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-596">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="98434-597">Ajoutez la méthode `DeleteTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="98434-597">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="98434-598">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="98434-598">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="98434-599">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="98434-599">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="98434-600">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="98434-600">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="98434-601">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="98434-601">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="98434-602">Définissez l’URI de l’objet à supprimer (par exemple `https://localhost:5001/api/todo/1`).</span><span class="sxs-lookup"><span data-stu-id="98434-602">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="98434-603">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="98434-603">Select **Send**.</span></span>

<span data-ttu-id="98434-604">L’exemple d’application vous permet de supprimer tous les éléments.</span><span class="sxs-lookup"><span data-stu-id="98434-604">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="98434-605">Toutefois, quand le dernier élément est supprimé, un autre est créé par le constructeur de classe de modèle au prochain appel de l’API.</span><span class="sxs-lookup"><span data-stu-id="98434-605">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="98434-606">Appelez l’API web avec JavaScript</span><span class="sxs-lookup"><span data-stu-id="98434-606">Call the web API with JavaScript</span></span>

<span data-ttu-id="98434-607">Dans cette section, une page HTML qui utilise JavaScript pour appeler l’API web est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="98434-607">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="98434-608">jQuery lance la demande.</span><span class="sxs-lookup"><span data-stu-id="98434-608">jQuery initiates the request.</span></span> <span data-ttu-id="98434-609">JavaScript met à jour la page avec les détails de la réponse de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="98434-609">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="98434-610">Configurez l’application pour [traiter les fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [activer le mappage de fichier par défaut](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en mettant à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-610">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="98434-611">Créez un dossier *wwwroot* dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="98434-611">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="98434-612">Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="98434-612">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="98434-613">Remplacez son contenu par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-613">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="98434-614">Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="98434-614">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="98434-615">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="98434-615">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="98434-616">Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :</span><span class="sxs-lookup"><span data-stu-id="98434-616">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="98434-617">Ouvrez *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="98434-617">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="98434-618">Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.</span><span class="sxs-lookup"><span data-stu-id="98434-618">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="98434-619">Cet exemple appelle toutes les méthodes CRUD de l’API web.</span><span class="sxs-lookup"><span data-stu-id="98434-619">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="98434-620">Les explications suivantes traitent des appels à l’API.</span><span class="sxs-lookup"><span data-stu-id="98434-620">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="98434-621">Obtenir une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="98434-621">Get a list of to-do items</span></span>

<span data-ttu-id="98434-622">jQuery envoie une requête HTTP obtenir à l’API Web, qui retourne JSON représentant un tableau d’éléments de tâche.</span><span class="sxs-lookup"><span data-stu-id="98434-622">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="98434-623">La fonction de rappel `success` est appelée si la requête réussit.</span><span class="sxs-lookup"><span data-stu-id="98434-623">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="98434-624">Dans le rappel, le DOM est mis à jour avec les informations des tâches.</span><span class="sxs-lookup"><span data-stu-id="98434-624">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="98434-625">Ajouter une tâche</span><span class="sxs-lookup"><span data-stu-id="98434-625">Add a to-do item</span></span>

<span data-ttu-id="98434-626">jQuery envoie une requête HTTP POSTÉRIEURe avec l’élément de tâche dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="98434-626">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="98434-627">Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour spécifier le type de média qui est reçu et envoyé.</span><span class="sxs-lookup"><span data-stu-id="98434-627">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="98434-628">La tâche est convertie au format JSON à l’aide de [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="98434-628">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="98434-629">Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.</span><span class="sxs-lookup"><span data-stu-id="98434-629">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="98434-630">Mettre à jour une tâche</span><span class="sxs-lookup"><span data-stu-id="98434-630">Update a to-do item</span></span>

<span data-ttu-id="98434-631">La mise à jour d’une tâche est similaire à l’ajout d’une tâche.</span><span class="sxs-lookup"><span data-stu-id="98434-631">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="98434-632">L’identificateur unique de la tâche est ajouté à l’`url` et le `type` est `PUT`.</span><span class="sxs-lookup"><span data-stu-id="98434-632">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="98434-633">Supprimer une tâche</span><span class="sxs-lookup"><span data-stu-id="98434-633">Delete a to-do item</span></span>

<span data-ttu-id="98434-634">Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="98434-634">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="98434-635">Ajouter la prise en charge de l’authentification à une API Web</span><span class="sxs-lookup"><span data-stu-id="98434-635">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="98434-636">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="98434-636">Additional resources</span></span>

<span data-ttu-id="98434-637">[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="98434-637">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="98434-638">Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="98434-638">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="98434-639">Pour plus d'informations, voir les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="98434-639">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="98434-640">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="98434-640">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
