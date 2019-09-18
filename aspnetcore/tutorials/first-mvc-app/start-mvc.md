---
title: Bien démarrer avec ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 52d8fc631075408991d3a2e62cdaccf681c5927f
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082160"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="ec6ec-103">Bien démarrer avec ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="ec6ec-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="ec6ec-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ec6ec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ec6ec-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ec6ec-106">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="ec6ec-107">Vous allez apprendre à effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ec6ec-108">Créer une application web.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-108">Create a web app.</span></span>
> * <span data-ttu-id="ec6ec-109">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="ec6ec-110">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-110">Work with a database.</span></span>
> * <span data-ttu-id="ec6ec-111">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-111">Add search and validation.</span></span>

<span data-ttu-id="ec6ec-112">À la fin, vous obtenez une application qui peut gérer et afficher des données de films.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="ec6ec-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ec6ec-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec6ec-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec6ec-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec6ec-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec6ec-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ec6ec-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ec6ec-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="ec6ec-117">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="ec6ec-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec6ec-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec6ec-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ec6ec-119">Dans Visual Studio, sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="ec6ec-120">Sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nouvelle application web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="ec6ec-122">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="ec6ec-123">Il est important de nommer le projet **MvcMovie** pour que l’espace de noms corresponde quand vous copiez du code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nouvelle application web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="ec6ec-125">Sélectionnez **Application web (modèle-vue-contrôleur)** , puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="ec6ec-126">Boîte de dialogue Nouveau projet, .NET Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec6ec-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="ec6ec-127">Visual Studio a utilisé le modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="ec6ec-128">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ec6ec-129">Il s’agit d’un projet de démarrage de base.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec6ec-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec6ec-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ec6ec-131">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ec6ec-132">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="ec6ec-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="ec6ec-133">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ec6ec-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ec6ec-134">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ec6ec-135">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-135">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="ec6ec-136">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="ec6ec-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="ec6ec-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-137">Select **Yes**</span></span>

  * <span data-ttu-id="ec6ec-138">`dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="ec6ec-139">`code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ec6ec-140">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ec6ec-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ec6ec-141">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-141">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ec6ec-143">Sélectionnez **.NET Core** > **Application** > **Application web (modèle-vue-contrôleur)** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="ec6ec-145">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, définissez **.NET Core 3.0** comme **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="ec6ec-146">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="ec6ec-147">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="ec6ec-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec6ec-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec6ec-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ec6ec-149">Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="ec6ec-150">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ec6ec-151">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ec6ec-152">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ec6ec-153">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="ec6ec-154">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ec6ec-155">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ec6ec-156">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ec6ec-158">Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="ec6ec-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

  <span data-ttu-id="ec6ec-160">L’image suivante montre l’application :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-160">The following image shows the app:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec6ec-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec6ec-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ec6ec-163">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="ec6ec-164">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="ec6ec-165">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="ec6ec-166">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ec6ec-167">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="ec6ec-168">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ec6ec-169">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ec6ec-171">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ec6ec-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ec6ec-172">Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-172">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ec6ec-173">Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-173">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ec6ec-174">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-174">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ec6ec-175">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-175">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ec6ec-176">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-176">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ec6ec-177">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-177">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ec6ec-178">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-178">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="ec6ec-179">L’image suivante montre l’application :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-179">The following image shows the app:</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="ec6ec-181">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-181">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ec6ec-182">Next</span><span class="sxs-lookup"><span data-stu-id="ec6ec-182">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="ec6ec-183">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-183">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="ec6ec-184">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-184">The app manages a database of movie titles.</span></span> <span data-ttu-id="ec6ec-185">Vous allez apprendre à effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-185">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ec6ec-186">Créer une application web.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-186">Create a web app.</span></span>
