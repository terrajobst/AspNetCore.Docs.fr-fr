---
title: 'Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 07/25/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 57a10895c718c539ece280afcb27cb4033c7fb45
ms.sourcegitcommit: 979dbfc5e9ce09b9470789989cddfcfb57079d94
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682791"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="9727a-104">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9727a-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="9727a-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9727a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="9727a-106">C’est le premier d’une série de tutoriels, qui décrit les principes fondamentaux liés à la génération d’une application web de Razor Pages dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9727a-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9727a-107">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="9727a-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9727a-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="9727a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9727a-109">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9727a-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9727a-110">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="9727a-110">Run the app.</span></span>
> * <span data-ttu-id="9727a-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="9727a-111">Examine the project files.</span></span>

<span data-ttu-id="9727a-112">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="9727a-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9727a-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9727a-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9727a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9727a-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9727a-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9727a-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9727a-117">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9727a-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9727a-118">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="9727a-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9727a-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9727a-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9727a-120">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="9727a-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9727a-121">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="9727a-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="9727a-122">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="9727a-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="9727a-123">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9727a-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9727a-124">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="9727a-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="9727a-125">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="9727a-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="9727a-126">Sélectionnez **ASP.NET Core 3.0** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="9727a-126">Select **ASP.NET Core 3.0** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="9727a-128">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="9727a-128">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9727a-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9727a-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9727a-131">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9727a-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9727a-132">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="9727a-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9727a-133">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9727a-133">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9727a-134">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="9727a-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9727a-135">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9727a-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9727a-136">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="9727a-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9727a-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="9727a-137">Select **Yes**.</span></span>

  <span data-ttu-id="9727a-138">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="9727a-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9727a-139">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9727a-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9727a-140">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9727a-140">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9727a-141">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9727a-141">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9727a-142">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="9727a-142">Open the project</span></span>

<span data-ttu-id="9727a-143">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9727a-143">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9727a-144">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="9727a-144">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9727a-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9727a-145">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9727a-146">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="9727a-146">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9727a-147">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="9727a-147">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9727a-148">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9727a-148">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9727a-149">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-149">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9727a-150">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-150">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9727a-151">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="9727a-151">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9727a-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9727a-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9727a-153">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="9727a-153">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9727a-154">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9727a-154">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9727a-155">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9727a-155">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9727a-156">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-156">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9727a-157">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-157">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9727a-158">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9727a-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9727a-159">Appuyez sur **Alt-Cmd-Entrée** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="9727a-159">Press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="9727a-160">Vous pouvez également accéder à la barre de menus et accéder à Exécuter > Démarrer sans débogage.</span><span class="sxs-lookup"><span data-stu-id="9727a-160">Alternatively, navigate to the menu bar and go to Run>Start Without Debugging.</span></span>

  <span data-ttu-id="9727a-161">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9727a-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9727a-162">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="9727a-162">Examine the project files</span></span>

<span data-ttu-id="9727a-163">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="9727a-163">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9727a-164">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="9727a-164">Pages folder</span></span>

<span data-ttu-id="9727a-165">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="9727a-165">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9727a-166">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="9727a-166">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9727a-167">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="9727a-167">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9727a-168">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="9727a-168">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9727a-169">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="9727a-169">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9727a-170">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="9727a-170">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9727a-171">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="9727a-171">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9727a-172">Pour plus d’informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9727a-172">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9727a-173">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="9727a-173">wwwroot folder</span></span>

<span data-ttu-id="9727a-174">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="9727a-174">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9727a-175">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9727a-175">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9727a-176">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9727a-176">appSettings.json</span></span>

<span data-ttu-id="9727a-177">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="9727a-177">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9727a-178">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9727a-178">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9727a-179">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9727a-179">Program.cs</span></span>

<span data-ttu-id="9727a-180">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="9727a-180">Contains the entry point for the program.</span></span> <span data-ttu-id="9727a-181">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9727a-181">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9727a-182">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9727a-182">Startup.cs</span></span>

<span data-ttu-id="9727a-183">contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="9727a-183">Contains code that configures app behavior.</span></span> <span data-ttu-id="9727a-184">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9727a-184">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9727a-185">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9727a-185">Next steps</span></span>

