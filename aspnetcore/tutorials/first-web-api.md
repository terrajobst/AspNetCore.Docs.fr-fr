---
title: 'Tutoriel : Créer une API web avec ASP.NET Core'
author: rick-anderson
description: Apprendre à créer une API web avec ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 2d0eb24641c3d1f795b9e85ce10d42ee96d30846
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187308"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="6e5bd-103">Tutoriel : Créer une API web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e5bd-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="6e5bd-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="6e5bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="6e5bd-105">Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6e5bd-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6e5bd-107">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-107">Create a web API project.</span></span>
> * <span data-ttu-id="6e5bd-108">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="6e5bd-109">Générer automatiquement des modèles pour un contrôleur avec des méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="6e5bd-110">Configurer le routage, les chemins d’URL et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="6e5bd-111">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="6e5bd-111">Call the web API with Postman.</span></span>

<span data-ttu-id="6e5bd-112">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="6e5bd-113">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="6e5bd-113">Overview</span></span>

<span data-ttu-id="6e5bd-114">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="6e5bd-115">API</span><span class="sxs-lookup"><span data-stu-id="6e5bd-115">API</span></span> | <span data-ttu-id="6e5bd-116">Description</span><span class="sxs-lookup"><span data-stu-id="6e5bd-116">Description</span></span> | <span data-ttu-id="6e5bd-117">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="6e5bd-117">Request body</span></span> | <span data-ttu-id="6e5bd-118">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="6e5bd-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="6e5bd-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6e5bd-119">GET /api/TodoItems</span></span> | <span data-ttu-id="6e5bd-120">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="6e5bd-120">Get all to-do items</span></span> | <span data-ttu-id="6e5bd-121">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-121">None</span></span> | <span data-ttu-id="6e5bd-122">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="6e5bd-122">Array of to-do items</span></span>|
|<span data-ttu-id="6e5bd-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6e5bd-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="6e5bd-124">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="6e5bd-124">Get an item by ID</span></span> | <span data-ttu-id="6e5bd-125">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-125">None</span></span> | <span data-ttu-id="6e5bd-126">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-126">To-do item</span></span>|
|<span data-ttu-id="6e5bd-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6e5bd-127">POST /api/TodoItems</span></span> | <span data-ttu-id="6e5bd-128">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="6e5bd-128">Add a new item</span></span> | <span data-ttu-id="6e5bd-129">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-129">To-do item</span></span> | <span data-ttu-id="6e5bd-130">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-130">To-do item</span></span> |
|<span data-ttu-id="6e5bd-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6e5bd-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="6e5bd-132">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6e5bd-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="6e5bd-133">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-133">To-do item</span></span> | <span data-ttu-id="6e5bd-134">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-134">None</span></span> |
|<span data-ttu-id="6e5bd-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6e5bd-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="6e5bd-136">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6e5bd-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="6e5bd-137">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-137">None</span></span> | <span data-ttu-id="6e5bd-138">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-138">None</span></span>|

<span data-ttu-id="6e5bd-139">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-139">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="6e5bd-145">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6e5bd-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-148">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="6e5bd-149">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="6e5bd-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-151">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="6e5bd-152">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="6e5bd-153">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="6e5bd-154">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="6e5bd-155">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="6e5bd-156">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-156">**Don't** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6e5bd-159">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6e5bd-160">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="6e5bd-161">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-161">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="6e5bd-162">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="6e5bd-163">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-163">The preceding commands:</span></span>

  * <span data-ttu-id="6e5bd-164">Créent un projet API Web et l’ouvrent dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="6e5bd-165">Ajoutent les packages NuGet qui sont requis dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-166">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e5bd-167">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-167">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="6e5bd-169">Sélectionnez **.NET Core** > **Application** > **API** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="6e5bd-171">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, sélectionnez *.NET Core 3.0* comme **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="6e5bd-172">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="6e5bd-174">Ouvrez un terminal de commande dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-174">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="6e5bd-175">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="6e5bd-175">Test the API</span></span>

