---
title: Bien démarrer avec ASP.NET Core MVC
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Découvrez comment bien démarrer avec ASP.NET Core MVC.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: cfce3b5792a5d0673bae5ddbba9e2d4d515a6279
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381801"
---
# <a name="get-started-with-aspnet-core-mvc"></a><span data-ttu-id="10d43-103">Bien démarrer avec ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="10d43-103">Get started with ASP.NET Core MVC</span></span>

<span data-ttu-id="10d43-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="10d43-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

https://docs.microsoft.com/en-us/visualstudio/ide/visual-studio-ide?view=vs-2017

<span data-ttu-id="10d43-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="10d43-105">This tutorial teaches the basics of building an ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="10d43-106">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="10d43-106">The app manages a database of movie titles.</span></span> <span data-ttu-id="10d43-107">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="10d43-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="10d43-108">Créer une application web.</span><span class="sxs-lookup"><span data-stu-id="10d43-108">Create a web app.</span></span>
> * <span data-ttu-id="10d43-109">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="10d43-109">Add and scaffold a model.</span></span>
> * <span data-ttu-id="10d43-110">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="10d43-110">Work with a database.</span></span>
> * <span data-ttu-id="10d43-111">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="10d43-111">Add search and validation.</span></span>

<span data-ttu-id="10d43-112">À la fin, vous obtenez une application qui peut gérer et afficher des données de films.</span><span class="sxs-lookup"><span data-stu-id="10d43-112">At the end, you have an app that can manage and display movie data.</span></span>

[!INCLUDE[](~/includes/mvc-intro/download.md)]

> [!NOTE]
> <span data-ttu-id="10d43-113">Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10d43-113">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="10d43-114">Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="10d43-114">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="10d43-115">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="10d43-115">Create a web app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="10d43-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="10d43-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="10d43-117">Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="10d43-117">From Visual Studio, select  **File > New > Project**.</span></span>

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="10d43-119">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="10d43-119">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="10d43-120">Dans le volet gauche, sélectionnez **.NET Core**</span><span class="sxs-lookup"><span data-stu-id="10d43-120">In the left pane, select **.NET Core**</span></span>
* <span data-ttu-id="10d43-121">Dans le volet central, sélectionnez **Application web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="10d43-121">In the center pane, select **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="10d43-122">Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).</span><span class="sxs-lookup"><span data-stu-id="10d43-122">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="10d43-123">Sélectionnez **OK**</span><span class="sxs-lookup"><span data-stu-id="10d43-123">select **OK**</span></span>

![<span data-ttu-id="10d43-124">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10d43-124">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="10d43-125">Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :</span><span class="sxs-lookup"><span data-stu-id="10d43-125">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="10d43-126">Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.2**</span><span class="sxs-lookup"><span data-stu-id="10d43-126">In the version selector drop-down box select **ASP.NET Core 2.2**</span></span>
* <span data-ttu-id="10d43-127">Sélectionnez **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="10d43-127">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="10d43-128">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="10d43-128">select **OK**.</span></span>

![<span data-ttu-id="10d43-129">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10d43-129">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="10d43-130">Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="10d43-130">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="10d43-131">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="10d43-131">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="10d43-132">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="10d43-132">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="10d43-133">Sélectionnez **Ctrl-F5** pour exécuter l'application en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="10d43-133">Select **Ctrl-F5** to run the app in non-debug mode.</span></span>

* <span data-ttu-id="10d43-134">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="10d43-134">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="10d43-135">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="10d43-135">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="10d43-136">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="10d43-136">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="10d43-137">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="10d43-137">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="10d43-138">Dans l’image ci-dessus, le numéro de port est 5000.</span><span class="sxs-lookup"><span data-stu-id="10d43-138">In the image above, the port number is 5000.</span></span> <span data-ttu-id="10d43-139">L’URL dans le navigateur affiche `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="10d43-139">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="10d43-140">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="10d43-140">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="10d43-141">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="10d43-141">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="10d43-142">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="10d43-142">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="10d43-143">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="10d43-143">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="10d43-145">Vous pouvez déboguer l’application en sélectionnant le bouton **IIS Express**</span><span class="sxs-lookup"><span data-stu-id="10d43-145">You can debug the app by selecting the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="10d43-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="10d43-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="10d43-148">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="10d43-148">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="10d43-149">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="10d43-149">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span>

* <span data-ttu-id="10d43-150">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="10d43-150">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="10d43-151">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="10d43-151">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="10d43-152">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="10d43-152">Run the following command:</span></span>

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * <span data-ttu-id="10d43-153">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « MvcMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="10d43-153">A dialog box appears with **Required assets to build and debug are missing from 'MvcMovie'. Add them?**</span></span>  <span data-ttu-id="10d43-154">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="10d43-154">Select **Yes**</span></span>

  * <span data-ttu-id="10d43-155">`dotnet new mvc -o MvcMovie` : crée un nouveau projet ASP.NET Core MVC dans le dossier *MvcMovie*.</span><span class="sxs-lookup"><span data-stu-id="10d43-155">`dotnet new mvc -o MvcMovie`: creates a new ASP.NET Core MVC project in the *MvcMovie* folder.</span></span>
  * <span data-ttu-id="10d43-156">`code -r MvcMovie`: charge le fichier de projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="10d43-156">`code -r MvcMovie`: Loads the *MvcMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="10d43-157">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="10d43-157">Launch the app</span></span>

* <span data-ttu-id="10d43-158">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="10d43-158">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="10d43-159">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="10d43-159">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="10d43-160">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="10d43-160">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="10d43-161">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="10d43-161">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="10d43-162">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="10d43-162">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="10d43-163">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="10d43-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="10d43-164">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="10d43-164">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="10d43-165">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="10d43-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="10d43-166">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="10d43-166">Select **File** > **New Solution**.</span></span>

  ![macOS - Nouvelle solution](~/tutorials/first-web-api-mac/_static/sln.png)

* <span data-ttu-id="10d43-168">Sélectionnez **Application .NET Core** > **ASP.NET Core** > **Application web ASP.NET Core (MVC)** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="10d43-168">Select **.NET Core App** > **ASP.NET Core** > **ASP.NET Core Web App (MVC)** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* <span data-ttu-id="10d43-170">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, acceptez la valeur par défaut \**.NET Core 2.2* pour **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="10d43-170">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="10d43-171">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="10d43-171">Name the project **MvcMovie**, and then select **Create**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="10d43-172">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="10d43-172">Launch the app</span></span>

<span data-ttu-id="10d43-173">Sélectionnez **Exécuter** > **Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="10d43-173">Select **Run** > **Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="10d43-174">Visual Studio pour Mac démarre le serveur [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="10d43-174">Visual Studio for Mac starts [Kestrel](xref:fundamentals/servers/index#kestrel) server, launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

* <span data-ttu-id="10d43-175">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="10d43-175">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="10d43-176">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="10d43-176">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="10d43-177">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="10d43-177">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="10d43-178">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="10d43-178">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="10d43-179">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="10d43-179">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

---  
<!-- End of VS tabs -->

* <span data-ttu-id="10d43-180">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="10d43-180">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="10d43-181">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="10d43-181">This app doesn't track personal information.</span></span> <span data-ttu-id="10d43-182">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="10d43-182">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/privacy.png)

  <span data-ttu-id="10d43-184">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="10d43-184">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

<span data-ttu-id="10d43-186">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="10d43-186">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="10d43-187">Next</span><span class="sxs-lookup"><span data-stu-id="10d43-187">Next</span></span>](adding-controller.md)  
