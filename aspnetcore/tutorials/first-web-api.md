---
title: "Créer une API web avec ASP.NET Core et Visual Studio pour Windows"
author: rick-anderson
description: "Générer une API web avec ASP.NET Core MVC et Visual Studio pour Windows"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1146132693681eca8f92beb00ebabd7296534688
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="34845-103">Créer une API web avec ASP.NET Core et Visual Studio pour Windows</span><span class="sxs-lookup"><span data-stu-id="34845-103">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="34845-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="34845-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="34845-105">Ce didacticiel génère une API web de gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="34845-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="34845-106">Aucune interface utilisateur n’est créée.</span><span class="sxs-lookup"><span data-stu-id="34845-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="34845-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="34845-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="34845-108">Windows : API web avec Visual Studio pour Windows (ce didacticiel)</span><span class="sxs-lookup"><span data-stu-id="34845-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="34845-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="34845-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="34845-110">macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="34845-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="34845-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="34845-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="34845-112">Consultez [ce fichier PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) pour la version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="34845-112">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="34845-113">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="34845-113">Create the project</span></span>

<span data-ttu-id="34845-114">Dans Visual Studio, sélectionnez **Fichier** Menu > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="34845-114">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="34845-115">Sélectionnez le modèle de projet **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="34845-115">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="34845-116">Nommez le projet `TodoApi` et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="34845-116">Name the project `TodoApi` and select **OK**.</span></span>

![Boîte de dialogue Nouveau projet](first-web-api/_static/new-project.png)

<span data-ttu-id="34845-118">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, sélectionnez le modèle **API web**.</span><span class="sxs-lookup"><span data-stu-id="34845-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="34845-119">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="34845-119">Select **OK**.</span></span> <span data-ttu-id="34845-120">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="34845-120">Do **not** select **Enable Docker Support**.</span></span>

![Boîte de dialogue Nouvelle application web ASP.NET avec modèle de projet API web sélectionné parmi les modèles ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="34845-122">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="34845-122">Launch the app</span></span>

<span data-ttu-id="34845-123">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="34845-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="34845-124">Visual Studio lance un navigateur et accède à `http://localhost:port/api/values`, où *port* est un numéro de port choisi au hasard.</span><span class="sxs-lookup"><span data-stu-id="34845-124">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="34845-125">Chrome, Microsoft Edge et Firefox affichent la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="34845-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="34845-126">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="34845-126">Add a model class</span></span>

<span data-ttu-id="34845-127">Un modèle est un objet qui représente les données de l’application.</span><span class="sxs-lookup"><span data-stu-id="34845-127">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="34845-128">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="34845-128">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="34845-129">Ajoutez un dossier nommé « Models ».</span><span class="sxs-lookup"><span data-stu-id="34845-129">Add a folder named "Models".</span></span> <span data-ttu-id="34845-130">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="34845-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="34845-131">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="34845-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="34845-132">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="34845-132">Name the folder *Models*.</span></span>

<span data-ttu-id="34845-133">Remarque : Les classes de modèles vont n’importe où dans le projet.</span><span class="sxs-lookup"><span data-stu-id="34845-133">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="34845-134">Le dossier *Modèles* est utilisé par convention pour les classes de modèles.</span><span class="sxs-lookup"><span data-stu-id="34845-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="34845-135">Ajoutez une classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="34845-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="34845-136">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="34845-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="34845-137">Nommez la classe `TodoItem` et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="34845-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="34845-138">Ajoutez le code suivant à la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="34845-138">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="34845-139">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="34845-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="34845-140">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="34845-140">Create the database context</span></span>

<span data-ttu-id="34845-141">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="34845-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="34845-142">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="34845-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="34845-143">Ajoutez une classe `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="34845-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="34845-144">Cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="34845-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="34845-145">Nommez la classe `TodoContext` et sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="34845-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="34845-146">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="34845-146">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="34845-147">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="34845-147">Add a controller</span></span>

<span data-ttu-id="34845-148">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="34845-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="34845-149">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="34845-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="34845-150">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur des API web**.</span><span class="sxs-lookup"><span data-stu-id="34845-150">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="34845-151">Nommez la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="34845-151">Name the class `TodoController`.</span></span>

![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

<span data-ttu-id="34845-153">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="34845-153">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="34845-154">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="34845-154">Launch the app</span></span>

<span data-ttu-id="34845-155">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="34845-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="34845-156">Visual Studio lance un navigateur et accède à `http://localhost:port/api/values`, où *port* est un numéro de port choisi au hasard.</span><span class="sxs-lookup"><span data-stu-id="34845-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="34845-157">Accédez au contrôleur `Todo` à l’adresse `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="34845-157">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

