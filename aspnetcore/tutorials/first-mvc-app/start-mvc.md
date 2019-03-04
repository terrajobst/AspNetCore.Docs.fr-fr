---
title: Bien démarrer avec ASP.NET Core MVC
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: c09c06f55c4179e9e2174f0063ab7387b7e4c31b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899227"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="2f370-103">Bien démarrer avec ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="2f370-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="2f370-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f370-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="2f370-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2f370-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="2f370-106">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="2f370-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="2f370-107">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="2f370-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2f370-108">Créer une application web.</span><span class="sxs-lookup"><span data-stu-id="2f370-108">Create a web app.</span></span>
> * <span data-ttu-id="2f370-109">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="2f370-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="2f370-110">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="2f370-110">Work with a database.</span></span>
> * <span data-ttu-id="2f370-111">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="2f370-111">Add search and validation.</span></span>

<span data-ttu-id="2f370-112">À la fin, vous obtenez une application qui peut gérer et afficher des données de films.</span><span class="sxs-lookup"><span data-stu-id="2f370-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="2f370-113">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="2f370-113">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f370-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f370-114">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2f370-115">Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="2f370-115">From Visual Studio, select  **File > New > Project**.</span></span>

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="2f370-117">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="2f370-117">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="2f370-118">Dans le volet gauche, sélectionnez **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="2f370-118">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="2f370-119">Dans le volet central, sélectionnez **Application web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="2f370-119">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="2f370-120">Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).</span><span class="sxs-lookup"><span data-stu-id="2f370-120">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="2f370-121">Sélectionnez **OK**</span><span class="sxs-lookup"><span data-stu-id="2f370-121">select **OK**</span></span>

![<span data-ttu-id="2f370-122">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f370-122">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="2f370-123">Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :</span><span class="sxs-lookup"><span data-stu-id="2f370-123">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="2f370-124">Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.2**</span><span class="sxs-lookup"><span data-stu-id="2f370-124">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="2f370-125">Sélectionnez **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="2f370-125">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="2f370-126">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f370-126">select **OK**.</span></span>

![<span data-ttu-id="2f370-127">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f370-127">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="2f370-128">Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="2f370-128">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="2f370-129">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="2f370-129">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="2f370-130">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="2f370-130">This is a basic starter project, and it's a good place to start.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2f370-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2f370-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2f370-132">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="2f370-132">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="2f370-133">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="2f370-133">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="2f370-134">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2f370-134">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2f370-135">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="2f370-135">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="2f370-136">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2f370-136">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="2f370-137">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="2f370-137">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="2f370-138">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="2f370-138">Select **Yes**</span></span>

  * <span data-ttu-id="2f370-139">`dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="2f370-139">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="2f370-140">`code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2f370-140">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2f370-141">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2f370-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2f370-142">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="2f370-142">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="2f370-144">Sélectionnez **Application .NET Core** > **ASP.NET Core** > **Application web ASP.NET Core (MVC)** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="2f370-144">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="2f370-146">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut \**.NET Core 2.2* pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="2f370-146">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="2f370-147">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="2f370-147">Name the project **MvcMovie**, and then select **Create**.</span></span>

---  
<!-- End of VS tabs -->

### <a name="run-the-app"></a><span data-ttu-id="2f370-148">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="2f370-148">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2f370-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f370-149">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="2f370-150">Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="2f370-150">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

* <span data-ttu-id="2f370-151">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="2f370-151">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="2f370-152">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2f370-152">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2f370-153">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="2f370-153">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2f370-154">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="2f370-154">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
* <span data-ttu-id="2f370-155">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="2f370-155">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="2f370-156">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="2f370-156">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="2f370-157">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="2f370-157">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

  ![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="2f370-159">Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="2f370-159">You can debug the app by selecting the **IIS Express** button</span></span>

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2f370-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2f370-161">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="2f370-162">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="2f370-162">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="2f370-163">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2f370-163">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `https://localhost:5001`.</span></span> <span data-ttu-id="2f370-164">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2f370-164">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="2f370-165">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="2f370-165">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="2f370-166">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="2f370-166">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="2f370-167">Si vous lancez l’application avec Ctrl+F5 (mode sans débogage), vous pouvez apporter des modifications au code, enregistrer le fichier, actualiser le navigateur et examiner les modifications apportées au code.</span><span class="sxs-lookup"><span data-stu-id="2f370-167">Launching the app with Ctrl+F5 (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="2f370-168">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="2f370-168">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2f370-169">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2f370-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2f370-170">Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="2f370-170">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="2f370-171">Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="2f370-171">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="2f370-172">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="2f370-172">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="2f370-173">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="2f370-173">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="2f370-174">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="2f370-174">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="2f370-175">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="2f370-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="2f370-176">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="2f370-176">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

------

* <span data-ttu-id="2f370-177">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="2f370-177">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="2f370-178">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="2f370-178">This app doesn't track personal information.</span></span> <span data-ttu-id="2f370-179">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="2f370-179">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="2f370-181">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="2f370-181">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="2f370-183">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="2f370-183">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2f370-184">Next</span><span class="sxs-lookup"><span data-stu-id="2f370-184">Next</span></span>](adding-controller.md)  