<span data-ttu-id="6e5bd-176">Le modèle de projet crée une API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-176">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="6e5bd-177">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-177">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-178">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6e5bd-179">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6e5bd-180">Visual Studio lance un navigateur et accède à `https://localhost:<port>/WeatherForecast`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-180">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="6e5bd-181">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-181">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="6e5bd-182">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-182">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-183">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-183">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6e5bd-184">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-184">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6e5bd-185">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-185">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-186">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-186">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6e5bd-187">Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-187">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="6e5bd-188">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-188">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6e5bd-189">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-189">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="6e5bd-190">Ajoutez `/WeatherForecast` à l’URL (définissez-la sur `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-190">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="6e5bd-191">Un code JSON similaire au suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-191">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="6e5bd-192">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="6e5bd-192">Add a model class</span></span>

<span data-ttu-id="6e5bd-193">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-193">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="6e5bd-194">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-194">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-196">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-196">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="6e5bd-197">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-197">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6e5bd-198">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-198">Name the folder *Models*.</span></span>

* <span data-ttu-id="6e5bd-199">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-199">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6e5bd-200">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-200">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="6e5bd-201">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-201">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6e5bd-203">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-203">Add a folder named *Models*.</span></span>

* <span data-ttu-id="6e5bd-204">Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-204">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-205">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e5bd-206">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-206">Right-click the project.</span></span> <span data-ttu-id="6e5bd-207">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6e5bd-208">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-208">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="6e5bd-210">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-210">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="6e5bd-211">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-211">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="6e5bd-212">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-212">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6e5bd-213">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-213">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="6e5bd-214">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-214">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="6e5bd-215">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6e5bd-215">Add a database context</span></span>

<span data-ttu-id="6e5bd-216">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-216">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="6e5bd-217">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-217">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-218">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="6e5bd-219">Ajouter Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="6e5bd-219">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="6e5bd-220">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-220">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="6e5bd-221">Cochez la case **Inclure la préversion**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-221">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="6e5bd-222">Sélectionnez l’onglet **Parcourir**, puis entrez **Microsoft.EntityFrameworkCore.SqlServer** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-222">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="6e5bd-223">Sélectionnez **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** dans le volet gauche.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-223">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="6e5bd-224">Cochez la case **Projet** dans le volet droit, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-224">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="6e5bd-225">Utilisez les instructions précédentes pour ajouter le package NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-225">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Package Manager](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="6e5bd-227">Ajouter le contexte de base de données TodoContext</span><span class="sxs-lookup"><span data-stu-id="6e5bd-227">Add the TodoContext database context</span></span>

* <span data-ttu-id="6e5bd-228">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-228">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6e5bd-229">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-229">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6e5bd-230">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-230">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6e5bd-231">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-231">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="6e5bd-232">Entrez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-232">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="6e5bd-233">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6e5bd-233">Register the database context</span></span>

<span data-ttu-id="6e5bd-234">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-234">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6e5bd-235">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-235">The container provides the service to controllers.</span></span>

<span data-ttu-id="6e5bd-236">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-236">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="6e5bd-237">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-237">The preceding code:</span></span>

* <span data-ttu-id="6e5bd-238">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-238">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="6e5bd-239">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-239">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="6e5bd-240">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-240">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="6e5bd-241">Générer automatiquement des modèles pour un contrôleur</span><span class="sxs-lookup"><span data-stu-id="6e5bd-241">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-242">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-243">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-243">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="6e5bd-244">Sélectionnez **Ajouter** > **Nouvel élément généré automatiquement**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-244">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="6e5bd-245">Sélectionnez **Contrôleur d’API avec actions, utilisant Entity Framework**, puis **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-245">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="6e5bd-246">Dans la boîte de dialogue **Contrôleur d’API avec actions, utilisant Entity Framework** :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-246">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="6e5bd-247">Sélectionnez **TodoItem (TodoAPI.Models)** dans **Classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-247">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="6e5bd-248">Sélectionnez **TodoContext (TodoAPI.Models)** dans **Classe du contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-248">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="6e5bd-249">Sélectionnez **Ajouter**</span><span class="sxs-lookup"><span data-stu-id="6e5bd-249">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6e5bd-250">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-250">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6e5bd-251">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-251">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="6e5bd-252">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-252">The preceding commands:</span></span>

