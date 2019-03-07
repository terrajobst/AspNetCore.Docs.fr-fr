---
title: 'Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 29d9369cfa6a4c76f015b5a819a27dfa280d4075
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346409"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="be3ea-104">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be3ea-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="be3ea-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="be3ea-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="be3ea-106">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="be3ea-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="be3ea-107">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="be3ea-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="be3ea-108">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="be3ea-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="be3ea-109">Dans ce didacticiel, vous allez effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="be3ea-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="be3ea-110">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="be3ea-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="be3ea-111">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="be3ea-111">Run the app.</span></span>
> * <span data-ttu-id="be3ea-112">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="be3ea-112">Examine the project files.</span></span>

<span data-ttu-id="be3ea-113">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="be3ea-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="be3ea-115">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="be3ea-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be3ea-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be3ea-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be3ea-117">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="be3ea-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="be3ea-118">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="be3ea-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="be3ea-119">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="be3ea-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="be3ea-120">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="be3ea-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="be3ea-122">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="be3ea-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="be3ea-124">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="be3ea-124">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be3ea-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be3ea-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="be3ea-127">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="be3ea-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="be3ea-128">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="be3ea-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="be3ea-129">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="be3ea-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="be3ea-130">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="be3ea-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="be3ea-131">La commande `code` ouvre le dossier *RazorPagesMovie* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="be3ea-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="be3ea-132">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="be3ea-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="be3ea-133">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="be3ea-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be3ea-134">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="be3ea-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="be3ea-135">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="be3ea-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="be3ea-136">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="be3ea-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="be3ea-137">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="be3ea-137">Open the project</span></span>

<span data-ttu-id="be3ea-138">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="be3ea-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="be3ea-139">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="be3ea-139">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be3ea-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be3ea-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="be3ea-141">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="be3ea-141">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="be3ea-142">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="be3ea-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="be3ea-143">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="be3ea-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="be3ea-144">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="be3ea-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="be3ea-145">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="be3ea-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="be3ea-146">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="be3ea-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be3ea-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be3ea-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="be3ea-148">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="be3ea-148">Press **Ctrl-F5** to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  <span data-ttu-id="be3ea-149">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="be3ea-149">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="be3ea-150">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="be3ea-150">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="be3ea-151">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="be3ea-151">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="be3ea-152">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="be3ea-152">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be3ea-153">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="be3ea-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="be3ea-154">Sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="be3ea-154">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="be3ea-155">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="be3ea-155">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* <span data-ttu-id="be3ea-156">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="be3ea-156">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="be3ea-157">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="be3ea-157">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="be3ea-159">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="be3ea-159">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="be3ea-161">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="be3ea-161">Examine the project files</span></span>

<span data-ttu-id="be3ea-162">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="be3ea-162">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="be3ea-163">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="be3ea-163">Pages folder</span></span>

<span data-ttu-id="be3ea-164">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="be3ea-164">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="be3ea-165">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="be3ea-165">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="be3ea-166">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="be3ea-166">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="be3ea-167">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="be3ea-167">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="be3ea-168">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="be3ea-168">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="be3ea-169">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="be3ea-169">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="be3ea-170">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="be3ea-170">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="be3ea-171">Pour plus d'informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="be3ea-171">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="be3ea-172">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="be3ea-172">wwwroot folder</span></span>

<span data-ttu-id="be3ea-173">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="be3ea-173">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="be3ea-174">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="be3ea-174">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="be3ea-175">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="be3ea-175">appSettings.json</span></span>

<span data-ttu-id="be3ea-176">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="be3ea-176">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="be3ea-177">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="be3ea-177">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="be3ea-178">Program.cs</span><span class="sxs-lookup"><span data-stu-id="be3ea-178">Program.cs</span></span>

<span data-ttu-id="be3ea-179">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="be3ea-179">Contains the entry point for the program.</span></span> <span data-ttu-id="be3ea-180">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="be3ea-180">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="be3ea-181">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="be3ea-181">Startup.cs</span></span>

<span data-ttu-id="be3ea-182">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="be3ea-182">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="be3ea-183">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="be3ea-183">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be3ea-184">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="be3ea-184">Additional resources</span></span>

* [<span data-ttu-id="be3ea-185">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="be3ea-185">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="be3ea-186">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="be3ea-186">Next steps</span></span>

<span data-ttu-id="be3ea-187">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="be3ea-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="be3ea-188">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="be3ea-188">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="be3ea-189">Exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="be3ea-189">Ran the app.</span></span>
> * <span data-ttu-id="be3ea-190">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="be3ea-190">Examined the project files.</span></span>

<span data-ttu-id="be3ea-191">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="be3ea-191">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="be3ea-192">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="be3ea-192">Add a model</span></span>](xref:tutorials/razor-pages/model)
