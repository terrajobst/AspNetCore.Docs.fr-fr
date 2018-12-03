---
title: Créer une API web avec ASP.NET Core et Visual Studio
author: rick-anderson
description: Générer une API web avec ASP.NET Core MVC et Visual Studio sur Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450695"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="638a6-103">Créer une API web avec ASP.NET Core et Visual Studio</span><span class="sxs-lookup"><span data-stu-id="638a6-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="638a6-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="638a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="638a6-105">Ce didacticiel génère une API web de gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="638a6-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="638a6-106">Aucune interface utilisateur n’est créée.</span><span class="sxs-lookup"><span data-stu-id="638a6-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="638a6-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="638a6-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="638a6-108">Windows : API web avec Visual Studio sur Windows (ce tutoriel, voir la [version vidéo](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="638a6-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="638a6-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="638a6-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="638a6-110">macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="638a6-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="638a6-111">Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="638a6-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="638a6-112">Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="638a6-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="638a6-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="638a6-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="638a6-114">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="638a6-114">Create the project</span></span>

<span data-ttu-id="638a6-115">Suivez ces étapes dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="638a6-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="638a6-116">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="638a6-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="638a6-117">Sélectionnez le modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="638a6-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="638a6-118">Nommez le projet *TodoApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="638a6-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="638a6-119">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, choisissez la version ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="638a6-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="638a6-120">Sélectionnez le modèle **API** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="638a6-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="638a6-121">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="638a6-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="638a6-122">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="638a6-122">Launch the app</span></span>

<span data-ttu-id="638a6-123">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="638a6-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="638a6-124">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="638a6-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="638a6-125">Chrome, Microsoft Edge et Firefox affichent la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="638a6-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="638a6-126">Si vous utilisez Internet Explorer, vous êtes invité à enregistrer un fichier *values.json*.</span><span class="sxs-lookup"><span data-stu-id="638a6-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="638a6-127">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="638a6-127">Add a model class</span></span>

<span data-ttu-id="638a6-128">Un modèle est un objet qui représente les données de l’application.</span><span class="sxs-lookup"><span data-stu-id="638a6-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="638a6-129">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="638a6-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="638a6-130">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="638a6-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="638a6-131">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="638a6-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="638a6-132">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="638a6-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="638a6-133">Les classes de modèles vont n’importe où dans le projet.</span><span class="sxs-lookup"><span data-stu-id="638a6-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="638a6-134">Le dossier *Models* est utilisé par convention pour les classes de modèles.</span><span class="sxs-lookup"><span data-stu-id="638a6-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="638a6-135">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="638a6-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="638a6-136">Nommez la classe *TodoItem* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="638a6-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="638a6-137">Ajoutez le code suivant à la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="638a6-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="638a6-138">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="638a6-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="638a6-139">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="638a6-139">Create the database context</span></span>

<span data-ttu-id="638a6-140">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="638a6-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="638a6-141">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="638a6-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="638a6-142">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="638a6-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="638a6-143">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="638a6-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="638a6-144">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="638a6-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="638a6-145">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="638a6-145">Add a controller</span></span>

<span data-ttu-id="638a6-146">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="638a6-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="638a6-147">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="638a6-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="638a6-148">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="638a6-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="638a6-149">Nommez la classe *TodoController* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="638a6-149">Name the class *TodoController*, and click **Add**.</span></span>

![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

<span data-ttu-id="638a6-151">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="638a6-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="638a6-152">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="638a6-152">Launch the app</span></span>

<span data-ttu-id="638a6-153">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="638a6-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="638a6-154">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="638a6-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="638a6-155">Accédez au contrôleur `Todo` à l’adresse `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="638a6-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
