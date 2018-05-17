---
title: Créer une API web avec ASP.NET Core et Visual Studio pour Windows
author: rick-anderson
description: Générer une API web avec ASP.NET Core MVC et Visual Studio pour Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: cb46f8b4013488dbe2bb5ca3d08a8c6e452141dd
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="1903a-103">Créer une API web avec ASP.NET Core et Visual Studio pour Windows</span><span class="sxs-lookup"><span data-stu-id="1903a-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="1903a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="1903a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="1903a-105">Ce didacticiel génère une API web de gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="1903a-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="1903a-106">Aucune interface utilisateur n’est créée.</span><span class="sxs-lookup"><span data-stu-id="1903a-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="1903a-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="1903a-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="1903a-108">Windows : API web avec Visual Studio pour Windows (ce didacticiel)</span><span class="sxs-lookup"><span data-stu-id="1903a-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="1903a-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="1903a-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="1903a-110">macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="1903a-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="1903a-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="1903a-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="1903a-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="1903a-112">Create the project</span></span>

<span data-ttu-id="1903a-113">Suivez ces étapes dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="1903a-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="1903a-114">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="1903a-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="1903a-115">Sélectionnez le modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1903a-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="1903a-116">Nommez le projet *TodoApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1903a-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="1903a-117">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, choisissez la version ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1903a-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="1903a-118">Sélectionnez le modèle **API** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="1903a-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="1903a-119">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="1903a-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="1903a-120">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="1903a-120">Launch the app</span></span>

<span data-ttu-id="1903a-121">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="1903a-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="1903a-122">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="1903a-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="1903a-123">Chrome, Microsoft Edge et Firefox affichent la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="1903a-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="1903a-124">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="1903a-124">Add a model class</span></span>

<span data-ttu-id="1903a-125">Un modèle est un objet qui représente les données de l’application.</span><span class="sxs-lookup"><span data-stu-id="1903a-125">A model is an object representing the data in the app.</span></span> <span data-ttu-id="1903a-126">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="1903a-126">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="1903a-127">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="1903a-127">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="1903a-128">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="1903a-128">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="1903a-129">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="1903a-129">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="1903a-130">Les classes de modèles vont n’importe où dans le projet.</span><span class="sxs-lookup"><span data-stu-id="1903a-130">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="1903a-131">Le dossier *Models* est utilisé par convention pour les classes de modèles.</span><span class="sxs-lookup"><span data-stu-id="1903a-131">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="1903a-132">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="1903a-132">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1903a-133">Nommez la classe *TodoItem* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="1903a-133">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="1903a-134">Ajoutez le code suivant à la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="1903a-134">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="1903a-135">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="1903a-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="1903a-136">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="1903a-136">Create the database context</span></span>

<span data-ttu-id="1903a-137">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="1903a-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="1903a-138">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1903a-138">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="1903a-139">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="1903a-139">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="1903a-140">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="1903a-140">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="1903a-141">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1903a-141">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="1903a-142">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="1903a-142">Add a controller</span></span>

<span data-ttu-id="1903a-143">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="1903a-143">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="1903a-144">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="1903a-144">Select **Add** > **New Item**.</span></span> <span data-ttu-id="1903a-145">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="1903a-145">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="1903a-146">Nommez la classe *TodoController* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="1903a-146">Name the class *TodoController*, and click **Add**.</span></span>

![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

<span data-ttu-id="1903a-148">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1903a-148">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="1903a-149">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="1903a-149">Launch the app</span></span>

<span data-ttu-id="1903a-150">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="1903a-150">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="1903a-151">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="1903a-151">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="1903a-152">Accédez au contrôleur `Todo` à l’adresse `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="1903a-152">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
