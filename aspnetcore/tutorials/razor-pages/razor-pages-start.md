---
title: Bien démarrer avec des pages Razor dans ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Cette série de tutoriels montre comment utiliser Razor Pages dans ASP.NET Core. Découvrez comment créer un modèle, générer du code pour Razor Pages, utiliser Entity Framework Core et SQL Server pour l’accès aux données, ajouter des fonctionnalités de recherche, ajouter la validation d’entrée et mettre à jour le modèle à l’aide de migrations.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861626"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="b485b-104">Tutoriel : Bien démarrer avec Razor Pages dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b485b-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="b485b-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b485b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b485b-106">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b485b-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="b485b-107">L’application gère une base de données de titres de films.</span><span class="sxs-lookup"><span data-stu-id="b485b-107">The app manages a database of movie titles.</span></span> <span data-ttu-id="b485b-108">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="b485b-108">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b485b-109">Créer une application web Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="b485b-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="b485b-110">Ajouter et structurer un modèle.</span><span class="sxs-lookup"><span data-stu-id="b485b-110">Add and scaffold a model.</span></span>
> * <span data-ttu-id="b485b-111">Utiliser une base de données.</span><span class="sxs-lookup"><span data-stu-id="b485b-111">Work with a database.</span></span>
> * <span data-ttu-id="b485b-112">Ajouter une fonctionnalité de recherche et de validation.</span><span class="sxs-lookup"><span data-stu-id="b485b-112">Add search and validation.</span></span>

<span data-ttu-id="b485b-113">À la fin, vous obtenez une application qui peut gérer et afficher des éléments de titres de films.</span><span class="sxs-lookup"><span data-stu-id="b485b-113">At the end, you have an app that can manage and display movie titles items.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a><span data-ttu-id="b485b-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b485b-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="b485b-115">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="b485b-115">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b485b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b485b-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b485b-117">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="b485b-117">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b485b-118">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b485b-118">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="b485b-119">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="b485b-119">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="b485b-120">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.</span><span class="sxs-lookup"><span data-stu-id="b485b-120">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="b485b-121">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="b485b-121">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>

* <span data-ttu-id="b485b-122">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="b485b-122">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>

  ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="b485b-124">Le projet de démarrage suivant est créé :</span><span class="sxs-lookup"><span data-stu-id="b485b-124">The following starter project is created:</span></span>

  ![Explorateur de solutions](razor-pages-start/_static/se2.2.png)

* <span data-ttu-id="b485b-126">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="b485b-126">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b485b-127">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="b485b-127">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="b485b-128">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b485b-128">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b485b-129">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="b485b-129">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="b485b-130">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="b485b-130">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="b485b-131">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="b485b-131">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b485b-132">Dans l’image précédente, le numéro de port est 5001.</span><span class="sxs-lookup"><span data-stu-id="b485b-132">In the preceding image, the port number is 5001.</span></span> <span data-ttu-id="b485b-133">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="b485b-133">When you run the app, you'll see a different port number.</span></span>

  <span data-ttu-id="b485b-134">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="b485b-134">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b485b-135">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="b485b-135">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b485b-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b485b-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b485b-137">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b485b-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b485b-138">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="b485b-138">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b485b-139">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="b485b-139">Run the following command:</span></span>

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * <span data-ttu-id="b485b-140">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « RazorPagesMovie ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="b485b-140">A dialog box appears with **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span>  <span data-ttu-id="b485b-141">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="b485b-141">Select **Yes**</span></span>

  * <span data-ttu-id="b485b-142">`dotnet new webapp -o RazorPagesMovie` : crée un nouveau projet Razor Pages dans le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="b485b-142">`dotnet new webapp -o RazorPagesMovie`: creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="b485b-143">`code -r RazorPagesMovie` : charge le fichier projet *RazorPagesMovie.csproj* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b485b-143">`code -r RazorPagesMovie`: Loads the *RazorPagesMovie.csproj* project file in Visual Studio Code.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="b485b-144">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="b485b-144">Launch the app</span></span>

