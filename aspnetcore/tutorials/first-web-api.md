---
title: Créer une API web avec ASP.NET Core et Visual Studio pour Windows
author: rick-anderson
description: Générer une API web avec ASP.NET Core MVC et Visual Studio pour Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 3da22cbbbe0db7771656997a13587521e182fb2a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277398"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="36397-103">Créer une API web avec ASP.NET Core et Visual Studio pour Windows</span><span class="sxs-lookup"><span data-stu-id="36397-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="36397-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="36397-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="36397-105">Ce didacticiel génère une API web de gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="36397-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="36397-106">Aucune interface utilisateur n’est créée.</span><span class="sxs-lookup"><span data-stu-id="36397-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="36397-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="36397-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="36397-108">Windows : API web avec Visual Studio pour Windows (ce didacticiel)</span><span class="sxs-lookup"><span data-stu-id="36397-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="36397-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="36397-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="36397-110">macOS, Linux, Windows : [API web avec Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="36397-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="36397-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="36397-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="36397-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="36397-112">Create the project</span></span>

<span data-ttu-id="36397-113">Suivez ces étapes dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="36397-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="36397-114">Dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="36397-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="36397-115">Sélectionnez le modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="36397-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="36397-116">Nommez le projet *TodoApi*, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="36397-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="36397-117">Dans la boîte de dialogue **Nouvelle application web ASP.NET Core - TodoApi**, choisissez la version ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="36397-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="36397-118">Sélectionnez le modèle **API** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="36397-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="36397-119">Ne sélectionnez **pas** **Activer la prise en charge de Docker**.</span><span class="sxs-lookup"><span data-stu-id="36397-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="36397-120">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="36397-120">Launch the app</span></span>

<span data-ttu-id="36397-121">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="36397-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="36397-122">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="36397-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="36397-123">Chrome, Microsoft Edge et Firefox affichent la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="36397-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="36397-124">Si vous utilisez Internet Explorer, vous êtes invité à enregistrer un fichier *values.json*.</span><span class="sxs-lookup"><span data-stu-id="36397-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="36397-125">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="36397-125">Add a model class</span></span>

<span data-ttu-id="36397-126">Un modèle est un objet qui représente les données de l’application.</span><span class="sxs-lookup"><span data-stu-id="36397-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="36397-127">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="36397-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="36397-128">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet.</span><span class="sxs-lookup"><span data-stu-id="36397-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="36397-129">Sélectionnez **Ajouter** > **Nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="36397-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="36397-130">Nommez le dossier *Models*.</span><span class="sxs-lookup"><span data-stu-id="36397-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="36397-131">Les classes de modèles vont n’importe où dans le projet.</span><span class="sxs-lookup"><span data-stu-id="36397-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="36397-132">Le dossier *Models* est utilisé par convention pour les classes de modèles.</span><span class="sxs-lookup"><span data-stu-id="36397-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="36397-133">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="36397-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="36397-134">Nommez la classe *TodoItem* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="36397-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="36397-135">Ajoutez le code suivant à la classe `TodoItem` :</span><span class="sxs-lookup"><span data-stu-id="36397-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="36397-136">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="36397-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="36397-137">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="36397-137">Create the database context</span></span>

<span data-ttu-id="36397-138">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="36397-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="36397-139">Cette classe est créée en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="36397-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="36397-140">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Models* et sélectionnez **Ajouter** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="36397-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="36397-141">Nommez la classe *TodoContext* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="36397-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="36397-142">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="36397-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="36397-143">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="36397-143">Add a controller</span></span>

<span data-ttu-id="36397-144">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier *Contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="36397-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="36397-145">Sélectionnez **Ajouter** > **Nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="36397-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="36397-146">Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez le modèle **Classe de contrôleur d’API**.</span><span class="sxs-lookup"><span data-stu-id="36397-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="36397-147">Nommez la classe *TodoController* et cliquez sur **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="36397-147">Name the class *TodoController*, and click **Add**.</span></span>

![Boîte de dialogue Ajouter un nouvel élément avec contrôleur dans la zone de recherche et le contrôleur des API web sélectionné](first-web-api/_static/new_controller.png)

<span data-ttu-id="36397-149">Remplacez la classe par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="36397-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="36397-150">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="36397-150">Launch the app</span></span>

<span data-ttu-id="36397-151">Dans Visual Studio, appuyez sur Ctrl+F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="36397-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="36397-152">Visual Studio lance un navigateur et accède à `http://localhost:<port>/api/values`, où `<port>` est un numéro de port choisi de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="36397-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="36397-153">Accédez au contrôleur `Todo` à l’adresse `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="36397-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
