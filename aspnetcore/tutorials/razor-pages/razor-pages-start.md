---
title: 'Tutoriel : Bien démarrer avec Razor Pages dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 11/12/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: b651437b698d01310f90c5f14832616c1896e6c0
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959097"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="475e6-104">Tutoriel : Bien démarrer avec Razor Pages dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="475e6-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="475e6-105">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="475e6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="475e6-106">C’est le premier d’une série de tutoriels, qui décrit les principes fondamentaux liés à la génération d’une application web de Razor Pages dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="475e6-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="475e6-107">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="475e6-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="475e6-108">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="475e6-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="475e6-109">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475e6-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="475e6-110">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="475e6-110">Run the app.</span></span>
> * <span data-ttu-id="475e6-111">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="475e6-111">Examine the project files.</span></span>

<span data-ttu-id="475e6-112">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="475e6-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="475e6-114">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="475e6-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="475e6-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="475e6-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="475e6-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="475e6-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="475e6-117">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="475e6-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="475e6-118">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="475e6-118">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="475e6-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="475e6-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="475e6-120">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="475e6-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="475e6-121">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="475e6-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="475e6-122">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="475e6-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="475e6-123">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="475e6-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="475e6-124">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="475e6-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="475e6-125">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="475e6-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="475e6-126">Sélectionnez **ASP.NET Core 3,1** dans la liste déroulante, **application Web**, puis sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="475e6-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="475e6-128">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="475e6-128">The following starter project is created:</span></span>

  ![l'Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="475e6-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="475e6-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="475e6-131">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="475e6-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="475e6-132">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="475e6-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="475e6-133">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="475e6-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="475e6-134">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="475e6-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="475e6-135">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="475e6-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="475e6-136">Une fois que l’icône de flamme OmniSharp de la barre d’État devient verte, une boîte de dialogue demande les **ressources requises à générer et à déboguer manquantes dans « RazorPagesMovie ». Ajoutez-les ?**</span><span class="sxs-lookup"><span data-stu-id="475e6-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="475e6-137">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="475e6-137">Select **Yes**.</span></span>

  <span data-ttu-id="475e6-138">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="475e6-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="475e6-139">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="475e6-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="475e6-140">Sélectionnez **Fichier** > **Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="475e6-140">Select **File** > **New Solution**.</span></span>

![macOS - Nouvelle solution](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="475e6-142">Sélectionnez **.NET Core** > **Application** > **Application web** > **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="475e6-142">Select **.NET Core** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS - Boîte de dialogue Nouveau projet](razor-pages-start/_static/webapp.png)

* <span data-ttu-id="475e6-144">Dans la boîte de dialogue **configurer votre nouvelle API Web ASP.net Core** , définissez **Framework cible** sur **.net Core 3,1**.</span><span class="sxs-lookup"><span data-stu-id="475e6-144">In the **Configure your new ASP.NET Core Web API** dialog, set the  **Target Framework** to **.NET Core 3.1**.</span></span>

  ![Sélection de .NET Core 3.0 pour macOS](razor-pages-start/_static/targetframework3.png)

* <span data-ttu-id="475e6-146">Nommez le projet **RazorPagesMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="475e6-146">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)


## <a name="open-the-project"></a><span data-ttu-id="475e6-148">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="475e6-148">Open the project</span></span>

<span data-ttu-id="475e6-149">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="475e6-149">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="475e6-150">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="475e6-150">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="475e6-151">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="475e6-151">Examine the project files</span></span>

<span data-ttu-id="475e6-152">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="475e6-152">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="475e6-153">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="475e6-153">Pages folder</span></span>

<span data-ttu-id="475e6-154">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="475e6-154">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="475e6-155">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="475e6-155">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="475e6-156">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="475e6-156">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="475e6-157">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="475e6-157">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="475e6-158">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="475e6-158">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="475e6-159">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="475e6-159">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="475e6-160">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="475e6-160">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="475e6-161">Pour plus d'informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="475e6-161">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="475e6-162">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="475e6-162">wwwroot folder</span></span>

<span data-ttu-id="475e6-163">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="475e6-163">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="475e6-164">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="475e6-164">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="475e6-165">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="475e6-165">appSettings.json</span></span>

<span data-ttu-id="475e6-166">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="475e6-166">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="475e6-167">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="475e6-167">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="475e6-168">Program.cs</span><span class="sxs-lookup"><span data-stu-id="475e6-168">Program.cs</span></span>

<span data-ttu-id="475e6-169">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="475e6-169">Contains the entry point for the program.</span></span> <span data-ttu-id="475e6-170">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="475e6-170">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="475e6-171">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="475e6-171">Startup.cs</span></span>

<span data-ttu-id="475e6-172">contient le code qui configure le comportement de l’application.</span><span class="sxs-lookup"><span data-stu-id="475e6-172">Contains code that configures app behavior.</span></span> <span data-ttu-id="475e6-173">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="475e6-173">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="475e6-174">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="475e6-174">Next steps</span></span>

