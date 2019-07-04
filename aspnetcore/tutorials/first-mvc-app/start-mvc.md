---
title: Bien démarrer avec ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC.
ms.author: riande
ms.date: 04/24/2019
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 91564bac02df77632a3b56b6dbddeb3b622f6438
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555847"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="204af-103">Bien démarrer avec ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="204af-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="204af-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="204af-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="204af-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="204af-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="204af-106">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="204af-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="204af-107">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="204af-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="204af-108">Créer une application web.</span><span class="sxs-lookup"><span data-stu-id="204af-108">Create a web app.</span></span>
> * <span data-ttu-id="204af-109">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="204af-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="204af-110">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="204af-110">Work with a database.</span></span>
> * <span data-ttu-id="204af-111">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="204af-111">Add search and validation.</span></span>

<span data-ttu-id="204af-112">À la fin, vous obtenez une application qui peut gérer et afficher des données de films.</span><span class="sxs-lookup"><span data-stu-id="204af-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="204af-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="204af-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="204af-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="204af-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="204af-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="204af-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="204af-116">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="204af-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a><span data-ttu-id="204af-117">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="204af-117">Create a web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="204af-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="204af-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="204af-119">Dans Visual Studio, sélectionnez **Créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="204af-119">From the Visual Studio select **Create a new project**.</span></span>

* <span data-ttu-id="204af-120">Sélectionnez **Application web ASP.NET Core**, puis **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="204af-120">Selecct **ASP.NET Core Web Application** and then select **Next**.</span></span>

![Nouvelle application web ASP.NET Core](start-mvc/_static/np_2.1.png)

* <span data-ttu-id="204af-122">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="204af-122">Name the project **MvcMovie** and select **Create**.</span></span> <span data-ttu-id="204af-123">Il est important de nommer le projet **MvcMovie** pour que l’espace de noms corresponde quand vous copiez du code.</span><span class="sxs-lookup"><span data-stu-id="204af-123">It's important to name the project **MvcMovie** so when you copy code, the namespace will match.</span></span>

  ![Nouvelle application web ASP.NET Core](start-mvc/_static/config.png)


* <span data-ttu-id="204af-125">Sélectionnez **Application web (modèle-vue-contrôleur)** , puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="204af-125">Select **Web Application(Model-View-Controller)**, and then select **Create**.</span></span>

![<span data-ttu-id="204af-126">Boîte de dialogue Nouveau projet, .NET Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="204af-126">New project dialog, .NET Core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="204af-127">Visual Studio a utilisé le modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="204af-127">Visual Studio used the default template for the MVC project you just created.</span></span> <span data-ttu-id="204af-128">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="204af-128">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="204af-129">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="204af-129">This is a basic starter project, and it's a good place to start.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="204af-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="204af-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="204af-131">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="204af-131">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="204af-132">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="204af-132">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="204af-133">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="204af-133">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="204af-134">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="204af-134">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="204af-135">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="204af-135">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="204af-136">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="204af-136">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="204af-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="204af-137">Select **Yes**</span></span>

  * <span data-ttu-id="204af-138">`dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="204af-138">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="204af-139">`code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="204af-139">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="204af-140">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="204af-140">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="204af-141">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="204af-141">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](./start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="204af-143">Sélectionnez **Application** **.NET Core** >  > **Application web (modèle-vue-contrôleur)**  > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="204af-143">Select **.NET Core** > **App** > **Web Application (Model-View-Controller)** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](./start-mvc/_static/new_project_mvc_vsmac.png)

* <span data-ttu-id="204af-145">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut **.NET Core 2.2** pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="204af-145">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of **.NET Core 2.2**.</span></span>

  ![sélection de .NET Core 2.2 pour macOS](./start-mvc/_static/new_project_22_vsmac.png)

* <span data-ttu-id="204af-147">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="204af-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---

### <a name="run-the-app"></a><span data-ttu-id="204af-148">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="204af-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="204af-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="204af-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="204af-150">Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="204af-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="204af-151">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="204af-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="204af-152">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="204af-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="204af-153">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="204af-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="204af-154">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="204af-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="204af-155">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="204af-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="204af-156">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="204af-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="204af-157">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="204af-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="204af-159">Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="204af-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

* <span data-ttu-id="204af-161">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="204af-161">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="204af-162">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="204af-162">This app doesn't track personal information.</span></span> <span data-ttu-id="204af-163">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="204af-163">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="204af-165">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="204af-165">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="204af-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="204af-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="204af-168">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="204af-168">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="204af-169">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="204af-169">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="204af-170">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="204af-170">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="204af-171">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="204af-171">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="204af-172">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="204af-172">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="204af-173">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="204af-173">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="204af-174">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="204af-174">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

* <span data-ttu-id="204af-175">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="204af-175">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="204af-176">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="204af-176">This app doesn't track personal information.</span></span> <span data-ttu-id="204af-177">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="204af-177">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="204af-179">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="204af-179">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="204af-181">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="204af-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="204af-182">Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="204af-182">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="204af-183">Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="204af-183">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="204af-184">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="204af-184">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="204af-185">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="204af-185">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="204af-186">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="204af-186">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="204af-187">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="204af-187">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="204af-188">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="204af-188">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

* <span data-ttu-id="204af-189">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="204af-189">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="204af-190">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="204af-190">This app doesn't track personal information.</span></span> <span data-ttu-id="204af-191">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="204af-191">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_privacy_macos.png)

  <span data-ttu-id="204af-193">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="204af-193">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="204af-195">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="204af-195">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="204af-196">Next</span><span class="sxs-lookup"><span data-stu-id="204af-196">Next</span></span>](adding-controller.md)