* <span data-ttu-id="6e5bd-253">Ajoutent les packages NuGet nécessaires à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-253">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="6e5bd-254">Installent le moteur de génération de modèles automatique (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-254">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="6e5bd-255">Génèrent automatiquement des modèles pour `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-255">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="6e5bd-256">Le code généré :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-256">The generated code:</span></span>

* <span data-ttu-id="6e5bd-257">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-257">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="6e5bd-258">Décore la classe avec l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-258">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="6e5bd-259">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-259">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="6e5bd-260">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-260">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="6e5bd-261">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-261">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="6e5bd-262">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-262">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="6e5bd-263">Examiner la méthode de création de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-263">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="6e5bd-264">Remplacez l’instruction return dans `PostTodoItem` pour utiliser l’opérateur [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-264">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="6e5bd-265">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-265">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6e5bd-266">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-266">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="6e5bd-267">La méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-267">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="6e5bd-268">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-268">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="6e5bd-269">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-269">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="6e5bd-270">Ajoute un en-tête [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) à la réponse.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-270">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="6e5bd-271">L’en-tête `Location` spécifie l’[URI](https://developer.mozilla.org/docs/Glossary/URI) de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-271">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="6e5bd-272">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-272">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="6e5bd-273">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-273">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="6e5bd-274">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-274">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="6e5bd-275">Installer Postman</span><span class="sxs-lookup"><span data-stu-id="6e5bd-275">Install Postman</span></span>

<span data-ttu-id="6e5bd-276">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-276">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="6e5bd-277">Installez [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-277">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="6e5bd-278">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-278">Start the web app.</span></span>
* <span data-ttu-id="6e5bd-279">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-279">Start Postman.</span></span>
* <span data-ttu-id="6e5bd-280">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-280">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="6e5bd-281">À partir de **Fichier > Paramètres** (onglet \**Général*), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-281">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="6e5bd-282">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-282">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="6e5bd-283">Tester PostTodoItem avec Postman</span><span class="sxs-lookup"><span data-stu-id="6e5bd-283">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="6e5bd-284">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-284">Create a new request.</span></span>
* <span data-ttu-id="6e5bd-285">Affectez `POST` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-285">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="6e5bd-286">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-286">Select the **Body** tab.</span></span>
* <span data-ttu-id="6e5bd-287">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-287">Select the **raw** radio button.</span></span>
* <span data-ttu-id="6e5bd-288">Définissez le type sur **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="6e5bd-288">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="6e5bd-289">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-289">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="6e5bd-290">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-290">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="6e5bd-292">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="6e5bd-292">Test the location header URI</span></span>

* <span data-ttu-id="6e5bd-293">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-293">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="6e5bd-294">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-294">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="6e5bd-296">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-296">Set the method to GET.</span></span>
* <span data-ttu-id="6e5bd-297">Collez l’URI (par exemple, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-297">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="6e5bd-298">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-298">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="6e5bd-299">Examiner les méthodes GET</span><span class="sxs-lookup"><span data-stu-id="6e5bd-299">Examine the GET methods</span></span>

<span data-ttu-id="6e5bd-300">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-300">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="6e5bd-301">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur ou de Postman.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-301">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="6e5bd-302">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-302">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="6e5bd-303">Une réponse semblable à la suivante est produite par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-303">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="6e5bd-304">Tester Get avec Postman</span><span class="sxs-lookup"><span data-stu-id="6e5bd-304">Test Get with Postman</span></span>

* <span data-ttu-id="6e5bd-305">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-305">Create a new request.</span></span>
* <span data-ttu-id="6e5bd-306">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-306">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="6e5bd-307">Définissez l’URL de la requête sur `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-307">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="6e5bd-308">Par exemple, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-308">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="6e5bd-309">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-309">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="6e5bd-310">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-310">Select **Send**.</span></span>

