---
title: Bien démarrer avec ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC.
ms.author: riande
ms.date: 08/05/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: f6a92423546ebd9d4c8e1a92fb81b6b72f847f61
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820101"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="014cd-103">Bien démarrer avec ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="014cd-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="014cd-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="014cd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="014cd-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="014cd-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="014cd-106">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="014cd-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="014cd-107">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="014cd-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="014cd-108">Créer une application web.</span><span class="sxs-lookup"><span data-stu-id="014cd-108">Create a web app.</span></span>
> * <span data-ttu-id="014cd-109">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="014cd-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="014cd-110">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="014cd-110">Work with a database.</span></span>
> * <span data-ttu-id="014cd-111">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="014cd-111">Add search and validation.</span></span>

<span data-ttu-id="014cd-112">À la fin, vous obtenez une application qui peut gérer et afficher des données de films.</span><span class="sxs-lookup"><span data-stu-id="014cd-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="014cd-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="014cd-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="014cd-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="014cd-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="014cd-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="014cd-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="014cd-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="014cd-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app"></a><span data-ttu-id="014cd-117">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="014cd-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="014cd-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="014cd-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="014cd-119">Dans Visual Studio, sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="014cd-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="014cd-120">Sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="014cd-120">Select **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nouvelle application web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="014cd-122">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="014cd-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="014cd-123">Il est important de nommer le projet **MvcMovie** pour que l’espace de noms corresponde quand vous copiez du code.</span><span class="sxs-lookup"><span data-stu-id="014cd-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nouvelle application web ASP.NET Core](start-mvc/_static/config.png)

* <span data-ttu-id="014cd-125">Sélectionnez **Application web (modèle-vue-contrôleur)** , puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="014cd-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="014cd-126">Boîte de dialogue Nouveau projet, .NET Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="014cd-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="014cd-127">Visual Studio a utilisé le modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="014cd-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="014cd-128">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="014cd-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="014cd-129">Il s’agit d’un projet de démarrage de base.</span><span class="sxs-lookup"><span data-stu-id="014cd-129">This is a basic starter project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="014cd-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="014cd-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="014cd-131">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="014cd-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="014cd-132">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="014cd-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="014cd-133">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="014cd-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="014cd-134">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="014cd-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="014cd-135">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="014cd-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="014cd-136">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="014cd-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="014cd-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="014cd-137">Select **Yes**</span></span>

  * <span data-ttu-id="014cd-138">`dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="014cd-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="014cd-139">`code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="014cd-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="014cd-140">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="014cd-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="014cd-141">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="014cd-141">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="014cd-143">Sélectionnez **.NET Core** > **Application** > **Application web (modèle-vue-contrôleur)** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="014cd-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="014cd-145">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, définissez **.NET Core 3.0** comme **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="014cd-145">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** of **.NET Core 3.0**.</span></span>

<!-- 
  ![macOS .NET Core 2.2 selection](./start-mvc/_static/new_project_22_vsmac.png)
-->

* <span data-ttu-id="014cd-146">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="014cd-146">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="014cd-147">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="014cd-147">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="014cd-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="014cd-148">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="014cd-149">Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="014cd-149">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="014cd-150">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="014cd-150">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="014cd-151">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="014cd-151">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="014cd-152">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-152">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="014cd-153">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="014cd-153">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="014cd-154">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="014cd-154">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="014cd-155">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="014cd-155">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="014cd-156">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="014cd-156">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="014cd-158">Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="014cd-158">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)


  <span data-ttu-id="014cd-160">L’image suivante montre l’application :</span><span class="sxs-lookup"><span data-stu-id="014cd-160">The following image shows the app:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="014cd-162">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="014cd-162">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="014cd-163">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="014cd-163">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="014cd-164">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="014cd-164">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="014cd-165">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="014cd-165">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="014cd-166">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-166">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="014cd-167">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-167">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="014cd-168">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="014cd-168">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="014cd-169">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="014cd-169">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="014cd-170">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="014cd-170">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="014cd-171">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="014cd-171">This app doesn't track personal information.</span></span> <span data-ttu-id="014cd-172">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="014cd-172">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="014cd-174">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="014cd-174">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="014cd-176">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="014cd-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="014cd-177">Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="014cd-177">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="014cd-178">Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="014cd-178">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="014cd-179">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="014cd-179">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="014cd-180">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-180">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="014cd-181">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="014cd-181">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="014cd-182">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="014cd-182">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="014cd-183">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="014cd-183">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

  <span data-ttu-id="014cd-184">L’image suivante montre l’application :</span><span class="sxs-lookup"><span data-stu-id="014cd-184">The following image shows the app:</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="014cd-186">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="014cd-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="014cd-187">Next</span><span class="sxs-lookup"><span data-stu-id="014cd-187">Next</span></span>](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="014cd-188">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="014cd-188">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="014cd-189">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="014cd-189">The app manages a database of movie titles.</span></span> <span data-ttu-id="014cd-190">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="014cd-190">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="014cd-191">Créer une application web.</span><span class="sxs-lookup"><span data-stu-id="014cd-191">Create a web app.</span></span>
