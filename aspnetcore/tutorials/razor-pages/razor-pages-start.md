---
title: 'Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 0cc00cb85b6054752417b82c783cfd4c306aeda5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082579"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="c0977-104">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0977-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c0977-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0977-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="c0977-106">C’est le premier d’une série de tutoriels, qui décrit les principes fondamentaux liés à la génération d’une application web de Razor Pages dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0977-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="c0977-107">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="c0977-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="c0977-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="c0977-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c0977-109">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c0977-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="c0977-110">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c0977-110">Run the app.</span></span>
> * <span data-ttu-id="c0977-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="c0977-111">Examine the project files.</span></span>

<span data-ttu-id="c0977-112">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="c0977-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="c0977-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="c0977-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0977-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0977-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0977-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0977-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0977-117">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c0977-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="c0977-118">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="c0977-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0977-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0977-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c0977-120">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="c0977-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c0977-121">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="c0977-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="c0977-122">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="c0977-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="c0977-123">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="c0977-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c0977-124">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="c0977-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="c0977-125">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="c0977-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="c0977-126">Sélectionnez **ASP.NET Core 3.0** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="c0977-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="c0977-128">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="c0977-128">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0977-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0977-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c0977-131">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="c0977-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="c0977-132">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="c0977-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="c0977-133">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="c0977-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="c0977-134">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="c0977-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="c0977-135">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c0977-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="c0977-136">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="c0977-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="c0977-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="c0977-137">Select **Yes**.</span></span>

  <span data-ttu-id="c0977-138">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="c0977-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0977-139">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c0977-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c0977-140">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="c0977-140">Select **File** > **New Solution**.</span></span>

