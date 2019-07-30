---
title: 'Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1605197188d97f27a884739a72400da2d5818b1a
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372010"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="013f4-104">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="013f4-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="013f4-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="013f4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="013f4-106">C’est le premier d’une série de tutoriels, qui décrit les principes fondamentaux liés à la génération d’une application web de Razor Pages dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="013f4-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="013f4-107">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="013f4-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="013f4-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="013f4-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="013f4-109">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="013f4-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="013f4-110">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="013f4-110">Run the app.</span></span>
> * <span data-ttu-id="013f4-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="013f4-111">Examine the project files.</span></span>

<span data-ttu-id="013f4-112">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="013f4-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="013f4-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="013f4-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013f4-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013f4-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013f4-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013f4-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013f4-117">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="013f4-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="013f4-118">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="013f4-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013f4-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013f4-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013f4-120">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="013f4-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="013f4-121">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="013f4-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="013f4-122">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="013f4-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="013f4-123">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="013f4-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="013f4-124">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="013f4-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="013f4-125">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="013f4-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="013f4-126">Sélectionnez **ASP.NET Core 3.0** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="013f4-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="013f4-128">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="013f4-128">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013f4-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013f4-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="013f4-131">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="013f4-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="013f4-132">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="013f4-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="013f4-133">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="013f4-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="013f4-134">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="013f4-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="013f4-135">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="013f4-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="013f4-136">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="013f4-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="013f4-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="013f4-137">Select **Yes**.</span></span>

  <span data-ttu-id="013f4-138">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="013f4-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013f4-139">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="013f4-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="013f4-140">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="013f4-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="013f4-141">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="013f4-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="013f4-142">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="013f4-142">Open the project</span></span>

<span data-ttu-id="013f4-143">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="013f4-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="013f4-144">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="013f4-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013f4-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013f4-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013f4-146">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="013f4-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="013f4-147">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="013f4-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="013f4-148">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="013f4-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="013f4-149">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="013f4-150">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="013f4-151">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="013f4-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013f4-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013f4-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="013f4-153">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="013f4-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="013f4-154">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="013f4-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="013f4-155">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="013f4-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="013f4-156">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="013f4-157">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013f4-158">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="013f4-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="013f4-159">Appuyez sur **Cmd+Opt+F5** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="013f4-159">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="013f4-160">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="013f4-160">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="013f4-161">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="013f4-161">Examine the project files</span></span>

<span data-ttu-id="013f4-162">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="013f4-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="013f4-163">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="013f4-163">Pages folder</span></span>

<span data-ttu-id="013f4-164">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="013f4-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="013f4-165">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="013f4-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="013f4-166">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="013f4-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="013f4-167">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="013f4-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="013f4-168">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="013f4-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="013f4-169">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="013f4-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="013f4-170">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="013f4-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="013f4-171">Pour plus d’informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="013f4-171">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="013f4-172">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="013f4-172">wwwroot folder</span></span>

<span data-ttu-id="013f4-173">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="013f4-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="013f4-174">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="013f4-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="013f4-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="013f4-175">appSettings.json</span></span>

<span data-ttu-id="013f4-176">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="013f4-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="013f4-177">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="013f4-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="013f4-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="013f4-178">Program.cs</span></span>

<span data-ttu-id="013f4-179">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="013f4-179">Contains the entry point for the program.</span></span> <span data-ttu-id="013f4-180">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="013f4-180">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="013f4-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="013f4-181">Startup.cs</span></span>

<span data-ttu-id="013f4-182">contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="013f4-182">Contains code that configures app behavior.</span></span> <span data-ttu-id="013f4-183">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="013f4-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="013f4-184">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="013f4-184">Next steps</span></span>

<span data-ttu-id="013f4-185">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="013f4-185">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="013f4-186">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="013f4-186">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="013f4-187">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="013f4-187">This is the first tutorial of a series.</span></span> <span data-ttu-id="013f4-188">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="013f4-188">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="013f4-189">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="013f4-189">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="013f4-190">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="013f4-190">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="013f4-191">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="013f4-191">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="013f4-192">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="013f4-192">Run the app.</span></span>
> * <span data-ttu-id="013f4-193">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="013f4-193">Examine the project files.</span></span>