<span data-ttu-id="475e6-175">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="475e6-175">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="475e6-176">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="475e6-176">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="475e6-177">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="475e6-177">This is the first tutorial of a series.</span></span> <span data-ttu-id="475e6-178">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="475e6-178">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="475e6-179">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="475e6-179">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="475e6-180">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="475e6-180">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="475e6-181">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475e6-181">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="475e6-182">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="475e6-182">Run the app.</span></span>
> * <span data-ttu-id="475e6-183">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="475e6-183">Examine the project files.</span></span>

<span data-ttu-id="475e6-184">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="475e6-184">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou d’index](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="475e6-186">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="475e6-186">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="475e6-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="475e6-187">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="475e6-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="475e6-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="475e6-189">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="475e6-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="475e6-190">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="475e6-190">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="475e6-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="475e6-191">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="475e6-192">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="475e6-192">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="475e6-193">Créez une application web ASP.NET Core, puis sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="475e6-193">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="475e6-195">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="475e6-195">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="475e6-196">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="475e6-196">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="475e6-198">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, sélectionnez **Application web**, puis **Créer**.</span><span class="sxs-lookup"><span data-stu-id="475e6-198">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="475e6-200">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="475e6-200">The following starter project is created:</span></span>

  ![l'Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="475e6-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="475e6-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="475e6-203">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="475e6-203">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="475e6-204">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="475e6-204">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="475e6-205">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="475e6-205">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="475e6-206">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="475e6-206">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="475e6-207">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="475e6-207">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="475e6-208">Une fois que l’icône de flamme OmniSharp de la barre d’État devient verte, une boîte de dialogue demande les **ressources requises à générer et à déboguer manquantes dans « RazorPagesMovie ». Ajoutez-les ?**</span><span class="sxs-lookup"><span data-stu-id="475e6-208">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="475e6-209">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="475e6-209">Select **Yes**.</span></span>

  <span data-ttu-id="475e6-210">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="475e6-210">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="475e6-211">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="475e6-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="475e6-212">Dans un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="475e6-212">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```dotnetcli
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="475e6-213">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="475e6-213">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="475e6-214">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="475e6-214">Open the project</span></span>

<span data-ttu-id="475e6-215">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="475e6-215">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="475e6-216">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="475e6-216">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="475e6-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="475e6-217">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="475e6-218">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="475e6-218">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="475e6-219">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="475e6-219">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="475e6-220">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="475e6-220">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="475e6-221">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="475e6-221">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="475e6-222">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="475e6-222">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="475e6-223">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="475e6-223">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="475e6-224">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="475e6-224">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="475e6-225">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="475e6-225">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="475e6-227">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="475e6-227">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="475e6-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="475e6-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="475e6-230">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="475e6-230">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="475e6-231">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="475e6-231">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="475e6-232">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="475e6-232">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="475e6-233">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="475e6-233">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="475e6-234">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="475e6-234">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="475e6-235">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="475e6-235">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="475e6-236">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="475e6-236">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="475e6-238">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="475e6-238">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="475e6-240">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="475e6-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="475e6-241">Appuyez sur **Cmd+Opt+F5** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="475e6-241">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="475e6-242">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="475e6-242">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="475e6-243">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="475e6-243">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="475e6-244">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="475e6-244">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou d’index](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="475e6-246">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="475e6-246">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou d’index](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="475e6-248">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="475e6-248">Examine the project files</span></span>

<span data-ttu-id="475e6-249">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="475e6-249">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="475e6-250">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="475e6-250">Pages folder</span></span>

<span data-ttu-id="475e6-251">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="475e6-251">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="475e6-252">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="475e6-252">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="475e6-253">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="475e6-253">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="475e6-254">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="475e6-254">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="475e6-255">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="475e6-255">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="475e6-256">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="475e6-256">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="475e6-257">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="475e6-257">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="475e6-258">Pour plus d'informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="475e6-258">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="475e6-259">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="475e6-259">wwwroot folder</span></span>

<span data-ttu-id="475e6-260">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="475e6-260">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="475e6-261">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="475e6-261">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="475e6-262">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="475e6-262">appSettings.json</span></span>

<span data-ttu-id="475e6-263">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="475e6-263">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="475e6-264">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="475e6-264">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="475e6-265">Program.cs</span><span class="sxs-lookup"><span data-stu-id="475e6-265">Program.cs</span></span>

<span data-ttu-id="475e6-266">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="475e6-266">Contains the entry point for the program.</span></span> <span data-ttu-id="475e6-267">Pour plus d'informations, consultez <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="475e6-267">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="475e6-268">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="475e6-268">Startup.cs</span></span>

<span data-ttu-id="475e6-269">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="475e6-269">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="475e6-270">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="475e6-270">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="475e6-271">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="475e6-271">Additional resources</span></span>

* [<span data-ttu-id="475e6-272">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="475e6-272">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="475e6-273">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="475e6-273">Next steps</span></span>

<span data-ttu-id="475e6-274">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="475e6-274">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="475e6-275">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="475e6-275">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