![macOS - Nouvelle solution](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="c0977-142">Sélectionnez **.NET Core** > **Application** > **Application web** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="c0977-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="c0977-144">Dans la boîte de dialogue **Configurer votre nouvelle API web ASP.NET Core**, définissez **Framework cible** sur **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="c0977-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.0**.</span></span>

  ![Sélection de .NET Core 3.0 pour macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="c0977-146">Nommez le projet **RazorPagesMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="c0977-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="c0977-148">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="c0977-148">Open the project</span></span>

<span data-ttu-id="c0977-149">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c0977-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="c0977-150">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="c0977-150">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0977-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0977-151">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c0977-152">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c0977-152">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="c0977-153">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="c0977-153">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c0977-154">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="c0977-154">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c0977-155">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-155">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="c0977-156">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-156">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c0977-157">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="c0977-157">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0977-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0977-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="c0977-159">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c0977-159">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c0977-160">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0977-160">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="c0977-161">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="c0977-161">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c0977-162">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-162">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="c0977-163">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-163">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0977-164">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c0977-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="c0977-165">Appuyez sur **Alt-Cmd-Entrée** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c0977-165">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="c0977-166">Vous pouvez également accéder à la barre de menus et accéder à Exécuter > Démarrer sans débogage.</span><span class="sxs-lookup"><span data-stu-id="c0977-166">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="c0977-167">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0977-167">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="c0977-168">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="c0977-168">Examine the project files</span></span>

<span data-ttu-id="c0977-169">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="c0977-169">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="c0977-170">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="c0977-170">Pages folder</span></span>

<span data-ttu-id="c0977-171">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="c0977-171">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="c0977-172">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="c0977-172">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="c0977-173">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="c0977-173">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="c0977-174">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="c0977-174">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="c0977-175">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="c0977-175">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="c0977-176">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="c0977-176">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="c0977-177">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="c0977-177">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="c0977-178">Pour plus d'informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="c0977-178">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="c0977-179">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="c0977-179">wwwroot folder</span></span>

<span data-ttu-id="c0977-180">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="c0977-180">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="c0977-181">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="c0977-181">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c0977-182">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="c0977-182">appSettings.json</span></span>

<span data-ttu-id="c0977-183">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="c0977-183">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="c0977-184">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c0977-184">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="c0977-185">Program.cs</span><span class="sxs-lookup"><span data-stu-id="c0977-185">Program.cs</span></span>

<span data-ttu-id="c0977-186">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="c0977-186">Contains the entry point for the program.</span></span> <span data-ttu-id="c0977-187">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="c0977-187">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="c0977-188">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c0977-188">Startup.cs</span></span>

<span data-ttu-id="c0977-189">contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="c0977-189">Contains code that configures app behavior.</span></span> <span data-ttu-id="c0977-190">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c0977-190">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c0977-191">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="c0977-191">Next steps</span></span>

<span data-ttu-id="c0977-192">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="c0977-192">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c0977-193">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="c0977-193">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c0977-194">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="c0977-194">This is the first tutorial of a series.</span></span> <span data-ttu-id="c0977-195">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0977-195">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="c0977-196">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="c0977-196">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="c0977-197">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="c0977-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c0977-198">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c0977-198">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="c0977-199">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="c0977-199">Run the app.</span></span>
> * <span data-ttu-id="c0977-200">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="c0977-200">Examine the project files.</span></span>

<span data-ttu-id="c0977-201">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="c0977-201">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="c0977-203">Prérequis</span><span class="sxs-lookup"><span data-stu-id="c0977-203">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0977-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0977-204">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0977-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0977-205">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0977-206">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c0977-206">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="c0977-207">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="c0977-207">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0977-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0977-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c0977-209">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="c0977-209">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="c0977-210">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="c0977-210">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="c0977-212">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="c0977-212">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="c0977-213">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="c0977-213">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="c0977-215">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="c0977-215">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="c0977-217">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="c0977-217">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0977-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0977-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c0977-220">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="c0977-220">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="c0977-221">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="c0977-221">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="c0977-222">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="c0977-222">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="c0977-223">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="c0977-223">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="c0977-224">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c0977-224">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="c0977-225">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="c0977-225">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="c0977-226">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="c0977-226">Select **Yes**.</span></span>

  <span data-ttu-id="c0977-227">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="c0977-227">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0977-228">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c0977-228">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c0977-229">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c0977-229">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="c0977-230">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c0977-230">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="c0977-231">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="c0977-231">Open the project</span></span>

<span data-ttu-id="c0977-232">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c0977-232">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="c0977-233">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="c0977-233">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0977-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0977-234">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c0977-235">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c0977-235">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="c0977-236">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="c0977-236">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="c0977-237">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="c0977-237">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c0977-238">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-238">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="c0977-239">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-239">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="c0977-240">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="c0977-240">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="c0977-241">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="c0977-241">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c0977-242">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="c0977-242">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="c0977-244">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="c0977-244">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c0977-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c0977-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="c0977-247">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c0977-247">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c0977-248">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0977-248">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="c0977-249">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="c0977-249">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="c0977-250">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-250">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="c0977-251">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="c0977-251">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="c0977-252">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="c0977-252">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c0977-253">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="c0977-253">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="c0977-255">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="c0977-255">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c0977-257">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c0977-257">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="c0977-258">Appuyez sur **Cmd+Opt+F5** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="c0977-258">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="c0977-259">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c0977-259">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="c0977-260">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="c0977-260">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="c0977-261">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="c0977-261">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="c0977-263">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="c0977-263">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="c0977-265">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="c0977-265">Examine the project files</span></span>

<span data-ttu-id="c0977-266">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="c0977-266">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="c0977-267">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="c0977-267">Pages folder</span></span>

<span data-ttu-id="c0977-268">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="c0977-268">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="c0977-269">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="c0977-269">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="c0977-270">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="c0977-270">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="c0977-271">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="c0977-271">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="c0977-272">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="c0977-272">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="c0977-273">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="c0977-273">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="c0977-274">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="c0977-274">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="c0977-275">Pour plus d'informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="c0977-275">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="c0977-276">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="c0977-276">wwwroot folder</span></span>

<span data-ttu-id="c0977-277">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="c0977-277">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="c0977-278">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="c0977-278">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c0977-279">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="c0977-279">appSettings.json</span></span>

<span data-ttu-id="c0977-280">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="c0977-280">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="c0977-281">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c0977-281">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="c0977-282">Program.cs</span><span class="sxs-lookup"><span data-stu-id="c0977-282">Program.cs</span></span>

<span data-ttu-id="c0977-283">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="c0977-283">Contains the entry point for the program.</span></span> <span data-ttu-id="c0977-284">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="c0977-284">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="c0977-285">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c0977-285">Startup.cs</span></span>

<span data-ttu-id="c0977-286">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="c0977-286">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="c0977-287">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c0977-287">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0977-288">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c0977-288">Additional resources</span></span>

* [<span data-ttu-id="c0977-289">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="c0977-289">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="c0977-290">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="c0977-290">Next steps</span></span>

<span data-ttu-id="c0977-291">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="c0977-291">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c0977-292">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="c0977-292">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