<span data-ttu-id="013f4-194">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="013f4-194">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="013f4-196">Prérequis</span><span class="sxs-lookup"><span data-stu-id="013f4-196">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013f4-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013f4-197">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013f4-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013f4-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013f4-199">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="013f4-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="013f4-200">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="013f4-200">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013f4-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013f4-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013f4-202">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="013f4-202">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="013f4-203">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="013f4-203">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="013f4-205">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="013f4-205">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="013f4-206">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="013f4-206">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="013f4-208">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="013f4-208">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="013f4-210">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="013f4-210">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013f4-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013f4-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="013f4-213">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="013f4-213">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="013f4-214">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="013f4-214">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="013f4-215">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="013f4-215">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="013f4-216">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="013f4-216">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="013f4-217">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="013f4-217">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="013f4-218">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="013f4-218">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="013f4-219">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="013f4-219">Select **Yes**.</span></span>

  <span data-ttu-id="013f4-220">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="013f4-220">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013f4-221">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="013f4-221">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="013f4-222">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="013f4-222">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="013f4-223">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="013f4-223">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="013f4-224">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="013f4-224">Open the project</span></span>

<span data-ttu-id="013f4-225">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="013f4-225">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="013f4-226">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="013f4-226">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="013f4-227">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="013f4-227">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="013f4-228">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="013f4-228">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="013f4-229">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="013f4-229">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="013f4-230">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="013f4-230">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="013f4-231">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-231">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="013f4-232">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-232">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="013f4-233">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="013f4-233">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="013f4-234">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="013f4-234">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="013f4-235">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="013f4-235">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="013f4-237">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="013f4-237">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="013f4-239">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="013f4-239">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="013f4-240">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="013f4-240">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="013f4-241">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="013f4-241">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="013f4-242">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="013f4-242">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="013f4-243">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-243">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="013f4-244">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="013f4-244">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="013f4-245">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="013f4-245">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="013f4-246">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="013f4-246">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="013f4-248">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="013f4-248">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="013f4-250">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="013f4-250">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="013f4-251">Appuyez sur **Cmd+Opt+F5** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="013f4-251">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="013f4-252">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="013f4-252">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="013f4-253">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="013f4-253">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="013f4-254">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="013f4-254">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="013f4-256">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="013f4-256">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="013f4-258">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="013f4-258">Examine the project files</span></span>

<span data-ttu-id="013f4-259">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="013f4-259">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="013f4-260">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="013f4-260">Pages folder</span></span>

<span data-ttu-id="013f4-261">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="013f4-261">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="013f4-262">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="013f4-262">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="013f4-263">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="013f4-263">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="013f4-264">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="013f4-264">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="013f4-265">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="013f4-265">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="013f4-266">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="013f4-266">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="013f4-267">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="013f4-267">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="013f4-268">Pour plus d’informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="013f4-268">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="013f4-269">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="013f4-269">wwwroot folder</span></span>

<span data-ttu-id="013f4-270">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="013f4-270">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="013f4-271">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="013f4-271">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="013f4-272">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="013f4-272">appSettings.json</span></span>

<span data-ttu-id="013f4-273">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="013f4-273">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="013f4-274">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="013f4-274">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="013f4-275">Program.cs</span><span class="sxs-lookup"><span data-stu-id="013f4-275">Program.cs</span></span>

<span data-ttu-id="013f4-276">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="013f4-276">Contains the entry point for the program.</span></span> <span data-ttu-id="013f4-277">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="013f4-277">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="013f4-278">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="013f4-278">Startup.cs</span></span>

<span data-ttu-id="013f4-279">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="013f4-279">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="013f4-280">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="013f4-280">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="013f4-281">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="013f4-281">Additional resources</span></span>

* [<span data-ttu-id="013f4-282">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="013f4-282">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="013f4-283">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="013f4-283">Next steps</span></span>

<span data-ttu-id="013f4-284">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="013f4-284">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="013f4-285">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="013f4-285">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end