<span data-ttu-id="6e5bd-311">Cette application utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-311">This app uses an in-memory database.</span></span> <span data-ttu-id="6e5bd-312">Si l’application est arrêtée et démarrée, la requête GET précédente ne retourne aucune donnée.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-312">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="6e5bd-313">Si aucune donnée n’est retournée, publiez ([POST](#post)) les données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-313">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="6e5bd-314">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="6e5bd-314">Routing and URL paths</span></span>

<span data-ttu-id="6e5bd-315">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-315">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="6e5bd-316">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-316">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="6e5bd-317">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-317">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="6e5bd-318">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="6e5bd-318">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="6e5bd-319">Pour cet exemple, le nom de la classe du contrôleur étant **TodoItems**Controller, le nom du contrôleur est « TodoItems ».</span><span class="sxs-lookup"><span data-stu-id="6e5bd-319">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="6e5bd-320">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-320">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="6e5bd-321">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-321">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="6e5bd-322">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-322">This sample doesn't use a template.</span></span> <span data-ttu-id="6e5bd-323">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-323">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="6e5bd-324">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-324">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="6e5bd-325">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-325">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="6e5bd-326">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="6e5bd-326">Return values</span></span>

<span data-ttu-id="6e5bd-327">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-327">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="6e5bd-328">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-328">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="6e5bd-329">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-329">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="6e5bd-330">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-330">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="6e5bd-331">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-331">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="6e5bd-332">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-332">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="6e5bd-333">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-333">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="6e5bd-334">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-334">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="6e5bd-335">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-335">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="6e5bd-336">Méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-336">The PutTodoItem method</span></span>

<span data-ttu-id="6e5bd-337">Examinez la méthode `PutTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-337">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="6e5bd-338">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-338">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="6e5bd-339">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-339">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="6e5bd-340">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-340">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="6e5bd-341">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-341">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="6e5bd-342">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-342">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="6e5bd-343">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-343">Test the PutTodoItem method</span></span>

<span data-ttu-id="6e5bd-344">Cet exemple utilise une base de données en mémoire qui doit être initialisée à chaque démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-344">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="6e5bd-345">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-345">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="6e5bd-346">Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-346">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="6e5bd-347">Mettez à jour la tâche dont l’ID = 1 et nommez-la « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-347">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="6e5bd-348">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-348">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="6e5bd-350">Méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-350">The DeleteTodoItem method</span></span>

<span data-ttu-id="6e5bd-351">Examinez la méthode `DeleteTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-351">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="6e5bd-352">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-352">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="6e5bd-353">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="6e5bd-354">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="6e5bd-355">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="6e5bd-356">Définissez l’URI de l’objet à supprimer, par exemple `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-356">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="6e5bd-357">Sélectionnez **Send**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-357">Select **Send**</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="6e5bd-358">Appelez l’API web avec JavaScript</span><span class="sxs-lookup"><span data-stu-id="6e5bd-358">Call the web API with JavaScript</span></span>

<span data-ttu-id="6e5bd-359">Consultez [le didacticiel: Appeler une API web ASP.NET Core avec JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-359">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6e5bd-360">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-360">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6e5bd-361">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-361">Create a web API project.</span></span>
> * <span data-ttu-id="6e5bd-362">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-362">Add a model class and a database context.</span></span>
> * <span data-ttu-id="6e5bd-363">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="6e5bd-363">Add a controller.</span></span>
> * <span data-ttu-id="6e5bd-364">Ajouter les méthodes CRUD</span><span class="sxs-lookup"><span data-stu-id="6e5bd-364">Add CRUD methods.</span></span>
> * <span data-ttu-id="6e5bd-365">Configurer le routage et les chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="6e5bd-365">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="6e5bd-366">Spécifier des valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="6e5bd-366">Specify return values.</span></span>
> * <span data-ttu-id="6e5bd-367">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="6e5bd-367">Call the web API with Postman.</span></span>
> * <span data-ttu-id="6e5bd-368">Appeler l’API web avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-368">Call the web API with JavaScript.</span></span>

<span data-ttu-id="6e5bd-369">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-369">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="6e5bd-370">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="6e5bd-370">Overview</span></span>

<span data-ttu-id="6e5bd-371">Ce didacticiel crée l’API suivante :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-371">This tutorial creates the following API:</span></span>

|<span data-ttu-id="6e5bd-372">API</span><span class="sxs-lookup"><span data-stu-id="6e5bd-372">API</span></span> | <span data-ttu-id="6e5bd-373">Description</span><span class="sxs-lookup"><span data-stu-id="6e5bd-373">Description</span></span> | <span data-ttu-id="6e5bd-374">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="6e5bd-374">Request body</span></span> | <span data-ttu-id="6e5bd-375">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="6e5bd-375">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="6e5bd-376">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6e5bd-376">GET /api/TodoItems</span></span> | <span data-ttu-id="6e5bd-377">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="6e5bd-377">Get all to-do items</span></span> | <span data-ttu-id="6e5bd-378">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-378">None</span></span> | <span data-ttu-id="6e5bd-379">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="6e5bd-379">Array of to-do items</span></span>|
|<span data-ttu-id="6e5bd-380">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6e5bd-380">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="6e5bd-381">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="6e5bd-381">Get an item by ID</span></span> | <span data-ttu-id="6e5bd-382">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-382">None</span></span> | <span data-ttu-id="6e5bd-383">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-383">To-do item</span></span>|
|<span data-ttu-id="6e5bd-384">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="6e5bd-384">POST /api/TodoItems</span></span> | <span data-ttu-id="6e5bd-385">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="6e5bd-385">Add a new item</span></span> | <span data-ttu-id="6e5bd-386">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-386">To-do item</span></span> | <span data-ttu-id="6e5bd-387">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-387">To-do item</span></span> |
|<span data-ttu-id="6e5bd-388">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="6e5bd-388">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="6e5bd-389">Mettre à jour un élément existant &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6e5bd-389">Update an existing item &nbsp;</span></span> | <span data-ttu-id="6e5bd-390">Tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-390">To-do item</span></span> | <span data-ttu-id="6e5bd-391">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-391">None</span></span> |
|<span data-ttu-id="6e5bd-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6e5bd-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="6e5bd-393">Supprimer un élément &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="6e5bd-393">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="6e5bd-394">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-394">None</span></span> | <span data-ttu-id="6e5bd-395">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-395">None</span></span>|

<span data-ttu-id="6e5bd-396">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-396">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="6e5bd-402">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6e5bd-402">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-403">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-404">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-404">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-405">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-405">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="6e5bd-406">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="6e5bd-406">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-407">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-407">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-408">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-408">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="6e5bd-409">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-409">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="6e5bd-410">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-410">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="6e5bd-411">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 2.2** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-411">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="6e5bd-412">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-412">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="6e5bd-413">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-413">**Don't** select **Enable Docker Support**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-415">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-415">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6e5bd-416">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-416">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6e5bd-417">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-417">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="6e5bd-418">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-418">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="6e5bd-419">Ces commandes créent un projet d’API web et ouvrent une nouvelle instance de Visual Studio Code dans le nouveau dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-419">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="6e5bd-420">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-420">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-421">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-421">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e5bd-422">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-422">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="6e5bd-424">Sélectionnez **.NET Core** > **Application** > **API** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-424">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="6e5bd-426">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut \* *.NET Core 2.2* pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-426">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="6e5bd-427">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-427">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="6e5bd-429">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="6e5bd-429">Test the API</span></span>

<span data-ttu-id="6e5bd-430">Le modèle de projet crée une API `values`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-430">The project template creates a `values` API.</span></span> <span data-ttu-id="6e5bd-431">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-431">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-432">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-432">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6e5bd-433">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-433">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6e5bd-434">Visual Studio lance un navigateur et accède à `https://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-434">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="6e5bd-435">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-435">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="6e5bd-436">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-436">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-437">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6e5bd-438">Appuyez sur Ctrl+F5 pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-438">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="6e5bd-439">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-439">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-440">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6e5bd-441">Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-441">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="6e5bd-442">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-442">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="6e5bd-443">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-443">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="6e5bd-444">Ajoutez `/api/values` à l’URL (définissez-la sur `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-444">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="6e5bd-445">Le code JSON suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-445">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="6e5bd-446">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="6e5bd-446">Add a model class</span></span>

<span data-ttu-id="6e5bd-447">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-447">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="6e5bd-448">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-448">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-449">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-449">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-450">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-450">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="6e5bd-451">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-451">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6e5bd-452">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-452">Name the folder *Models*.</span></span>

* <span data-ttu-id="6e5bd-453">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-453">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6e5bd-454">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-454">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="6e5bd-455">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-455">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6e5bd-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6e5bd-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6e5bd-457">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-457">Add a folder named *Models*.</span></span>

* <span data-ttu-id="6e5bd-458">Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-458">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6e5bd-459">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6e5bd-460">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-460">Right-click the project.</span></span> <span data-ttu-id="6e5bd-461">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-461">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="6e5bd-462">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-462">Name the folder *Models*.</span></span>

  ![nouveau dossier](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="6e5bd-464">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-464">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="6e5bd-465">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-465">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="6e5bd-466">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-466">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="6e5bd-467">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-467">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="6e5bd-468">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-468">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="6e5bd-469">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6e5bd-469">Add a database context</span></span>

<span data-ttu-id="6e5bd-470">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-470">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="6e5bd-471">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-471">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-472">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-472">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-473">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-473">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6e5bd-474">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-474">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6e5bd-475">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-475">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6e5bd-476">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-476">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="6e5bd-477">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-477">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="6e5bd-478">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="6e5bd-478">Register the database context</span></span>

<span data-ttu-id="6e5bd-479">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du [conteneur d’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-479">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6e5bd-480">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-480">The container provides the service to controllers.</span></span>

<span data-ttu-id="6e5bd-481">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-481">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="6e5bd-482">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-482">The preceding code:</span></span>

* <span data-ttu-id="6e5bd-483">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-483">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="6e5bd-484">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-484">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="6e5bd-485">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-485">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="6e5bd-486">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="6e5bd-486">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-488">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-488">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="6e5bd-489">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-489">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="6e5bd-490">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-490">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="6e5bd-491">Nommez la classe *TodoController* et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-491">Name the class *TodoController*, and select **Add**.</span></span>

  ![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6e5bd-493">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6e5bd-494">Dans le dossier *Controllers*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-494">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="6e5bd-495">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="6e5bd-496">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-496">The preceding code:</span></span>

* <span data-ttu-id="6e5bd-497">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-497">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="6e5bd-498">Décore la classe avec l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-498">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="6e5bd-499">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-499">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="6e5bd-500">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-500">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="6e5bd-501">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-501">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="6e5bd-502">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-502">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="6e5bd-503">Ajoute un élément nommé `Item1` à la base de données si celle-ci est vide.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-503">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="6e5bd-504">Ce code se trouvant dans le constructeur, il s’exécute chaque fois qu’une nouvelle requête HTTP existe.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-504">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="6e5bd-505">Si vous supprimez tous les éléments, le constructeur recrée `Item1` au prochain appel d’une méthode d’API.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-505">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="6e5bd-506">Ainsi, il peut vous sembler à tort que la suppression n’a pas fonctionné.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-506">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="6e5bd-507">Ajouter des méthodes Get</span><span class="sxs-lookup"><span data-stu-id="6e5bd-507">Add Get methods</span></span>

<span data-ttu-id="6e5bd-508">Pour fournir une API qui récupère les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-508">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="6e5bd-509">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-509">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="6e5bd-510">Arrêtez l’application si elle est toujours en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-510">Stop the app if it's still running.</span></span> <span data-ttu-id="6e5bd-511">Ensuite, réexécutez-la pour inclure les dernières modifications.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-511">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="6e5bd-512">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-512">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="6e5bd-513">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-513">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="6e5bd-514">La réponse HTTP suivante est générée par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-514">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="6e5bd-515">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="6e5bd-515">Routing and URL paths</span></span>

<span data-ttu-id="6e5bd-516">L’attribut [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-516">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="6e5bd-517">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-517">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="6e5bd-518">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-518">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="6e5bd-519">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="6e5bd-519">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="6e5bd-520">Pour cet exemple, le nom de classe du contrôleur étant **Todo**Controller, le nom du contrôleur est « todo ».</span><span class="sxs-lookup"><span data-stu-id="6e5bd-520">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="6e5bd-521">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-521">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="6e5bd-522">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-522">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="6e5bd-523">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-523">This sample doesn't use a template.</span></span> <span data-ttu-id="6e5bd-524">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-524">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="6e5bd-525">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-525">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="6e5bd-526">Quand `GetTodoItem` est appelée, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-526">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="6e5bd-527">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="6e5bd-527">Return values</span></span>

<span data-ttu-id="6e5bd-528">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-528">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="6e5bd-529">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-529">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="6e5bd-530">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-530">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="6e5bd-531">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-531">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="6e5bd-532">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-532">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="6e5bd-533">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-533">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="6e5bd-534">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 [introuvable](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-534">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="6e5bd-535">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-535">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="6e5bd-536">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-536">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="6e5bd-537">Tester la méthode GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="6e5bd-537">Test the GetTodoItems method</span></span>

<span data-ttu-id="6e5bd-538">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-538">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="6e5bd-539">Installez [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-539">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="6e5bd-540">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-540">Start the web app.</span></span>
* <span data-ttu-id="6e5bd-541">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-541">Start Postman.</span></span>
* <span data-ttu-id="6e5bd-542">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-542">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6e5bd-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e5bd-543">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6e5bd-544">À partir de **Fichier** > **Paramètres** (onglet **Général**), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-544">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6e5bd-545">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6e5bd-545">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6e5bd-546">À partir de **Postman** > **Préférences** (onglet **Général**), désactivez **Vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-546">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="6e5bd-547">Vous pouvez également sélectionner la clé et sélectionner **Paramètres**, puis désactiver la vérification du certificat SSL.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-547">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="6e5bd-548">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-548">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="6e5bd-549">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-549">Create a new request.</span></span>
  * <span data-ttu-id="6e5bd-550">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-550">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="6e5bd-551">Définissez l’URL de la requête sur `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-551">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="6e5bd-552">Par exemple, `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-552">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="6e5bd-553">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-553">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="6e5bd-554">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-554">Select **Send**.</span></span>

![Postman avec requête Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="6e5bd-556">Ajouter une méthode Create</span><span class="sxs-lookup"><span data-stu-id="6e5bd-556">Add a Create method</span></span>

<span data-ttu-id="6e5bd-557">Ajoutez la méthode `PostTodoItem` suivante à *Controllers/TodoController.cs* :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-557">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="6e5bd-558">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-558">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="6e5bd-559">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-559">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="6e5bd-560">La méthode `CreatedAtAction` :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-560">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="6e5bd-561">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-561">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="6e5bd-562">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="6e5bd-563">Ajoute un en-tête `Location` à la réponse.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-563">Adds a `Location` header to the response.</span></span> <span data-ttu-id="6e5bd-564">L’en-tête `Location` spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-564">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="6e5bd-565">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="6e5bd-566">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="6e5bd-567">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="6e5bd-568">Tester la méthode PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-568">Test the PostTodoItem method</span></span>

* <span data-ttu-id="6e5bd-569">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-569">Build the project.</span></span>
* <span data-ttu-id="6e5bd-570">Dans Postman, définissez la méthode HTTP sur `POST`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-570">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="6e5bd-571">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-571">Select the **Body** tab.</span></span>
* <span data-ttu-id="6e5bd-572">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-572">Select the **raw** radio button.</span></span>
* <span data-ttu-id="6e5bd-573">Définissez le type sur **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="6e5bd-573">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="6e5bd-574">Dans le corps de la demande, entrez la syntaxe JSON d’une tâche :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-574">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="6e5bd-575">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-575">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/create.png)

  <span data-ttu-id="6e5bd-577">Si vous obtenez une erreur 405 Méthode non autorisée, il est probable que le projet n’ait pas été compilé après l’ajout de la méthode `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-577">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="6e5bd-578">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="6e5bd-578">Test the location header URI</span></span>

* <span data-ttu-id="6e5bd-579">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-579">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="6e5bd-580">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-580">Copy the **Location** header value:</span></span>

  ![Onglet Headers de la console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="6e5bd-582">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-582">Set the method to GET.</span></span>
* <span data-ttu-id="6e5bd-583">Collez l’URI (par exemple, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-583">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="6e5bd-584">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-584">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="6e5bd-585">Ajouter une méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-585">Add a PutTodoItem method</span></span>

<span data-ttu-id="6e5bd-586">Ajoutez la méthode `PutTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-586">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="6e5bd-587">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-587">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="6e5bd-588">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-588">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="6e5bd-589">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-589">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="6e5bd-590">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-590">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="6e5bd-591">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-591">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="6e5bd-592">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-592">Test the PutTodoItem method</span></span>

<span data-ttu-id="6e5bd-593">Cet exemple utilise une base de données en mémoire qui doit être initialisée à chaque démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-593">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="6e5bd-594">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-594">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="6e5bd-595">Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-595">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="6e5bd-596">Mettez à jour la tâche dont l’ID = 1 et nommez-la « feed fish » :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-596">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="6e5bd-597">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-597">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="6e5bd-599">Ajouter une méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-599">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="6e5bd-600">Ajoutez la méthode `DeleteTodoItem` suivante :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-600">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="6e5bd-601">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-601">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="6e5bd-602">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="6e5bd-602">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="6e5bd-603">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-603">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="6e5bd-604">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-604">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="6e5bd-605">Définissez l’URI de l’objet à supprimer, par exemple `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-605">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="6e5bd-606">Sélectionnez **Send**.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-606">Select **Send**</span></span>

<span data-ttu-id="6e5bd-607">L’exemple d’application vous permet de supprimer tous les éléments.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-607">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="6e5bd-608">Toutefois, quand le dernier élément est supprimé, un autre est créé par le constructeur de classe de modèle au prochain appel de l’API.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-608">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="6e5bd-609">Appelez l’API web avec JavaScript</span><span class="sxs-lookup"><span data-stu-id="6e5bd-609">Call the web API with JavaScript</span></span>

<span data-ttu-id="6e5bd-610">Dans cette section, une page HTML qui utilise JavaScript pour appeler l’API web est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-610">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="6e5bd-611">L’API Fetch lance la demande.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-611">The Fetch API initiates the request.</span></span> <span data-ttu-id="6e5bd-612">JavaScript met à jour la page avec les détails de la réponse de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-612">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="6e5bd-613">Configurez l’application pour [traiter les fichiers statiques](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [activer le mappage de fichier par défaut](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) en mettant à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-613">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="6e5bd-614">Créez un dossier *wwwroot* dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-614">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="6e5bd-615">Ajoutez un fichier HTML nommé *index.html* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-615">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="6e5bd-616">Remplacez son contenu par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-616">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="6e5bd-617">Ajoutez un fichier JavaScript nommé *site.js* au répertoire *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-617">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="6e5bd-618">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-618">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="6e5bd-619">Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-619">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="6e5bd-620">Ouvrez *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-620">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="6e5bd-621">Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-621">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="6e5bd-622">Cet exemple appelle toutes les méthodes CRUD de l’API web.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-622">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="6e5bd-623">Les explications suivantes traitent des appels à l’API.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="6e5bd-624">Obtenir une liste de tâches</span><span class="sxs-lookup"><span data-stu-id="6e5bd-624">Get a list of to-do items</span></span>

<span data-ttu-id="6e5bd-625">FETCH envoie une requête HTTP GET à l’API web, qui retourne du code JSON représentant un tableau de tâches.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-625">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="6e5bd-626">La fonction de rappel `success` est appelée si la requête réussit.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="6e5bd-627">Dans le rappel, le DOM est mis à jour avec les informations des tâches.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="6e5bd-628">Ajouter une tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-628">Add a to-do item</span></span>

<span data-ttu-id="6e5bd-629">Fetch envoie une requête HTTP POST dont le corps indique la tâche.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-629">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="6e5bd-630">Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour spécifier le type de média qui est reçu et envoyé.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="6e5bd-631">La tâche est convertie au format JSON à l’aide de [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="6e5bd-632">Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="6e5bd-633">Mettre à jour une tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-633">Update a to-do item</span></span>

<span data-ttu-id="6e5bd-634">La mise à jour d’une tâche est similaire à l’ajout d’une tâche.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="6e5bd-635">L’identificateur unique de la tâche est ajouté à l’`url` et le `type` est `PUT`.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="6e5bd-636">Supprimer une tâche</span><span class="sxs-lookup"><span data-stu-id="6e5bd-636">Delete a to-do item</span></span>

<span data-ttu-id="6e5bd-637">Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="6e5bd-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="6e5bd-638">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6e5bd-638">Additional resources</span></span>

<span data-ttu-id="6e5bd-639">[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="6e5bd-640">Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="6e5bd-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="6e5bd-641">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e5bd-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="6e5bd-642">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="6e5bd-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