<span data-ttu-id="9727a-186">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="9727a-186">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9727a-187">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="9727a-187">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9727a-188">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="9727a-188">This is the first tutorial of a series.</span></span> <span data-ttu-id="9727a-189">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9727a-189">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="9727a-190">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="9727a-190">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="9727a-191">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="9727a-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9727a-192">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9727a-192">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="9727a-193">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="9727a-193">Run the app.</span></span>
> * <span data-ttu-id="9727a-194">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="9727a-194">Examine the project files.</span></span>

<span data-ttu-id="9727a-195">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="9727a-195">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="9727a-197">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9727a-197">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9727a-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9727a-198">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9727a-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9727a-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9727a-200">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9727a-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="9727a-201">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="9727a-201">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9727a-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9727a-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9727a-203">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="9727a-203">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="9727a-204">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="9727a-204">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="9727a-206">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="9727a-206">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="9727a-207">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="9727a-207">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="9727a-209">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="9727a-209">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="9727a-211">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="9727a-211">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9727a-213">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9727a-213">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9727a-214">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="9727a-214">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="9727a-215">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="9727a-215">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="9727a-216">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9727a-216">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="9727a-217">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="9727a-217">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="9727a-218">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="9727a-218">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="9727a-219">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="9727a-219">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="9727a-220">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="9727a-220">Select **Yes**.</span></span>

  <span data-ttu-id="9727a-221">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="9727a-221">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9727a-222">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9727a-222">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9727a-223">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9727a-223">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="9727a-224">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9727a-224">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="9727a-225">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="9727a-225">Open the project</span></span>

<span data-ttu-id="9727a-226">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="9727a-226">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="9727a-227">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="9727a-227">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9727a-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9727a-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9727a-229">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="9727a-229">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="9727a-230">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="9727a-230">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="9727a-231">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9727a-231">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9727a-232">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-232">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="9727a-233">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-233">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="9727a-234">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="9727a-234">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="9727a-235">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="9727a-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9727a-236">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="9727a-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9727a-238">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="9727a-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9727a-240">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9727a-240">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="9727a-241">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="9727a-241">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9727a-242">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9727a-242">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="9727a-243">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="9727a-243">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="9727a-244">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-244">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="9727a-245">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="9727a-245">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="9727a-246">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="9727a-246">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9727a-247">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="9727a-247">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="9727a-249">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="9727a-249">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9727a-251">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9727a-251">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="9727a-252">Appuyez sur **Cmd+Opt+F5** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="9727a-252">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="9727a-253">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="9727a-253">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="9727a-254">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="9727a-254">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="9727a-255">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="9727a-255">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="9727a-257">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="9727a-257">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="9727a-259">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="9727a-259">Examine the project files</span></span>

<span data-ttu-id="9727a-260">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="9727a-260">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="9727a-261">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="9727a-261">Pages folder</span></span>

<span data-ttu-id="9727a-262">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="9727a-262">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="9727a-263">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="9727a-263">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="9727a-264">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="9727a-264">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="9727a-265">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="9727a-265">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="9727a-266">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="9727a-266">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="9727a-267">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="9727a-267">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="9727a-268">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="9727a-268">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="9727a-269">Pour plus d’informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="9727a-269">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="9727a-270">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="9727a-270">wwwroot folder</span></span>

<span data-ttu-id="9727a-271">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="9727a-271">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="9727a-272">Pour plus d’informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="9727a-272">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="9727a-273">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="9727a-273">appSettings.json</span></span>

<span data-ttu-id="9727a-274">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="9727a-274">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="9727a-275">Pour plus d’informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="9727a-275">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="9727a-276">Program.cs</span><span class="sxs-lookup"><span data-stu-id="9727a-276">Program.cs</span></span>

<span data-ttu-id="9727a-277">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="9727a-277">Contains the entry point for the program.</span></span> <span data-ttu-id="9727a-278">Pour plus d’informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="9727a-278">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="9727a-279">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="9727a-279">Startup.cs</span></span>

<span data-ttu-id="9727a-280">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="9727a-280">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="9727a-281">Pour plus d’informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="9727a-281">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9727a-282">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9727a-282">Additional resources</span></span>

* [<span data-ttu-id="9727a-283">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="9727a-283">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="9727a-284">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9727a-284">Next steps</span></span>

<span data-ttu-id="9727a-285">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="9727a-285">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9727a-286">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="9727a-286">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