* <span data-ttu-id="b485b-145">Appuyez sur **Ctrl+F5** pour exécuter sans le débogueur.</span><span class="sxs-lookup"><span data-stu-id="b485b-145">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b485b-146">Visual Studio Code démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b485b-146">Visual Studio Code starts starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="b485b-147">La barre d’adresses affiche `localhost:port:5001` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b485b-147">The address bar shows `localhost:port:5001` and not something like `example.com`.</span></span> <span data-ttu-id="b485b-148">La raison en est que `localhost` est le nom d’hôte standard de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="b485b-148">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="b485b-149">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="b485b-149">Localhost only serves web requests from the local computer.</span></span>

  <span data-ttu-id="b485b-150">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="b485b-150">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="b485b-151">De nombreux développeurs préfèrent utiliser le mode sans débogage pour actualiser les modifications des pages et des vues.</span><span class="sxs-lookup"><span data-stu-id="b485b-151">Many developers prefer to use non-debug mode to refresh the page and view changes.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b485b-152">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b485b-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b485b-153">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b485b-153">From a terminal, run the following commands:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="b485b-154">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor.</span><span class="sxs-lookup"><span data-stu-id="b485b-154">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="b485b-155">Ouvrez un navigateur sur http://localhost:5000 pour voir l’application.</span><span class="sxs-lookup"><span data-stu-id="b485b-155">Open a browser to http://localhost:5000 to view the application.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="b485b-156">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="b485b-156">Open the project</span></span>

<span data-ttu-id="b485b-157">Appuyez sur Ctrl+C pour arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="b485b-157">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="b485b-158">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b485b-158">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="b485b-159">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="b485b-159">Launch the app</span></span>

<span data-ttu-id="b485b-160">Sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="b485b-160">Select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="b485b-161">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="b485b-161">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---

