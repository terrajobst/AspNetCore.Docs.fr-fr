---
title: Bien démarrer avec ASP.NET Core MVC et Visual Studio
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC et Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011698"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="8ca3d-103">Bien démarrer avec ASP.NET Core MVC et Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ca3d-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="8ca3d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="8ca3d-105">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="8ca3d-106">macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="8ca3d-107">Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="8ca3d-108">macOS, Linux et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="8ca3d-109">Installer Visual Studio et .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ca3d-109">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="8ca3d-110">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="8ca3d-110">Create a web app</span></span>

<span data-ttu-id="8ca3d-111">Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-111">From Visual Studio, select  **File > New > Project**.</span></span>

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="8ca3d-113">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8ca3d-114">Dans le volet gauche, cliquez sur **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-114">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="8ca3d-115">Dans le volet central, cliquez sur **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-115">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="8ca3d-116">Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).</span><span class="sxs-lookup"><span data-stu-id="8ca3d-116">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="8ca3d-117">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-117">Tap **OK**</span></span>

![<span data-ttu-id="8ca3d-118">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ca3d-118">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="8ca3d-119">Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-119">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8ca3d-120">Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-120">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="8ca3d-121">Sélectionnez **Application web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="8ca3d-121">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="8ca3d-122">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-122">Tap **OK**.</span></span>

![<span data-ttu-id="8ca3d-123">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ca3d-123">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="8ca3d-124">Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-124">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="8ca3d-125">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-125">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8ca3d-126">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-126">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="8ca3d-127">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl-F5** pour l’exécuter en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-127">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="8ca3d-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![application en cours d’exécution](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-128"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="8ca3d-129">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-129">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8ca3d-130">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-130">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8ca3d-131">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-131">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8ca3d-132">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8ca3d-133">Dans l’image ci-dessus, le numéro de port est 5000.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-133">In the image above, the port number is 5000.</span></span> <span data-ttu-id="8ca3d-134">L’URL dans le navigateur affiche `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-134">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="8ca3d-135">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-135">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8ca3d-136">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-136">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8ca3d-137">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-137">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8ca3d-138">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-138">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8ca3d-140">Vous pouvez déboguer l’application en appuyant sur le bouton **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-140">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="8ca3d-142">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-142">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8ca3d-143">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-143">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8ca3d-144">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-144">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icône de navigation en haut à droite](start-mvc/_static/2.png)

<span data-ttu-id="8ca3d-146">Si vous exécutiez l’application en mode débogage, appuyez sur **Maj-F5** pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-146">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="8ca3d-147">Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-147">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8ca3d-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8ca3d-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8ca3d-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8ca3d-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="8ca3d-150">Installez Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-150">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="8ca3d-151">Sélectionnez le téléchargement Community.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-151">Select the Community download.</span></span> <span data-ttu-id="8ca3d-152">Ignorez cette étape si Visual Studio 2017 est installé sur votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-152">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="8ca3d-153">Page d’accueil du programme d’installation de Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8ca3d-153">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="8ca3d-154">Exécutez le programme d’installation et sélectionnez les charges de travail suivantes :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-154">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="8ca3d-155">**ASP.NET et développement web** (sous **Web et cloud**)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-155">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="8ca3d-156">**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-156">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET et développement web** (sous **Web et cloud**)](start-mvc/_static/web_workload.png)

![**Développement multiplateforme .NET Core** (sous **Autres ensembles d’outils**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="8ca3d-159">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="8ca3d-159">Create a web app</span></span>

<span data-ttu-id="8ca3d-160">Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-160">From Visual Studio, select  **File > New > Project**.</span></span>

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="8ca3d-162">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-162">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="8ca3d-163">Dans le volet gauche, cliquez sur **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-163">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="8ca3d-164">Dans le volet central, cliquez sur **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-164">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="8ca3d-165">Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).</span><span class="sxs-lookup"><span data-stu-id="8ca3d-165">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="8ca3d-166">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-166">Tap **OK**</span></span>

![<span data-ttu-id="8ca3d-167">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ca3d-167">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8ca3d-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8ca3d-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8ca3d-169">Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-169">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8ca3d-170">Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-170">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="8ca3d-171">Sélectionnez **Application web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="8ca3d-171">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="8ca3d-172">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-172">Tap **OK**.</span></span>

![<span data-ttu-id="8ca3d-173">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ca3d-173">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8ca3d-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8ca3d-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8ca3d-175">Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-175">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="8ca3d-176">Dans la zone de liste déroulante du sélecteur de version, appuyez sur **ASP.NET Core 1.1**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-176">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="8ca3d-177">Appuyez sur **Application web**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-177">Tap **Web Application**</span></span>
* <span data-ttu-id="8ca3d-178">Conservez la valeur par défaut **Aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-178">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="8ca3d-179">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-179">Tap **OK**.</span></span>

![Nouvelle application web ASP.NET Core](start-mvc/_static/p3.png)

---

<span data-ttu-id="8ca3d-181">Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-181">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="8ca3d-182">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-182">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="8ca3d-183">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-183">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="8ca3d-184">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl-F5** pour l’exécuter en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-184">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="8ca3d-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![application en cours d’exécution](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="8ca3d-185"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="8ca3d-186">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-186">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8ca3d-187">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-187">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8ca3d-188">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-188">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8ca3d-189">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-189">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8ca3d-190">Dans l’image ci-dessus, le numéro de port est 5000.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-190">In the image above, the port number is 5000.</span></span> <span data-ttu-id="8ca3d-191">L’URL dans le navigateur affiche `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-191">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="8ca3d-192">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-192">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8ca3d-193">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-193">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8ca3d-194">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-194">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="8ca3d-195">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="8ca3d-195">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="8ca3d-197">Vous pouvez déboguer l’application en appuyant sur le bouton **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-197">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="8ca3d-199">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-199">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="8ca3d-200">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-200">The browser image above doesn't show these links.</span></span> <span data-ttu-id="8ca3d-201">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-201">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icône de navigation en haut à droite](start-mvc/_static/2.png)

<span data-ttu-id="8ca3d-203">Si vous exécutiez l’application en mode débogage, appuyez sur **Maj-F5** pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-203">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="8ca3d-204">Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="8ca3d-204">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="8ca3d-205">Next</span><span class="sxs-lookup"><span data-stu-id="8ca3d-205">Next</span></span>](adding-controller.md)  