> * <span data-ttu-id="014cd-192">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="014cd-192">Add and scaffold a model.</span></span>
> * <span data-ttu-id="014cd-193">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="014cd-193">Work with a database.</span></span>
> * <span data-ttu-id="014cd-194">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="014cd-194">Add search and validation.</span></span>

<span data-ttu-id="014cd-195">À la fin, vous obtenez une application qui peut gérer et afficher des données de films.</span><span class="sxs-lookup"><span data-stu-id="014cd-195">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="014cd-196">Prérequis</span><span class="sxs-lookup"><span data-stu-id="014cd-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="014cd-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="014cd-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="014cd-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="014cd-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="014cd-199">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="014cd-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="014cd-200">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="014cd-200">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="014cd-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="014cd-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="014cd-202">Dans Visual Studio, sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="014cd-202">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="014cd-203">Sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="014cd-203">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nouvelle application web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="014cd-205">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="014cd-205">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="014cd-206">Il est important de nommer le projet **MvcMovie** pour que l’espace de noms corresponde quand vous copiez du code.</span><span class="sxs-lookup"><span data-stu-id="014cd-206">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nouvelle application web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="014cd-208">Sélectionnez **Application web (modèle-vue-contrôleur)** , puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="014cd-208">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="014cd-209">Boîte de dialogue Nouveau projet, .NET Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="014cd-209">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="014cd-210">Visual Studio a utilisé le modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="014cd-210">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="014cd-211">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="014cd-211">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="014cd-212">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="014cd-212">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="014cd-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="014cd-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="014cd-214">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="014cd-214">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="014cd-215">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="014cd-215">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="014cd-216">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="014cd-216">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="014cd-217">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="014cd-217">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="014cd-218">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="014cd-218">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="014cd-219">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="014cd-219">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="014cd-220">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="014cd-220">Select **Yes**</span></span>

  * <span data-ttu-id="014cd-221">`dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="014cd-221">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="014cd-222">`code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="014cd-222">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="014cd-223">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="014cd-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="014cd-224">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="014cd-224">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="014cd-226">Sélectionnez **.NET Core** > **Application** > **Application web (modèle-vue-contrôleur)** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="014cd-226">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="014cd-228">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut **.NET Core 2.2** pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="014cd-228">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![sélection de .NET Core 2.2 pour macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="014cd-230">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="014cd-230">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="014cd-231">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="014cd-231">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="014cd-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="014cd-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="014cd-233">Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="014cd-233">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="014cd-234">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="014cd-234">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="014cd-235">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="014cd-235">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="014cd-236">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-236">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="014cd-237">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="014cd-237">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="014cd-238">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="014cd-238">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="014cd-239">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="014cd-239">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="014cd-240">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="014cd-240">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="014cd-242">Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="014cd-242">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="014cd-244">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="014cd-244">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="014cd-245">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="014cd-245">This app doesn't track personal information.</span></span> <span data-ttu-id="014cd-246">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="014cd-246">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="014cd-248">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="014cd-248">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="014cd-250">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="014cd-250">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="014cd-251">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="014cd-251">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="014cd-252">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="014cd-252">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="014cd-253">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="014cd-253">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="014cd-254">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-254">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="014cd-255">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-255">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="014cd-256">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="014cd-256">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="014cd-257">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="014cd-257">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="014cd-258">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="014cd-258">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="014cd-259">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="014cd-259">This app doesn't track personal information.</span></span> <span data-ttu-id="014cd-260">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="014cd-260">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="014cd-262">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="014cd-262">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="014cd-264">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="014cd-264">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="014cd-265">Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="014cd-265">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="014cd-266">Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="014cd-266">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="014cd-267">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="014cd-267">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="014cd-268">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="014cd-268">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="014cd-269">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="014cd-269">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="014cd-270">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="014cd-270">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="014cd-271">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="014cd-271">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="014cd-272">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="014cd-272">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="014cd-273">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="014cd-273">This app doesn't track personal information.</span></span> <span data-ttu-id="014cd-274">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="014cd-274">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="014cd-276">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="014cd-276">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="014cd-278">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="014cd-278">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="014cd-279">Next</span><span class="sxs-lookup"><span data-stu-id="014cd-279">Next</span></span>](adding-controller.md)

::: moniker-end
