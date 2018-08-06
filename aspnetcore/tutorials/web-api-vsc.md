---
title: Créer une API web avec ASP.NET Core et Visual Studio Code
author: rick-anderson
description: Générer une API web sur macOS, Linux ou Windows avec ASP.NET Core MVC et Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4ce808ec4241ab2fc3c2fb81c3fdb15dd853cd90
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342274"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="21ee9-103">Créer une API web avec ASP.NET Core et Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="21ee9-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="21ee9-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="21ee9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="21ee9-105">Dans ce didacticiel, créez une API web pour la gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="21ee9-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="21ee9-106">Une interface utilisateur n’est pas construite.</span><span class="sxs-lookup"><span data-stu-id="21ee9-106">A UI isn't constructed.</span></span>

<span data-ttu-id="21ee9-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="21ee9-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="21ee9-108">macOS, Linux, Windows : API web avec Visual Studio Code (le présent didacticiel)</span><span class="sxs-lookup"><span data-stu-id="21ee9-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="21ee9-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="21ee9-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="21ee9-110">Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="21ee9-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="21ee9-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="21ee9-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="21ee9-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="21ee9-112">Create the project</span></span>

<span data-ttu-id="21ee9-113">À partir d’une console, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="21ee9-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="21ee9-114">Le dossier *TodoApi* s’ouvre dans Visual Studio Code (VS Code).</span><span class="sxs-lookup"><span data-stu-id="21ee9-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="21ee9-115">Sélectionnez le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="21ee9-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="21ee9-116">Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="21ee9-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="21ee9-117">Faut-il les ajouter ? »</span><span class="sxs-lookup"><span data-stu-id="21ee9-117">Add them?"</span></span>
* <span data-ttu-id="21ee9-118">Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="21ee9-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="21ee9-122">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="21ee9-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="21ee9-123">Dans un navigateur, accédez à http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="21ee9-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="21ee9-124">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="21ee9-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="21ee9-125">Pour obtenir des conseils sur l’utilisation de VS Code, consultez [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="21ee9-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="21ee9-126">Ajouter la prise en charge d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="21ee9-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="21ee9-127">La création d’un projet dans ASP.NET Core 2.1 ou version ultérieure ajoute la référence de package [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) au fichier *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="21ee9-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file.</span></span> <span data-ttu-id="21ee9-128">Ajouter l’attribut `Version` s’il n’est pas déjà spécifié.</span><span class="sxs-lookup"><span data-stu-id="21ee9-128">Add the `Version` attribute, if not already specified.</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="21ee9-129">La création d’un projet dans ASP.NET Core 2.0 ajoute la référence de package [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) au fichier *TodoApi.csproj* :</span><span class="sxs-lookup"><span data-stu-id="21ee9-129">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="21ee9-130">Il est inutile d’installer le fournisseur de base de données [Entity Framework Core InMemory](/ef/core/providers/in-memory/) séparément.</span><span class="sxs-lookup"><span data-stu-id="21ee9-130">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="21ee9-131">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="21ee9-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="21ee9-132">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="21ee9-132">Add a model class</span></span>

<span data-ttu-id="21ee9-133">Un modèle est un objet qui représente les données de votre application.</span><span class="sxs-lookup"><span data-stu-id="21ee9-133">A model is an object representing the data in your app.</span></span> <span data-ttu-id="21ee9-134">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="21ee9-134">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="21ee9-135">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="21ee9-135">Add a folder named *Models*.</span></span> <span data-ttu-id="21ee9-136">Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="21ee9-136">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="21ee9-137">Ajouter une classe `TodoItem` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="21ee9-137">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="21ee9-138">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="21ee9-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="21ee9-139">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="21ee9-139">Create the database context</span></span>

<span data-ttu-id="21ee9-140">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="21ee9-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="21ee9-141">Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="21ee9-141">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="21ee9-142">Ajoutez une classe `TodoContext` dans le dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="21ee9-142">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="21ee9-143">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="21ee9-143">Add a controller</span></span>

<span data-ttu-id="21ee9-144">Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="21ee9-144">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="21ee9-145">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="21ee9-145">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="21ee9-146">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="21ee9-146">Launch the app</span></span>

<span data-ttu-id="21ee9-147">Dans VS Code, appuyez sur F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="21ee9-147">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="21ee9-148">Accédez à http://localhost:5000/api/todo (le contrôleur `Todo` que nous avons créé).</span><span class="sxs-lookup"><span data-stu-id="21ee9-148">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="21ee9-149">Aide de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="21ee9-149">Visual Studio Code help</span></span>

* [<span data-ttu-id="21ee9-150">Prise en main</span><span class="sxs-lookup"><span data-stu-id="21ee9-150">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="21ee9-151">Débogage</span><span class="sxs-lookup"><span data-stu-id="21ee9-151">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="21ee9-152">Terminal intégré</span><span class="sxs-lookup"><span data-stu-id="21ee9-152">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="21ee9-153">Raccourcis clavier</span><span class="sxs-lookup"><span data-stu-id="21ee9-153">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="21ee9-154">Raccourcis clavier macOS</span><span class="sxs-lookup"><span data-stu-id="21ee9-154">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="21ee9-155">Raccourcis clavier Linux</span><span class="sxs-lookup"><span data-stu-id="21ee9-155">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="21ee9-156">Raccourcis clavier Windows</span><span class="sxs-lookup"><span data-stu-id="21ee9-156">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
