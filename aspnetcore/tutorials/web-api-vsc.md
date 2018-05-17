---
title: Créer une API web avec ASP.NET Core et Visual Studio Code
author: rick-anderson
description: Générer une API web sur macOS, Linux ou Windows avec ASP.NET Core MVC et Visual Studio Code
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 9fac4d7b3f687881eafbd63ee71f99bff3b27183
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="a3e7b-103">Créer une API web avec ASP.NET Core et Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3e7b-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="a3e7b-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a3e7b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a3e7b-105">Dans ce didacticiel, créez une API web pour la gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a3e7b-106">Une interface utilisateur n’est pas construite.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-106">A UI isn't constructed.</span></span>

<span data-ttu-id="a3e7b-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="a3e7b-108">macOS, Linux, Windows : API web avec Visual Studio Code (le présent didacticiel)</span><span class="sxs-lookup"><span data-stu-id="a3e7b-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="a3e7b-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="a3e7b-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="a3e7b-110">Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="a3e7b-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="a3e7b-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a3e7b-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="a3e7b-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="a3e7b-112">Create the project</span></span>

<span data-ttu-id="a3e7b-113">À partir d’une console, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="a3e7b-114">Le dossier *TodoApi* s’ouvre dans Visual Studio Code (VS Code).</span><span class="sxs-lookup"><span data-stu-id="a3e7b-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="a3e7b-115">Sélectionnez le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="a3e7b-116">Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="a3e7b-117">Faut-il les ajouter ? »</span><span class="sxs-lookup"><span data-stu-id="a3e7b-117">Add them?"</span></span>
* <span data-ttu-id="a3e7b-118">Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="a3e7b-122">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="a3e7b-123">Dans un navigateur, accédez à http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="a3e7b-124">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="a3e7b-125">Pour obtenir des conseils sur l’utilisation de VS Code, consultez [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="a3e7b-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="a3e7b-126">Ajouter la prise en charge d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a3e7b-126">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a3e7b-127">La création d’un projet dans ASP.NET Core 2.0 ajoute la référence de package [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) au fichier *TodoApi.csproj* :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-127">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="a3e7b-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="a3e7b-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a3e7b-129">La création d’un projet dans ASP.NET Core 2.1 ou version ultérieure ajoute la référence de package [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) au fichier *TodoApi.csproj* :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-129">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="a3e7b-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="a3e7b-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="a3e7b-131">Il est inutile d’installer le fournisseur de base de données [Entity Framework Core InMemory](/ef/core/providers/in-memory/) séparément.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-131">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="a3e7b-132">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="a3e7b-133">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="a3e7b-133">Add a model class</span></span>

<span data-ttu-id="a3e7b-134">Un modèle est un objet qui représente les données de votre application.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="a3e7b-135">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a3e7b-136">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-136">Add a folder named *Models*.</span></span> <span data-ttu-id="a3e7b-137">Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="a3e7b-138">Ajouter une classe `TodoItem` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a3e7b-139">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="a3e7b-140">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="a3e7b-140">Create the database context</span></span>

<span data-ttu-id="a3e7b-141">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a3e7b-142">Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a3e7b-143">Ajoutez une classe `TodoContext` dans le dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="a3e7b-144">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="a3e7b-144">Add a controller</span></span>

<span data-ttu-id="a3e7b-145">Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="a3e7b-146">Remplacez son contenu par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a3e7b-146">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a3e7b-147">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="a3e7b-147">Launch the app</span></span>

<span data-ttu-id="a3e7b-148">Dans VS Code, appuyez sur F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="a3e7b-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="a3e7b-149">Accédez à http://localhost:5000/api/todo (le contrôleur `Todo` que nous avons créé).</span><span class="sxs-lookup"><span data-stu-id="a3e7b-149">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="a3e7b-150">Aide de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a3e7b-150">Visual Studio Code help</span></span>

* [<span data-ttu-id="a3e7b-151">Prise en main</span><span class="sxs-lookup"><span data-stu-id="a3e7b-151">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="a3e7b-152">Débogage</span><span class="sxs-lookup"><span data-stu-id="a3e7b-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="a3e7b-153">Terminal intégré</span><span class="sxs-lookup"><span data-stu-id="a3e7b-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="a3e7b-154">Raccourcis clavier</span><span class="sxs-lookup"><span data-stu-id="a3e7b-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="a3e7b-155">Raccourcis clavier macOS</span><span class="sxs-lookup"><span data-stu-id="a3e7b-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="a3e7b-156">Raccourcis clavier Linux</span><span class="sxs-lookup"><span data-stu-id="a3e7b-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="a3e7b-157">Raccourcis clavier Windows</span><span class="sxs-lookup"><span data-stu-id="a3e7b-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
