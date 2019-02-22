---
title: 'Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core'
author: rick-anderson
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: acebe52719e1876dc6808441fcff6fe849e36983
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410412"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="aa9bd-104">Tutoriel : Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa9bd-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="aa9bd-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa9bd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa9bd-106">Ceci est le premier didacticiel d’une série.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-106">This is the first tutorial of a series.</span></span> <span data-ttu-id="aa9bd-107">[Cette série](xref:tutorials/razor-pages/index) décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-107">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="aa9bd-108">À la fin de la série, vous disposez d’une application qui gère une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-108">At the end of the series you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="aa9bd-109">Dans ce didacticiel, vous allez effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="aa9bd-110">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="aa9bd-111">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-111">Run the app.</span></span>
> * <span data-ttu-id="aa9bd-112">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-112">Examine the project files.</span></span>

<span data-ttu-id="aa9bd-113">À la fin de ce didacticiel, vous disposez d’une application web Razor Pages fonctionnelle et générée dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-113">At the end of this tutorial you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="aa9bd-115">Créer une application web Pages Razor</span><span class="sxs-lookup"><span data-stu-id="aa9bd-115">Create a Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aa9bd-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aa9bd-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="aa9bd-117">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="aa9bd-118">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="aa9bd-119">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="aa9bd-120">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez et collez du code.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="aa9bd-122">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="aa9bd-124">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-124">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aa9bd-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aa9bd-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="aa9bd-127">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="aa9bd-127">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="aa9bd-128">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-128">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="aa9bd-129">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-129">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="aa9bd-130">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-130">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="aa9bd-131">La commande `code` ouvre le dossier *RazorPagesMovie* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-131">The `code` command opens the *RazorPagesMovie* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="aa9bd-132">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="aa9bd-132">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>

* <span data-ttu-id="aa9bd-133">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-133">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aa9bd-134">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="aa9bd-134">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="aa9bd-135">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-135">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

<span data-ttu-id="aa9bd-136">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-136">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="aa9bd-137">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="aa9bd-137">Open the project</span></span>

<span data-ttu-id="aa9bd-138">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-138">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="run-the-web-app"></a><span data-ttu-id="aa9bd-139">Exécuter l'application web</span><span class="sxs-lookup"><span data-stu-id="aa9bd-139">Run the web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aa9bd-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aa9bd-140">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="aa9bd-141">Appuyez sur Ctrl+F5 pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-141">Press Ctrl+F5 to run without the debugger.</span></span>

  <span data-ttu-id="aa9bd-142">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-142">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="aa9bd-143">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-143">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="aa9bd-144">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-144">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="aa9bd-145">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-145">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="aa9bd-146">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-146">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="aa9bd-147">Dans l’image précédente, le numéro de port est 5001.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-147">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="aa9bd-148">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-148">When you run the app, you'll see a different port number.</span></span>
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aa9bd-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aa9bd-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="aa9bd-150">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-150">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="aa9bd-151">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-151">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="aa9bd-152">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-152">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="aa9bd-153">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-153">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="aa9bd-154">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-154">Localhost only serves web requests from the local computer.</span></span>
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aa9bd-155">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="aa9bd-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="aa9bd-156">Sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-156">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="aa9bd-157">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-157">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="aa9bd-158">Sur la page d'accueil de l’application, sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-158">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="aa9bd-159">Cette application ne suit pas les informations personnelles, mais le modèle de projet inclut la fonctionnalité de consentement en cas de besoin pour vous conformer au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr) de l’Union européenne.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-159">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="aa9bd-161">L’illustration suivante montre l’application après avoir consenti au suivi :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-161">The following image shows the app after you give consent to tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a><span data-ttu-id="aa9bd-163">Examiner les fichiers projet</span><span class="sxs-lookup"><span data-stu-id="aa9bd-163">Examine the project files</span></span>

<span data-ttu-id="aa9bd-164">Voici une vue d’ensemble des principaux dossiers et fichiers projet que vous allez utiliser dans les didacticiels suivants.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-164">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="aa9bd-165">Dossier Pages</span><span class="sxs-lookup"><span data-stu-id="aa9bd-165">Pages folder</span></span>

<span data-ttu-id="aa9bd-166">Contient les pages Razor et les fichiers de prise en charge.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-166">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="aa9bd-167">Chaque page Razor est une paire de fichiers :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-167">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="aa9bd-168">Un fichier *.cshtml* qui contient le balisage HTML avec du code C# en utilisant la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-168">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="aa9bd-169">Un fichier *. cshtml.cs* qui contient du code C# gérant les événements de page.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-169">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="aa9bd-170">Les fichiers de prise en charge ont des noms commençant par un trait de soulignement.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-170">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="aa9bd-171">Par exemple, le fichier *_Layout.cshtml* configure les éléments d’interface communs à toutes les pages.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-171">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="aa9bd-172">Ce fichier définit le menu de navigation en haut de la page et la mention de copyright au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-172">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="aa9bd-173">Pour plus d'informations, consultez <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-173">For more information, see <xref:mvc/views/layout>.</span></span>


### <a name="wwwroot-folder"></a><span data-ttu-id="aa9bd-174">Dossier racine</span><span class="sxs-lookup"><span data-stu-id="aa9bd-174">wwwroot folder</span></span>

<span data-ttu-id="aa9bd-175">Contient des fichiers statiques, tels que les fichiers HTML, JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-175">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="aa9bd-176">Pour plus d'informations, consultez <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-176">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="aa9bd-177">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="aa9bd-177">appSettings.json</span></span>

<span data-ttu-id="aa9bd-178">Contient les données de configuration, comme les chaînes de connexion.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-178">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="aa9bd-179">Pour plus d'informations, consultez <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-179">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="aa9bd-180">Program.cs</span><span class="sxs-lookup"><span data-stu-id="aa9bd-180">Program.cs</span></span>

<span data-ttu-id="aa9bd-181">Contient le point d’entrée pour le programme.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-181">Contains the entry point for the program.</span></span> <span data-ttu-id="aa9bd-182">Pour plus d'informations, consultez <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-182">For more information, see <xref:fundamentals/host/web-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="aa9bd-183">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="aa9bd-183">Startup.cs</span></span>

<span data-ttu-id="aa9bd-184">Contient le code qui configure le comportement de l’application, comme le fait qu’elle exige un consentement pour les cookies.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-184">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="aa9bd-185">Pour plus d'informations, consultez <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-185">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa9bd-186">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="aa9bd-186">Next steps</span></span>

<span data-ttu-id="aa9bd-187">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-187">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="aa9bd-188">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-188">Created a Razor Pages web app.</span></span>
> * <span data-ttu-id="aa9bd-189">Exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-189">Ran the app.</span></span>
> * <span data-ttu-id="aa9bd-190">Examiner les fichiers projet.</span><span class="sxs-lookup"><span data-stu-id="aa9bd-190">Examined the project files.</span></span>

<span data-ttu-id="aa9bd-191">Passez au tutoriel suivant dans la série :</span><span class="sxs-lookup"><span data-stu-id="aa9bd-191">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="aa9bd-192">Ajouter un modèle</span><span class="sxs-lookup"><span data-stu-id="aa9bd-192">Add a model</span></span>](xref:tutorials/razor-pages/model)
