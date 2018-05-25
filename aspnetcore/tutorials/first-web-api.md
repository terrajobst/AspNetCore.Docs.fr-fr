---
title: Créer une API web avec ASP.NET Core et Visual Studio pour Windows
author: rick-anderson
description: Générer une API web avec ASP.NET Core MVC et Visual Studio pour Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1680d5e0be0f4844c904d923af30634c53bd1b81
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/18/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="31c55-103">Créer une API web avec ASP.NET Core et Visual Studio pour Windows</span><span class="sxs-lookup"><span data-stu-id="31c55-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="31c55-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="31c55-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="31c55-105">Ce didacticiel génère une API web de gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="31c55-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="31c55-106">Aucune interface utilisateur n’est créée.</span><span class="sxs-lookup"><span data-stu-id="31c55-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="31c55-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="31c55-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="31c55-108">Windows : API web avec Visual Studio pour Windows (ce didacticiel)</span><span class="sxs-lookup"><span data-stu-id="31c55-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="31c55-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="31c55-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="31c55-110">macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="31c55-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="31c55-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="31c55-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="31c55-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="31c55-112">Create the project</span></span>

<span data-ttu-id="31c55-113">Suivez ces étapes dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="31c55-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="31c55-114">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="31c55-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="31c55-115">Sélectionnez le modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="31c55-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="31c55-116">Nommez le projet *TodoApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="31c55-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="31c55-117">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, choisissez la version ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31c55-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="31c55-118">Sélectionnez le modèle **API** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="31c55-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="31c55-119">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="31c55-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="31c55-120">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="31c55-120">Launch the app</span></span>

<span data-ttu-id="31c55-121">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="31c55-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="31c55-122">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="31c55-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="31c55-123">Chrome, Microsoft Edge et Firefox affichent la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="31c55-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="31c55-124">Si vous utilisez Internet Explorer, vous êtes invité à enregistrer un fichier *values.json*.</span><span class="sxs-lookup"><span data-stu-id="31c55-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="31c55-125">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="31c55-125">Add a model class</span></span>

<span data-ttu-id="31c55-126">Un modèle est un objet qui représente les données de l’application.</span><span class="sxs-lookup"><span data-stu-id="31c55-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="31c55-127">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="31c55-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="31c55-128">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="31c55-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="31c55-129">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="31c55-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="31c55-130">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="31c55-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="31c55-131">Les classes de modèles vont n’importe où dans le projet.</span><span class="sxs-lookup"><span data-stu-id="31c55-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="31c55-132">Le dossier *Models* est utilisé par convention pour les classes de modèles.</span><span class="sxs-lookup"><span data-stu-id="31c55-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="31c55-133">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="31c55-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="31c55-134">Nommez la classe *TodoItem* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="31c55-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="31c55-135">Ajoutez le code suivant à la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="31c55-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="31c55-136">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="31c55-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="31c55-137">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="31c55-137">Create the database context</span></span>

<span data-ttu-id="31c55-138">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="31c55-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="31c55-139">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="31c55-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="31c55-140">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="31c55-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="31c55-141">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="31c55-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="31c55-142">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="31c55-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="31c55-143">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="31c55-143">Add a controller</span></span>

<span data-ttu-id="31c55-144">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="31c55-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="31c55-145">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="31c55-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="31c55-146">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="31c55-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="31c55-147">Nommez la classe *TodoController* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="31c55-147">Name the class *TodoController*, and click **Add**.</span></span>

![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

<span data-ttu-id="31c55-149">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="31c55-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="31c55-150">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="31c55-150">Launch the app</span></span>

<span data-ttu-id="31c55-151">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="31c55-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="31c55-152">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="31c55-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="31c55-153">Accédez au contrôleur `Todo` à l’adresse `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="31c55-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