* <span data-ttu-id="b485b-162">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="b485b-162">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="b485b-163">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="b485b-163">This app doesn't track personal information.</span></span> <span data-ttu-id="b485b-164">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="b485b-164">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="b485b-166">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="b485b-166">The following image shows the app after accepting tracking:</span></span>

  ![Page d’accueil ou page d’index](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a><span data-ttu-id="b485b-168">Dossiers et fichiers projet</span><span class="sxs-lookup"><span data-stu-id="b485b-168">Project files and folders</span></span>

<span data-ttu-id="b485b-169">Le tableau suivant répertorie les fichiers et dossiers du projet.</span><span class="sxs-lookup"><span data-stu-id="b485b-169">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="b485b-170">À ce stade du tutoriel, le plus important est de comprendre le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b485b-170">At this point in the tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="b485b-171">Vous n’avez pas besoin de passer en revue chaque lien ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="b485b-171">You don't need to review each link provided below.</span></span> <span data-ttu-id="b485b-172">Les liens sont fournis en guise de référence quand vous avez besoin d’informations supplémentaires sur un fichier ou un dossier du projet.</span><span class="sxs-lookup"><span data-stu-id="b485b-172">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="b485b-173">Fichier ou dossier</span><span class="sxs-lookup"><span data-stu-id="b485b-173">File or folder</span></span>              | <span data-ttu-id="b485b-174">Objectif</span><span class="sxs-lookup"><span data-stu-id="b485b-174">Purpose</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="b485b-175">*wwwroot*</span><span class="sxs-lookup"><span data-stu-id="b485b-175">*wwwroot*</span></span> | <span data-ttu-id="b485b-176">Contient des fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="b485b-176">Contains static files.</span></span> <span data-ttu-id="b485b-177">Consultez [Fichiers statiques](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="b485b-177">See [Static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="b485b-178">*Pages*</span><span class="sxs-lookup"><span data-stu-id="b485b-178">*Pages*</span></span> | <span data-ttu-id="b485b-179">Dossier pour [Pages Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="b485b-179">Folder for [Razor Pages](xref:razor-pages/index).</span></span> |
| <span data-ttu-id="b485b-180">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="b485b-180">*appsettings.json*</span></span> | [<span data-ttu-id="b485b-181">Configuration</span><span class="sxs-lookup"><span data-stu-id="b485b-181">Configuration</span></span>](xref:fundamentals/configuration/index) |
| <span data-ttu-id="b485b-182">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="b485b-182">*Program.cs*</span></span> | <span data-ttu-id="b485b-183">[Héberge](xref:fundamentals/host/index) l’application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b485b-183">[Hosts](xref:fundamentals/host/index) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="b485b-184">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="b485b-184">*Startup.cs*</span></span> | <span data-ttu-id="b485b-185">Configure les services et le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="b485b-185">Configures services and the request pipeline.</span></span> <span data-ttu-id="b485b-186">Voir [Démarrage](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="b485b-186">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="b485b-187">Le dossier Pages</span><span class="sxs-lookup"><span data-stu-id="b485b-187">The Pages folder</span></span>

<span data-ttu-id="b485b-188">Le fichier *_Layout.cshtml* contient des éléments HTML communs (scripts et feuilles de style) et définit la disposition de l’application.</span><span class="sxs-lookup"><span data-stu-id="b485b-188">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="b485b-189">Par exemple, quand vous cliquez sur **RazorPagesMovie**, **Home** ou **Privacy**, les mêmes éléments apparaissent.</span><span class="sxs-lookup"><span data-stu-id="b485b-189">For example, when you click on **RazorPagesMovie**, **Home**, or **Privacy**, you see the same elements.</span></span> <span data-ttu-id="b485b-190">Les éléments communs incluent le menu de navigation en haut et l’en-tête en bas de la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="b485b-190">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="b485b-191">Pour plus d’informations, consultez [Disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="b485b-191">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="b485b-192">Le fichier *_ViewImports.cshtml* contient des directives Razor qui sont importées dans chaque page Razor.</span><span class="sxs-lookup"><span data-stu-id="b485b-192">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="b485b-193">Pour plus d’informations, consultez [Importation de directives partagées](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="b485b-193">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="b485b-194">Le fichier *_ViewStart.cshtml* définit la propriété `Layout` des pages Razor pour qu’elle utilise le fichier *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b485b-194">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="b485b-195">Pour plus d’informations, consultez [Disposition](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="b485b-195">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="b485b-196">Le fichier *_ValidationScriptsPartial.cshtml* fournit une référence aux scripts de validation [jQuery](https://jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="b485b-196">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="b485b-197">Quand les pages `Create` et `Edit` sont ajoutées plus loin dans ce tutoriel, le fichier *_ValidationScriptsPartial.cshtml* est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b485b-197">When the `Create` and `Edit` pages are added later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="b485b-198">Les pages `Index`, `Error` et `Privacy` sont fournies pour :</span><span class="sxs-lookup"><span data-stu-id="b485b-198">`Index`, `Error`, and `Privacy` pages are provided to:</span></span>

* <span data-ttu-id="b485b-199">`Index` : démarrer une application.</span><span class="sxs-lookup"><span data-stu-id="b485b-199">`Index`: Start an app.</span></span>
* <span data-ttu-id="b485b-200">`Error` : afficher des informations d’erreur.</span><span class="sxs-lookup"><span data-stu-id="b485b-200">`Error`: Display error information.</span></span>
* <span data-ttu-id="b485b-201">`Privacy` : spécifier des informations sur la politique de confidentialité du site.</span><span class="sxs-lookup"><span data-stu-id="b485b-201">`Privacy`: Specify details about the site's privacy policy.</span></span>

<span data-ttu-id="b485b-202">Dans ce tutoriel, les pages précédentes ne sont pas utilisées.</span><span class="sxs-lookup"><span data-stu-id="b485b-202">For this tutorial, the preceding pages are not used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b485b-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b485b-203">Visual Studio</span></span>](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a><span data-ttu-id="b485b-204">Utilisez la touche F7 pour basculer entre une page Razor Pages et le modèle de page</span><span class="sxs-lookup"><span data-stu-id="b485b-204">Use F7 to toggle between a Razor Page and the PageModel</span></span>

<span data-ttu-id="b485b-205">F7 permet de basculer entre une page Razor Pages (fichier *\*.cshtml*) et le fichier C# (*\*.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="b485b-205">F7 toggles between a Razor Page (*\*.cshtml* file) and the C# file (*\*.cshtml.cs*).</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b485b-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b485b-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

<span data-ttu-id="b485b-207">Par convention, la page Razor Pages (fichier *\*.cshtml*) et le `PageModel` associé ont le même nom de fichier racine.</span><span class="sxs-lookup"><span data-stu-id="b485b-207">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b485b-208">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b485b-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b485b-209">Par convention, la page Razor Pages (fichier *\*.cshtml*) et le `PageModel` associé ont le même nom de fichier racine.</span><span class="sxs-lookup"><span data-stu-id="b485b-209">By convention, the Razor Page (*\*.cshtml* file) and the associated `PageModel` have the same root file name.</span></span>

---

> [!div class="step-by-step"]
> [<span data-ttu-id="b485b-210">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="b485b-210">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)