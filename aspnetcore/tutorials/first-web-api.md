---
title: 'Didacticiel : créer une API Web avec ASP.NET Core'
author: rick-anderson
description: Apprendre à créer une API web avec ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541853"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="dd508-103">Didacticiel : créer une API Web avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd508-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="dd508-104">De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="dd508-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="dd508-105">Ce tutoriel décrit les principes fondamentaux liés à la génération d’une API web avec ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dd508-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="dd508-106">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="dd508-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dd508-107">Créer un projet d’API web.</span><span class="sxs-lookup"><span data-stu-id="dd508-107">Create a web API project.</span></span>
> * <span data-ttu-id="dd508-108">Ajouter une classe de modèle et un contexte de base de données.</span><span class="sxs-lookup"><span data-stu-id="dd508-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="dd508-109">Générer automatiquement des modèles pour un contrôleur avec des méthodes CRUD.</span><span class="sxs-lookup"><span data-stu-id="dd508-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="dd508-110">Configurer le routage, les chemins d’URL et les valeurs de retour.</span><span class="sxs-lookup"><span data-stu-id="dd508-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="dd508-111">Appeler l’API web avec Postman</span><span class="sxs-lookup"><span data-stu-id="dd508-111">Call the web API with Postman.</span></span>

<span data-ttu-id="dd508-112">À la fin, vous disposez d’une API web qui peut gérer des tâches stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="dd508-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="dd508-113">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="dd508-113">Overview</span></span>

<span data-ttu-id="dd508-114">Ce didacticiel crée une API Web contenant les points de terminaison suivants :</span><span class="sxs-lookup"><span data-stu-id="dd508-114">This tutorial creates a web API containing the following endpoints:</span></span>

|<span data-ttu-id="dd508-115">Point de terminaison</span><span class="sxs-lookup"><span data-stu-id="dd508-115">Endpoint</span></span>                  |<span data-ttu-id="dd508-116">Description</span><span class="sxs-lookup"><span data-stu-id="dd508-116">Description</span></span>            |<span data-ttu-id="dd508-117">Corps de la requête</span><span class="sxs-lookup"><span data-stu-id="dd508-117">Request body</span></span>|<span data-ttu-id="dd508-118">Corps de réponse</span><span class="sxs-lookup"><span data-stu-id="dd508-118">Response body</span></span>       |
|--------------------------|-----------------------|------------|--------------------|
|<span data-ttu-id="dd508-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="dd508-119">GET /api/TodoItems</span></span>        |<span data-ttu-id="dd508-120">Obtenir toutes les tâches</span><span class="sxs-lookup"><span data-stu-id="dd508-120">Get all to-do items</span></span>    |<span data-ttu-id="dd508-121">aucune.</span><span class="sxs-lookup"><span data-stu-id="dd508-121">None</span></span>        |<span data-ttu-id="dd508-122">Tableau de tâches</span><span class="sxs-lookup"><span data-stu-id="dd508-122">Array of to-do items</span></span>|
|<span data-ttu-id="dd508-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="dd508-123">GET /api/TodoItems/{id}</span></span>   |<span data-ttu-id="dd508-124">Obtenir un élément par ID</span><span class="sxs-lookup"><span data-stu-id="dd508-124">Get an item by ID</span></span>      |<span data-ttu-id="dd508-125">aucune.</span><span class="sxs-lookup"><span data-stu-id="dd508-125">None</span></span>        |<span data-ttu-id="dd508-126">Tâche</span><span class="sxs-lookup"><span data-stu-id="dd508-126">To-do item</span></span>          |
|<span data-ttu-id="dd508-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="dd508-127">POST /api/TodoItems</span></span>       |<span data-ttu-id="dd508-128">Ajouter un nouvel élément</span><span class="sxs-lookup"><span data-stu-id="dd508-128">Add a new item</span></span>         |<span data-ttu-id="dd508-129">Tâche</span><span class="sxs-lookup"><span data-stu-id="dd508-129">To-do item</span></span>  |<span data-ttu-id="dd508-130">Tâche</span><span class="sxs-lookup"><span data-stu-id="dd508-130">To-do item</span></span>          |
|<span data-ttu-id="dd508-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="dd508-131">PUT /api/TodoItems/{id}</span></span>   |<span data-ttu-id="dd508-132">Mettre à jour un élément existant</span><span class="sxs-lookup"><span data-stu-id="dd508-132">Update an existing item</span></span>|<span data-ttu-id="dd508-133">Tâche</span><span class="sxs-lookup"><span data-stu-id="dd508-133">To-do item</span></span>  |<span data-ttu-id="dd508-134">aucune.</span><span class="sxs-lookup"><span data-stu-id="dd508-134">None</span></span>                |
|<span data-ttu-id="dd508-135">SUPPRIMER/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="dd508-135">DELETE /api/TodoItems/{id}</span></span>|<span data-ttu-id="dd508-136">Supprimer un élément</span><span class="sxs-lookup"><span data-stu-id="dd508-136">Delete an item</span></span>         |<span data-ttu-id="dd508-137">aucune.</span><span class="sxs-lookup"><span data-stu-id="dd508-137">None</span></span>        |<span data-ttu-id="dd508-138">aucune.</span><span class="sxs-lookup"><span data-stu-id="dd508-138">None</span></span>                |

