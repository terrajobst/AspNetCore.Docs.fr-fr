---
title: 'Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 67a5fcee0a37861fd39a018443edbc0b9e513213
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487659"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="d7e3d-104">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7e3d-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="d7e3d-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d7e3d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="d7e3d-106">C’est le premier d’une série de tutoriels, qui décrit les principes fondamentaux liés à la génération d’une application web de Razor Pages dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="d7e3d-107">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="d7e3d-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7e3d-109">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="d7e3d-110">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-110">Run the app.</span></span>
> * <span data-ttu-id="d7e3d-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-111">Examine the project files.</span></span>

<span data-ttu-id="d7e3d-112">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="d7e3d-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d7e3d-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7e3d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7e3d-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7e3d-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7e3d-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7e3d-117">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d7e3d-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="d7e3d-118">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="d7e3d-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7e3d-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7e3d-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d7e3d-120">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d7e3d-121">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="d7e3d-122">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="d7e3d-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="d7e3d-123">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="d7e3d-124">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="d7e3d-125">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="d7e3d-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="d7e3d-126">Sélectionnez **ASP.NET Core 3.0** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="d7e3d-128">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-128">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7e3d-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7e3d-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d7e3d-131">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d7e3d-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="d7e3d-132">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="d7e3d-133">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="d7e3d-134">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="d7e3d-135">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="d7e3d-136">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="d7e3d-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="d7e3d-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-137">Select **Yes**.</span></span>

  <span data-ttu-id="d7e3d-138">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7e3d-139">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d7e3d-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d7e3d-140">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-140">Select **File** > **New Solution**.</span></span>

![macOS - Nouvelle solution](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="d7e3d-142">Sélectionnez **.NET Core** > **Application** > **Application web** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="d7e3d-144">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, définissez **Framework cible** sur **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![Sélection de .NET Core 3.0 pour macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="d7e3d-146">Nommez le projet **RazorPagesMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="d7e3d-148">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="d7e3d-148">Open the project</span></span>

<span data-ttu-id="d7e3d-149">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="d7e3d-150">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="d7e3d-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7e3d-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7e3d-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d7e3d-152">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="d7e3d-153">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d7e3d-154">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d7e3d-155">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="d7e3d-156">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="d7e3d-157">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7e3d-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7e3d-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="d7e3d-159">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="d7e3d-160">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="d7e3d-161">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d7e3d-162">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d7e3d-163">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7e3d-164">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d7e3d-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d7e3d-165">Appuyez sur **Alt-Cmd-Entrée** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="d7e3d-166">Vous pouvez également accéder à la barre de menus et accéder à Exécuter > Démarrer sans débogage.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="d7e3d-167">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="d7e3d-168">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="d7e3d-168">Examine the project files</span></span>

<span data-ttu-id="d7e3d-169">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="d7e3d-170">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="d7e3d-170">Pages folder</span></span>

<span data-ttu-id="d7e3d-171">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="d7e3d-172">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="d7e3d-173">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="d7e3d-174">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="d7e3d-175">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="d7e3d-176">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="d7e3d-177">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="d7e3d-178">Pour plus d’informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="d7e3d-179">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="d7e3d-179">wwwroot folder</span></span>

<span data-ttu-id="d7e3d-180">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="d7e3d-181">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="d7e3d-182">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="d7e3d-182">appSettings.json</span></span>

<span data-ttu-id="d7e3d-183">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="d7e3d-184">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="d7e3d-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="d7e3d-185">Program.cs</span></span>

<span data-ttu-id="d7e3d-186">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-186">Contains the entry point for the program.</span></span> <span data-ttu-id="d7e3d-187">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="d7e3d-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d7e3d-188">Startup.cs</span></span>

<span data-ttu-id="d7e3d-189">contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="d7e3d-190">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7e3d-191">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d7e3d-191">Next steps</span></span>

<span data-ttu-id="d7e3d-192">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d7e3d-193">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="d7e3d-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d7e3d-194">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="d7e3d-195">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="d7e3d-196">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="d7e3d-197">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7e3d-198">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="d7e3d-199">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-199">Run the app.</span></span>
> * <span data-ttu-id="d7e3d-200">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-200">Examine the project files.</span></span>

<span data-ttu-id="d7e3d-201">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="d7e3d-203">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d7e3d-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7e3d-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7e3d-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7e3d-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7e3d-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7e3d-206">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d7e3d-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="d7e3d-207">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="d7e3d-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7e3d-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7e3d-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d7e3d-209">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="d7e3d-210">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="d7e3d-212">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="d7e3d-213">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="d7e3d-215">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="d7e3d-217">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-217">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7e3d-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7e3d-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d7e3d-220">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d7e3d-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="d7e3d-221">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="d7e3d-222">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-222">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="d7e3d-223">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="d7e3d-224">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="d7e3d-225">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="d7e3d-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="d7e3d-226">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-226">Select **Yes**.</span></span>

  <span data-ttu-id="d7e3d-227">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7e3d-228">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d7e3d-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d7e3d-229">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="d7e3d-230">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="d7e3d-231">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="d7e3d-231">Open the project</span></span>

<span data-ttu-id="d7e3d-232">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="d7e3d-233">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="d7e3d-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7e3d-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7e3d-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d7e3d-235">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="d7e3d-236">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d7e3d-237">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d7e3d-238">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="d7e3d-239">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="d7e3d-240">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="d7e3d-241">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d7e3d-242">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="d7e3d-244">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7e3d-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7e3d-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="d7e3d-247">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="d7e3d-248">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="d7e3d-249">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d7e3d-250">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d7e3d-251">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="d7e3d-252">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d7e3d-253">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="d7e3d-255">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7e3d-257">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d7e3d-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d7e3d-258">Appuyez sur **Cmd+Opt+F5** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="d7e3d-259">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="d7e3d-260">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d7e3d-261">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="d7e3d-263">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="d7e3d-265">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="d7e3d-265">Examine the project files</span></span>

<span data-ttu-id="d7e3d-266">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="d7e3d-267">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="d7e3d-267">Pages folder</span></span>

<span data-ttu-id="d7e3d-268">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="d7e3d-269">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="d7e3d-270">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="d7e3d-271">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="d7e3d-272">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="d7e3d-273">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="d7e3d-274">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="d7e3d-275">Pour plus d’informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="d7e3d-276">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="d7e3d-276">wwwroot folder</span></span>

<span data-ttu-id="d7e3d-277">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="d7e3d-278">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="d7e3d-279">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="d7e3d-279">appSettings.json</span></span>

<span data-ttu-id="d7e3d-280">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="d7e3d-281">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="d7e3d-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="d7e3d-282">Program.cs</span></span>

<span data-ttu-id="d7e3d-283">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-283">Contains the entry point for the program.</span></span> <span data-ttu-id="d7e3d-284">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="d7e3d-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d7e3d-285">Startup.cs</span></span>

<span data-ttu-id="d7e3d-286">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="d7e3d-287">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d7e3d-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d7e3d-288">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d7e3d-288">Additional resources</span></span>

* [<span data-ttu-id="d7e3d-289">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="d7e3d-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="d7e3d-290">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d7e3d-290">Next steps</span></span>

<span data-ttu-id="d7e3d-291">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="d7e3d-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d7e3d-292">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="d7e3d-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