> * <span data-ttu-id="ec6ec-187">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-187">Add and scaffold a model.</span></span>
> * <span data-ttu-id="ec6ec-188">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-188">Work with a database.</span></span>
> * <span data-ttu-id="ec6ec-189">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-189">Add search and validation.</span></span>

<span data-ttu-id="ec6ec-190">À la fin, vous obtenez une application qui peut gérer et afficher des données de films.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-190">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="ec6ec-191">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ec6ec-191">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec6ec-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec6ec-192">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec6ec-193">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec6ec-193">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ec6ec-194">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ec6ec-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="ec6ec-195">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="ec6ec-195">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec6ec-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec6ec-196">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ec6ec-197">Dans Visual Studio, sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-197">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="ec6ec-198">Sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-198">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nouvelle application web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="ec6ec-200">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-200">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="ec6ec-201">Il est important de nommer le projet **MvcMovie** pour que l’espace de noms corresponde quand vous copiez du code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-201">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nouvelle application web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="ec6ec-203">Sélectionnez **Application web (modèle-vue-contrôleur)** , puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-203">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="ec6ec-204">Boîte de dialogue Nouveau projet, .NET Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec6ec-204">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="ec6ec-205">Visual Studio a utilisé le modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-205">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="ec6ec-206">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-206">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="ec6ec-207">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-207">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec6ec-208">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec6ec-208">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ec6ec-209">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-209">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ec6ec-210">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="ec6ec-210">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="ec6ec-211">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ec6ec-211">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="ec6ec-212">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-212">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="ec6ec-213">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-213">Run the following command:</span></span>

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="ec6ec-214">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="ec6ec-214">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="ec6ec-215">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-215">Select **Yes**</span></span>

  * <span data-ttu-id="ec6ec-216">`dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-216">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="ec6ec-217">`code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-217">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ec6ec-218">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ec6ec-218">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ec6ec-219">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-219">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ec6ec-221">Sélectionnez **.NET Core** > **Application** > **Application web (modèle-vue-contrôleur)** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-221">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="ec6ec-223">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut **.NET Core 2.2** pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-223">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![sélection de .NET Core 2.2 pour macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="ec6ec-225">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-225">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="ec6ec-226">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="ec6ec-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ec6ec-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec6ec-227">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ec6ec-228">Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-228">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="ec6ec-229">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ec6ec-230">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-230">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ec6ec-231">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-231">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ec6ec-232">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-232">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="ec6ec-233">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-233">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ec6ec-234">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-234">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="ec6ec-235">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-235">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="ec6ec-237">Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="ec6ec-237">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="ec6ec-239">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-239">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ec6ec-240">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-240">This app doesn't track personal information.</span></span> <span data-ttu-id="ec6ec-241">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ec6ec-241">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="ec6ec-243">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-243">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ec6ec-245">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ec6ec-245">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ec6ec-246">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-246">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="ec6ec-247">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-247">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="ec6ec-248">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-248">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="ec6ec-249">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-249">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="ec6ec-250">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-250">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="ec6ec-251">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-251">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ec6ec-252">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-252">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="ec6ec-253">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-253">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ec6ec-254">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-254">This app doesn't track personal information.</span></span> <span data-ttu-id="ec6ec-255">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ec6ec-255">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="ec6ec-257">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-257">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ec6ec-259">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="ec6ec-259">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ec6ec-260">Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-260">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="ec6ec-261">Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-261">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ec6ec-262">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-262">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ec6ec-263">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-263">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="ec6ec-264">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-264">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="ec6ec-265">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-265">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="ec6ec-266">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-266">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="ec6ec-267">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-267">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="ec6ec-268">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-268">This app doesn't track personal information.</span></span> <span data-ttu-id="ec6ec-269">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ec6ec-269">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="ec6ec-271">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="ec6ec-271">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="ec6ec-273">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="ec6ec-273">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ec6ec-274">Next</span><span class="sxs-lookup"><span data-stu-id="ec6ec-274">Next</span></span>](adding-controller.md)

::: moniker-end