<span data-ttu-id="dd508-139">Le diagramme suivant illustre la conception de l’application.</span><span class="sxs-lookup"><span data-stu-id="dd508-139">The following diagram shows the design of the app.</span></span>

![Le client est représenté par une zone située à gauche.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="dd508-145">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="dd508-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dd508-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd508-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dd508-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dd508-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dd508-148">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dd508-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="dd508-149">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="dd508-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dd508-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd508-150">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="dd508-151">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="dd508-151">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="dd508-152">Sélectionnez le modèle **Application web ASP.NET Core** et cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="dd508-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
1. <span data-ttu-id="dd508-153">Nommez le projet *TodoApi* et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="dd508-153">Name the project *TodoApi* and click **Create**.</span></span>
1. <span data-ttu-id="dd508-154">Dans la boîte de dialogue **Créer une application web ASP.NET Core**, vérifiez que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="dd508-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="dd508-155">Sélectionnez le modèle **API** et cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="dd508-155">Select the **API** template and click **Create**.</span></span>

![Boîte de dialogue de nouveau projet dans VS](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dd508-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dd508-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="dd508-158">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dd508-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
1. <span data-ttu-id="dd508-159">Définissez les répertoires (`cd`) sur le dossier destiné à contenir le dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="dd508-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
1. <span data-ttu-id="dd508-160">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dd508-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. <span data-ttu-id="dd508-161">Quand une boîte de dialogue vous demande si vous souhaitez ajouter les composants nécessaires au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="dd508-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="dd508-162">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="dd508-162">The preceding commands:</span></span>

  * <span data-ttu-id="dd508-163">Créez un projet d’API Web et ouvrez-le dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dd508-163">Create a new web API project and open it in Visual Studio Code.</span></span>
  * <span data-ttu-id="dd508-164">Ajoutez les packages NuGet qui sont requis dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="dd508-164">Add the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dd508-165">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dd508-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="dd508-166">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="dd508-166">Select **File** > **New Solution**.</span></span>

    ![macOS - Nouvelle solution](first-web-api-mac/_static/sln.png)

1. <span data-ttu-id="dd508-168">Sélectionnez **.NET Core** > **Application** > **API** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="dd508-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

    ![macOS - Boîte de dialogue Nouveau projet](first-web-api-mac/_static/1.png)
  
1. <span data-ttu-id="dd508-170">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, sélectionnez *.NET Core 3.0* comme **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="dd508-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>
1. <span data-ttu-id="dd508-171">Entrez *TodoApi* comme **Nom du projet**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="dd508-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![boîte de dialogue de configuration](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="dd508-173">Ouvrez un terminal de commande dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dd508-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="dd508-174">Tester l’API</span><span class="sxs-lookup"><span data-stu-id="dd508-174">Test the API</span></span>

<span data-ttu-id="dd508-175">Le modèle de projet crée une API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="dd508-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="dd508-176">Appelez la méthode `Get` à partir d’un navigateur pour tester l’application.</span><span class="sxs-lookup"><span data-stu-id="dd508-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dd508-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd508-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dd508-178">Appuyez sur <kbd>CTRL + F5</kbd> pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="dd508-178">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="dd508-179">Visual Studio lance un navigateur et accède à `https://localhost:<port>/WeatherForecast`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="dd508-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="dd508-180">Si une boîte de dialogue apparaît vous demandant si vous devez approuver le certificat IIS Express, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="dd508-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="dd508-181">Dans la boîte de dialogue **Avertissement de sécurité** qui s’affiche ensuite, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="dd508-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dd508-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dd508-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="dd508-183">Appuyez sur <kbd>CTRL + F5</kbd> pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="dd508-183">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="dd508-184">Dans un navigateur, accédez à l’URL suivante : [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="dd508-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dd508-185">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dd508-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dd508-186">Sélectionnez **Exécuter** > **Démarrer le débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="dd508-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="dd508-187">Visual Studio pour Mac lance un navigateur et accède à `https://localhost:<port>`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="dd508-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="dd508-188">Une erreur HTTP 404 (introuvable) est retournée.</span><span class="sxs-lookup"><span data-stu-id="dd508-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="dd508-189">Ajoutez `/WeatherForecast` à l’URL (définissez-la sur `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="dd508-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="dd508-190">Un code JSON similaire au suivant est retourné :</span><span class="sxs-lookup"><span data-stu-id="dd508-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="dd508-191">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="dd508-191">Add a model class</span></span>

<span data-ttu-id="dd508-192">Un *modèle* est un ensemble de classes qui représentent les données gérées par l’application.</span><span class="sxs-lookup"><span data-stu-id="dd508-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="dd508-193">Le modèle pour cette application est une classe `TodoItem` unique.</span><span class="sxs-lookup"><span data-stu-id="dd508-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dd508-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd508-194">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="dd508-195">Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="dd508-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="dd508-196">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="dd508-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="dd508-197">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="dd508-197">Name the folder *Models*.</span></span>
1. <span data-ttu-id="dd508-198">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="dd508-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="dd508-199">Nommez la classe *TodoItem* et sélectionnez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="dd508-199">Name the class *TodoItem* and select **Add**.</span></span>
1. <span data-ttu-id="dd508-200">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd508-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dd508-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dd508-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="dd508-202">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="dd508-202">Add a folder named *Models*.</span></span>
1. <span data-ttu-id="dd508-203">Ajoutez une classe `TodoItem` au dossier *Models* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd508-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dd508-204">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dd508-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="dd508-205">Cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="dd508-205">Right-click the project.</span></span> <span data-ttu-id="dd508-206">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="dd508-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="dd508-207">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="dd508-207">Name the folder *Models*.</span></span>

    ![nouveau dossier](first-web-api-mac/_static/folder.png)

1. <span data-ttu-id="dd508-209">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Nouveau fichier** > **Général** > **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="dd508-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>
1. <span data-ttu-id="dd508-210">Nommez la classe *TodoItem* et cliquez sur **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="dd508-210">Name the class *TodoItem*, and then click **New**.</span></span>
1. <span data-ttu-id="dd508-211">Remplacez le code du modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd508-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="dd508-212">La propriété `Id` fonctionne comme la clé unique dans une base de données relationnelle.</span><span class="sxs-lookup"><span data-stu-id="dd508-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="dd508-213">Vous pouvez placer des classes de modèle n’importe où dans le projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="dd508-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="dd508-214">Ajouter un contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="dd508-214">Add a database context</span></span>

<span data-ttu-id="dd508-215">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données.</span><span class="sxs-lookup"><span data-stu-id="dd508-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="dd508-216">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="dd508-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dd508-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd508-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="dd508-218">Ajouter Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="dd508-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

1. <span data-ttu-id="dd508-219">Dans le menu **Outils**, sélectionnez **Gestionnaire de package NuGet > Gérer les packages NuGet pour la solution**.</span><span class="sxs-lookup"><span data-stu-id="dd508-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
1. <span data-ttu-id="dd508-220">Sélectionnez l’onglet **Parcourir**, puis entrez **Microsoft.EntityFrameworkCore.SqlServer** dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="dd508-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
1. <span data-ttu-id="dd508-221">Dans le volet gauche, sélectionnez **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="dd508-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
1. <span data-ttu-id="dd508-222">Cochez la case **Projet** dans le volet droit, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="dd508-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
1. <span data-ttu-id="dd508-223">Utilisez les instructions précédentes pour ajouter le package NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="dd508-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet Package Manager](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="dd508-225">Ajouter le contexte de base de données TodoContext</span><span class="sxs-lookup"><span data-stu-id="dd508-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="dd508-226">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="dd508-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="dd508-227">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="dd508-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="dd508-228">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dd508-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="dd508-229">Ajoutez une classe `TodoContext` au dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="dd508-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="dd508-230">Entrez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="dd508-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="dd508-231">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="dd508-231">Register the database context</span></span>

<span data-ttu-id="dd508-232">Dans ASP.NET Core, les services tels que le contexte de base de données doivent être inscrits auprès du conteneur d' [injection de dépendances (di)](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="dd508-232">In ASP.NET Core, services such as the database context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="dd508-233">Le conteneur fournit le service aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="dd508-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="dd508-234">Mettez à jour *Startup.cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="dd508-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="dd508-235">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="dd508-235">The preceding code:</span></span>

* <span data-ttu-id="dd508-236">Supprime les déclarations `using` inutilisées.</span><span class="sxs-lookup"><span data-stu-id="dd508-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="dd508-237">Ajoute le contexte de base de données au conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="dd508-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="dd508-238">Spécifie que le contexte de base de données utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="dd508-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="dd508-239">Générer automatiquement des modèles pour un contrôleur</span><span class="sxs-lookup"><span data-stu-id="dd508-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dd508-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd508-240">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="dd508-241">Cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="dd508-241">Right-click the *Controllers* folder.</span></span>
1. <span data-ttu-id="dd508-242">Sélectionnez **ajouter**  > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="dd508-242">Select **Add** > **New Scaffolded Item**.</span></span>
1. <span data-ttu-id="dd508-243">Sélectionnez **Contrôleur d’API avec actions, utilisant Entity Framework**, puis **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="dd508-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
1. <span data-ttu-id="dd508-244">Dans la boîte de dialogue **Contrôleur d’API avec actions, utilisant Entity Framework** :</span><span class="sxs-lookup"><span data-stu-id="dd508-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>
    * <span data-ttu-id="dd508-245">Sélectionnez **TodoItem (TodoApi. Models)** dans la **classe de modèle**.</span><span class="sxs-lookup"><span data-stu-id="dd508-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
    * <span data-ttu-id="dd508-246">Sélectionnez **TodoContext (TodoApi. Models)** dans la **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="dd508-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
    * <span data-ttu-id="dd508-247">Sélectionnez **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="dd508-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="dd508-248">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dd508-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="dd508-249">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dd508-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="dd508-250">Les commandes précédentes :</span><span class="sxs-lookup"><span data-stu-id="dd508-250">The preceding commands:</span></span>

* <span data-ttu-id="dd508-251">Ajoutent les packages NuGet nécessaires à la génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="dd508-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="dd508-252">Installent le moteur de génération de modèles automatique (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="dd508-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="dd508-253">Génèrent automatiquement des modèles pour `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="dd508-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="dd508-254">Le code généré :</span><span class="sxs-lookup"><span data-stu-id="dd508-254">The generated code:</span></span>

* <span data-ttu-id="dd508-255">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="dd508-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="dd508-256">Décore la classe avec l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute).</span><span class="sxs-lookup"><span data-stu-id="dd508-256">Decorates the class with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="dd508-257">Cet attribut indique que le contrôleur répond aux requêtes de l’API web.</span><span class="sxs-lookup"><span data-stu-id="dd508-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="dd508-258">Pour plus d’informations sur les comportements spécifiques que permet l’attribut, consultez <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="dd508-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="dd508-259">Utilise l’injection de dépendances pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="dd508-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="dd508-260">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="dd508-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="dd508-261">Examiner la méthode de création de PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="dd508-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="dd508-262">Remplacez l’instruction return dans `PostTodoItem` pour utiliser l’opérateur [nameof](/dotnet/csharp/language-reference/operators/nameof) :</span><span class="sxs-lookup"><span data-stu-id="dd508-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="dd508-263">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute).</span><span class="sxs-lookup"><span data-stu-id="dd508-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="dd508-264">La méthode obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd508-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="dd508-265">La méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> :</span><span class="sxs-lookup"><span data-stu-id="dd508-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="dd508-266">Retourne un code d’état HTTP 201 en cas de réussite.</span><span class="sxs-lookup"><span data-stu-id="dd508-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="dd508-267">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="dd508-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="dd508-268">Ajoute un en-tête [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) à la réponse.</span><span class="sxs-lookup"><span data-stu-id="dd508-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="dd508-269">L’en-tête `Location` spécifie l’[URI](https://developer.mozilla.org/docs/Glossary/URI) de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="dd508-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="dd508-270">Pour plus d’informations, consultez la section [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="dd508-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="dd508-271">Fait référence à l’action `GetTodoItem` pour créer l’URI `Location` de l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="dd508-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="dd508-272">Le mot clé `nameof` C# est utilisé pour éviter de coder en dur le nom de l’action dans l’appel `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="dd508-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="dd508-273">Installer Postman</span><span class="sxs-lookup"><span data-stu-id="dd508-273">Install Postman</span></span>

<span data-ttu-id="dd508-274">Ce tutoriel utilise Postman pour tester l’API web.</span><span class="sxs-lookup"><span data-stu-id="dd508-274">This tutorial uses Postman to test the web API.</span></span>

1. <span data-ttu-id="dd508-275">Installez [Postman](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="dd508-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
1. <span data-ttu-id="dd508-276">Démarrez l’application web.</span><span class="sxs-lookup"><span data-stu-id="dd508-276">Start the web app.</span></span>
1. <span data-ttu-id="dd508-277">Démarrez Postman.</span><span class="sxs-lookup"><span data-stu-id="dd508-277">Start Postman.</span></span>
1. <span data-ttu-id="dd508-278">Désactivez la **vérification du certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="dd508-278">Disable **SSL certificate verification**</span></span>
1. <span data-ttu-id="dd508-279">À partir de**paramètres** de  >  de **fichier** (onglet**général** ), désactivez la **vérification de certificat SSL**.</span><span class="sxs-lookup"><span data-stu-id="dd508-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="dd508-280">Réactivez la vérification du certificat SSL après avoir testé le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="dd508-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="dd508-281">Tester PostTodoItem avec Postman</span><span class="sxs-lookup"><span data-stu-id="dd508-281">Test PostTodoItem with Postman</span></span>

1. <span data-ttu-id="dd508-282">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="dd508-282">Create a new request.</span></span>
1. <span data-ttu-id="dd508-283">Affectez `POST` à la méthode HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd508-283">Set the HTTP method to `POST`.</span></span>
1. <span data-ttu-id="dd508-284">Sélectionnez l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="dd508-284">Select the **Body** tab.</span></span>
1. <span data-ttu-id="dd508-285">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="dd508-285">Select the **raw** radio button.</span></span>
1. <span data-ttu-id="dd508-286">Définissez le type sur **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="dd508-286">Set the type to **JSON (application/json)**.</span></span>
1. <span data-ttu-id="dd508-287">Dans le corps de la demande, entrez JSON pour un élément de tâche :</span><span class="sxs-lookup"><span data-stu-id="dd508-287">In the request body, enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. <span data-ttu-id="dd508-288">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="dd508-288">Select **Send**.</span></span>

  ![Postman avec requête de création](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="dd508-290">Tester l’URI de l’en-tête d’emplacement</span><span class="sxs-lookup"><span data-stu-id="dd508-290">Test the location header URI</span></span>

1. <span data-ttu-id="dd508-291">Sélectionnez l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse).</span><span class="sxs-lookup"><span data-stu-id="dd508-291">Select the **Headers** tab in the **Response** pane.</span></span>
1. <span data-ttu-id="dd508-292">Copiez la valeur d’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="dd508-292">Copy the **Location** header value:</span></span>

    ![Onglet Headers de la console Postman](first-web-api/_static/create.png)

1. <span data-ttu-id="dd508-294">Définissez la méthode sur GET.</span><span class="sxs-lookup"><span data-stu-id="dd508-294">Set the method to GET.</span></span>
1. <span data-ttu-id="dd508-295">Collez l’URI (par exemple, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="dd508-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="dd508-296">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="dd508-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="dd508-297">Examiner les méthodes GET</span><span class="sxs-lookup"><span data-stu-id="dd508-297">Examine the GET methods</span></span>

<span data-ttu-id="dd508-298">Ces méthodes implémentent deux points de terminaison GET :</span><span class="sxs-lookup"><span data-stu-id="dd508-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="dd508-299">Testez l’application en appelant les deux points de terminaison à partir d’un navigateur ou de Postman.</span><span class="sxs-lookup"><span data-stu-id="dd508-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="dd508-300">Exemple :</span><span class="sxs-lookup"><span data-stu-id="dd508-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="dd508-301">Une réponse semblable à la suivante est produite par l’appel à `GetTodoItems` :</span><span class="sxs-lookup"><span data-stu-id="dd508-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="dd508-302">Tester Get avec Postman</span><span class="sxs-lookup"><span data-stu-id="dd508-302">Test Get with Postman</span></span>

1. <span data-ttu-id="dd508-303">Créez une requête.</span><span class="sxs-lookup"><span data-stu-id="dd508-303">Create a new request.</span></span>
1. <span data-ttu-id="dd508-304">Définissez la méthode HTTP sur **GET**.</span><span class="sxs-lookup"><span data-stu-id="dd508-304">Set the HTTP method to **GET**.</span></span>
1. <span data-ttu-id="dd508-305">Définissez l’URL de la requête sur `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="dd508-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="dd508-306">Par exemple, `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="dd508-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
1. <span data-ttu-id="dd508-307">Définissez l’**affichage à deux volets** dans Postman.</span><span class="sxs-lookup"><span data-stu-id="dd508-307">Set **Two pane view** in Postman.</span></span>
1. <span data-ttu-id="dd508-308">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="dd508-308">Select **Send**.</span></span>

<span data-ttu-id="dd508-309">Cette application utilise une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="dd508-309">This app uses an in-memory database.</span></span> <span data-ttu-id="dd508-310">Si l’application est arrêtée et démarrée, la requête d’extraction précédente ne retourne pas de données.</span><span class="sxs-lookup"><span data-stu-id="dd508-310">If the app is stopped and started, the preceding GET request won't return any data.</span></span> <span data-ttu-id="dd508-311">Si aucune donnée n’est retournée, publiez ([POST](#post)) les données dans l’application.</span><span class="sxs-lookup"><span data-stu-id="dd508-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="dd508-312">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="dd508-312">Routing and URL paths</span></span>

<span data-ttu-id="dd508-313">L’attribut [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) désigne une méthode qui répond à une requête http.</span><span class="sxs-lookup"><span data-stu-id="dd508-313">The [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="dd508-314">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="dd508-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="dd508-315">Partez de la chaîne de modèle dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="dd508-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="dd508-316">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="dd508-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="dd508-317">Pour cet exemple, le nom de la classe du contrôleur étant **TodoItems**Controller, le nom du contrôleur est « TodoItems ».</span><span class="sxs-lookup"><span data-stu-id="dd508-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="dd508-318">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="dd508-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="dd508-319">Si l’attribut `[HttpGet]` a un modèle de route (par exemple, `[HttpGet("products")]`), ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="dd508-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="dd508-320">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="dd508-320">This sample doesn't use a template.</span></span> <span data-ttu-id="dd508-321">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="dd508-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="dd508-322">Dans la méthode `GetTodoItem` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="dd508-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="dd508-323">Lorsque `GetTodoItem` est appelé, la valeur de `"{id}"` dans l’URL est fournie à la méthode dans son paramètre `id`.</span><span class="sxs-lookup"><span data-stu-id="dd508-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="dd508-324">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="dd508-324">Return values</span></span>

<span data-ttu-id="dd508-325">Le type de retour des méthodes `GetTodoItems` et `GetTodoItem` est [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="dd508-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="dd508-326">ASP.NET Core sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="dd508-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="dd508-327">Le code de réponse pour ce type de retour est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="dd508-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="dd508-328">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="dd508-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="dd508-329">Les types de retour `ActionResult` peuvent représenter une large plage de codes d’état HTTP.</span><span class="sxs-lookup"><span data-stu-id="dd508-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="dd508-330">Par exemple, `GetTodoItem` peut retourner deux valeurs d’état différentes :</span><span class="sxs-lookup"><span data-stu-id="dd508-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="dd508-331">Si aucun élément ne correspond à l’ID demandé, la méthode retourne un code d’erreur 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound>.</span><span class="sxs-lookup"><span data-stu-id="dd508-331">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> error code.</span></span>
* <span data-ttu-id="dd508-332">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="dd508-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="dd508-333">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="dd508-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="dd508-334">Méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="dd508-334">The PutTodoItem method</span></span>

<span data-ttu-id="dd508-335">Examinez la méthode `PutTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="dd508-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="dd508-336">`PutTodoItem` est similaire à `PostTodoItem`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="dd508-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="dd508-337">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="dd508-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="dd508-338">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les changements.</span><span class="sxs-lookup"><span data-stu-id="dd508-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="dd508-339">Pour prendre en charge les mises à jour partielles, utilisez [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="dd508-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="dd508-340">Si vous obtenez une erreur en appelant `PutTodoItem`, appelez `GET` pour vérifier que la base de données contient un élément.</span><span class="sxs-lookup"><span data-stu-id="dd508-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="dd508-341">Tester la méthode PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="dd508-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="dd508-342">Cet exemple utilise une base de données en mémoire qui doit être initialisée chaque fois que l’application est démarrée.</span><span class="sxs-lookup"><span data-stu-id="dd508-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="dd508-343">La base de données doit contenir un élément avant que vous ne passiez un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="dd508-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="dd508-344">Appelez GET pour vérifier qu’un élément existe dans la base de données avant d’effectuer un appel PUT.</span><span class="sxs-lookup"><span data-stu-id="dd508-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="dd508-345">Mettez à jour l’élément de tâche dont l’ID est 1.</span><span class="sxs-lookup"><span data-stu-id="dd508-345">Update the to-do item that has an ID of 1.</span></span> <span data-ttu-id="dd508-346">Définissez son nom sur « flux de poisson » :</span><span class="sxs-lookup"><span data-stu-id="dd508-346">Set its name to "feed fish":</span></span>

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

<span data-ttu-id="dd508-347">L’image suivante montre la mise à jour Postman :</span><span class="sxs-lookup"><span data-stu-id="dd508-347">The following image shows the Postman update:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="dd508-349">Méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="dd508-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="dd508-350">Examinez la méthode `DeleteTodoItem` :</span><span class="sxs-lookup"><span data-stu-id="dd508-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="dd508-351">La réponse `DeleteTodoItem` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="dd508-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="dd508-352">Tester la méthode DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="dd508-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="dd508-353">Utilisez Postman pour supprimer une tâche :</span><span class="sxs-lookup"><span data-stu-id="dd508-353">Use Postman to delete a to-do item:</span></span>

1. <span data-ttu-id="dd508-354">Définissez la méthode sur `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="dd508-354">Set the method to `DELETE`.</span></span>
1. <span data-ttu-id="dd508-355">Définissez l’URI de l’objet à supprimer (par exemple, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="dd508-355">Set the URI of the object to delete (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="dd508-356">Sélectionnez **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="dd508-356">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="dd508-357">Appelez l’API web avec JavaScript</span><span class="sxs-lookup"><span data-stu-id="dd508-357">Call the web API with JavaScript</span></span>

<span data-ttu-id="dd508-358">Consultez [Didacticiel : appeler une API web ASP.net core avec JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="dd508-358">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="dd508-359">Ajouter la prise en charge de l’authentification à une API Web</span><span class="sxs-lookup"><span data-stu-id="dd508-359">Add authentication support to a web API</span></span>

<span data-ttu-id="dd508-360">Consultez le didacticiel [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .</span><span class="sxs-lookup"><span data-stu-id="dd508-360">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dd508-361">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dd508-361">Additional resources</span></span>

<span data-ttu-id="dd508-362">[Affichez ou téléchargez l’exemple de code de ce tutoriel](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="dd508-362">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="dd508-363">Consultez [Guide pratique pour télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="dd508-363">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="dd508-364">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="dd508-364">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="dd508-365">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="dd508-365">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
