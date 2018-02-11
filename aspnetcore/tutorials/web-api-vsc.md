---
title: "Créer une API web avec ASP.NET Core et VS Code"
author: rick-anderson
description: "Générer une API web sur macOS, Linux ou Windows avec ASP.NET Core MVC et Visual Studio Code"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="c4ac5-103">Créer une API web avec ASP.NET Core MVC et Visual Studio Code sur Linux, macOS et Windows</span><span class="sxs-lookup"><span data-stu-id="c4ac5-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="c4ac5-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="c4ac5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="c4ac5-105">Dans ce didacticiel, créez une API web pour la gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="c4ac5-106">Une interface utilisateur n’est pas construite.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-106">A UI isn't constructed.</span></span>

<span data-ttu-id="c4ac5-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="c4ac5-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="c4ac5-108">macOS, Linux, Windows : API web avec Visual Studio Code (le présent didacticiel)</span><span class="sxs-lookup"><span data-stu-id="c4ac5-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="c4ac5-109">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="c4ac5-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="c4ac5-110">Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="c4ac5-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="c4ac5-111">Configurer votre environnement de développement</span><span class="sxs-lookup"><span data-stu-id="c4ac5-111">Set up your development environment</span></span>

<span data-ttu-id="c4ac5-112">Téléchargez et installez :</span><span class="sxs-lookup"><span data-stu-id="c4ac5-112">Download and install:</span></span>
- <span data-ttu-id="c4ac5-113">[SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="c4ac5-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="c4ac5-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4ac5-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="c4ac5-115">[Extension C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4ac5-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="c4ac5-116">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="c4ac5-116">Create the project</span></span>

<span data-ttu-id="c4ac5-117">À partir d’une console, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="c4ac5-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="c4ac5-118">Ouvrez le dossier *TodoApi* dans Visual Studio Code (VS Code), puis sélectionnez le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="c4ac5-119">Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="c4ac5-120">Faut-il les ajouter ? »</span><span class="sxs-lookup"><span data-stu-id="c4ac5-120">Add them?"</span></span>
- <span data-ttu-id="c4ac5-121">Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="c4ac5-125">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="c4ac5-126">Dans un navigateur, accédez à http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="c4ac5-127">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="c4ac5-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="c4ac5-128">Pour obtenir des conseils sur l’utilisation de VS Code, consultez [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="c4ac5-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="c4ac5-129">Ajouter la prise en charge d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c4ac5-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="c4ac5-130">La création d’un projet dans .NET Core 2.0 ajoute le fournisseur 'Microsoft.AspNetCore.All' au fichier *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="c4ac5-131">Il est inutile d’installer le fournisseur de base de données [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) séparément.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="c4ac5-132">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="c4ac5-133">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="c4ac5-133">Add a model class</span></span>

<span data-ttu-id="c4ac5-134">Un modèle est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="c4ac5-135">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="c4ac5-136">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-136">Add a folder named *Models*.</span></span> <span data-ttu-id="c4ac5-137">Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="c4ac5-138">Ajouter une classe `TodoItem` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c4ac5-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="c4ac5-139">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="c4ac5-140">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="c4ac5-140">Create the database context</span></span>

<span data-ttu-id="c4ac5-141">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="c4ac5-142">Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="c4ac5-143">Ajoutez une classe `TodoContext` dans le dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="c4ac5-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="c4ac5-144">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="c4ac5-144">Add a controller</span></span>

<span data-ttu-id="c4ac5-145">Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="c4ac5-146">Ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="c4ac5-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="c4ac5-147">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="c4ac5-147">Launch the app</span></span>

<span data-ttu-id="c4ac5-148">Dans VS Code, appuyez sur F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="c4ac5-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="c4ac5-149">Accédez à http://localhost:5000/api/todo (le contrôleur `Todo` que nous venons de créer).</span><span class="sxs-lookup"><span data-stu-id="c4ac5-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="c4ac5-150">Aide de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c4ac5-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="c4ac5-151">Prise en main</span><span class="sxs-lookup"><span data-stu-id="c4ac5-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="c4ac5-152">Débogage</span><span class="sxs-lookup"><span data-stu-id="c4ac5-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="c4ac5-153">Terminal intégré</span><span class="sxs-lookup"><span data-stu-id="c4ac5-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="c4ac5-154">Raccourcis clavier</span><span class="sxs-lookup"><span data-stu-id="c4ac5-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="c4ac5-155">Raccourcis clavier Mac</span><span class="sxs-lookup"><span data-stu-id="c4ac5-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="c4ac5-156">Raccourcis clavier Linux</span><span class="sxs-lookup"><span data-stu-id="c4ac5-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="c4ac5-157">Raccourcis clavier Windows</span><span class="sxs-lookup"><span data-stu-id="c4ac5-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


