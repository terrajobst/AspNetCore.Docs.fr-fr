---
title: 'Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 05/30/2019
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e9f11f68aa138ab74a0ffbbd0e32067bc984606d
ms.sourcegitcommit: 9ae1fd11f39b0a72b2ae42f0b450345e6e306bc0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66415659"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="d4959-104">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4959-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="d4959-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d4959-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d4959-106">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="d4959-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="d4959-107">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4959-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="d4959-108">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="d4959-108">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="d4959-109">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d4959-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d4959-110">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d4959-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="d4959-111">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="d4959-111">Run the app.</span></span>
> * <span data-ttu-id="d4959-112">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="d4959-112">Examine the project files.</span></span>

<span data-ttu-id="d4959-113">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="d4959-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="d4959-115">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="d4959-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d4959-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4959-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4959-117">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="d4959-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="d4959-118">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4959-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="d4959-119">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="d4959-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="d4959-120">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="d4959-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="d4959-122">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="d4959-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="d4959-124">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="d4959-124">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d4959-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4959-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d4959-127">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="d4959-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="d4959-128">Accédez au répertoire (`cd`) qui contiendra le projet.</span><span class="sxs-lookup"><span data-stu-id="d4959-128">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="d4959-129">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="d4959-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="d4959-130">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="d4959-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="d4959-131">La commande `code` ouvre le dossier *RazorPagesMovie* dans l’instance actuelle de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d4959-131">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="d4959-132">Après que l’icône en forme de flamme de la barre d’état d’OmniSharp devient verte, une boîte de dialogue indique **Les ressources requises pour créer et déboguer sont manquantes dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="d4959-132">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="d4959-133">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="d4959-133">Select **Yes**.</span></span>

  <span data-ttu-id="d4959-134">Un répertoire *.vscode* contenant des fichiers *launch.json* et *tasks.json* est ajouté au répertoire racine du projet.</span><span class="sxs-lookup"><span data-stu-id="d4959-134">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d4959-135">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d4959-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d4959-136">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d4959-136">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
```

<span data-ttu-id="d4959-137">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d4959-137">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="d4959-138">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="d4959-138">Open the project</span></span>

<span data-ttu-id="d4959-139">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="d4959-139">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="d4959-140">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="d4959-140">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d4959-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4959-141">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4959-142">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d4959-142">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="d4959-143">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="d4959-143">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="d4959-144">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d4959-144">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d4959-145">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d4959-145">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="d4959-146">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d4959-146">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="d4959-147">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="d4959-147">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="d4959-148">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="d4959-148">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d4959-149">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="d4959-149">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="d4959-151">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="d4959-151">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d4959-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4959-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="d4959-154">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d4959-154">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="d4959-155">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4959-155">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="d4959-156">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="d4959-156">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="d4959-157">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d4959-157">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="d4959-158">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="d4959-158">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="d4959-159">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="d4959-159">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d4959-160">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="d4959-160">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="d4959-162">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="d4959-162">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d4959-164">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d4959-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="d4959-165">Appuyez sur **Cmd+Opt+F5** pour lancer l’exécution sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="d4959-165">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="d4959-166">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4959-166">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="d4959-167">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="d4959-167">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="d4959-168">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="d4959-168">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="d4959-170">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="d4959-170">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="d4959-172">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="d4959-172">Examine the project files</span></span>

<span data-ttu-id="d4959-173">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="d4959-173">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="d4959-174">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="d4959-174">Pages folder</span></span>

<span data-ttu-id="d4959-175">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="d4959-175">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="d4959-176">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="d4959-176">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="d4959-177">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d4959-177">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="d4959-178">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="d4959-178">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="d4959-179">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="d4959-179">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="d4959-180">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="d4959-180">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="d4959-181">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="d4959-181">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="d4959-182">Pour plus d'informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="d4959-182">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="d4959-183">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="d4959-183">wwwroot folder</span></span>

<span data-ttu-id="d4959-184">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="d4959-184">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="d4959-185">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="d4959-185">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="d4959-186">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="d4959-186">appSettings.json</span></span>

<span data-ttu-id="d4959-187">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="d4959-187">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="d4959-188">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="d4959-188">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="d4959-189">Program.cs</span><span class="sxs-lookup"><span data-stu-id="d4959-189">Program.cs</span></span>

<span data-ttu-id="d4959-190">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="d4959-190">Contains the entry point for the program.</span></span> <span data-ttu-id="d4959-191">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="d4959-191">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="d4959-192">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="d4959-192">Startup.cs</span></span>

<span data-ttu-id="d4959-193">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="d4959-193">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="d4959-194">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d4959-194">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4959-195">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d4959-195">Additional resources</span></span>

* [<span data-ttu-id="d4959-196">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="d4959-196">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="d4959-197">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d4959-197">Next steps</span></span>

<span data-ttu-id="d4959-198">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="d4959-198">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d4959-199">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d4959-199">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="d4959-200">Exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="d4959-200">Ran the app.</span></span>
> * <span data-ttu-id="d4959-201">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="d4959-201">Examined the project files.</span></span>

<span data-ttu-id="d4959-202">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="d4959-202">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d4959-203">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="d4959-203">Add a model</span></span>](xref:tutorials/razor-pages/model)
