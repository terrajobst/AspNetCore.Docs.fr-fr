---
title: Bien démarrer avec ASP.NET Core MVC et Visual Studio
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC et Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 738c49272c2ae2b075866001f06ad09fe73969f9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862198"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="e9189-103">Bien démarrer avec ASP.NET Core MVC et Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9189-103">Get started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="e9189-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e9189-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

<span data-ttu-id="e9189-105">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="e9189-105">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e9189-106">macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e9189-106">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="e9189-107">Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e9189-107">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e9189-108">macOS, Linux et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e9189-108">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

> [!NOTE]
> <span data-ttu-id="e9189-109">Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e9189-109">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="e9189-110">Si vous avez quelques minutes pour essayer un exercice de recherche de 7 différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="e9189-110">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="e9189-111">Installer Visual Studio et .NET Core</span><span class="sxs-lookup"><span data-stu-id="e9189-111">Install Visual Studio and .NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="e9189-112">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="e9189-112">Create a web app</span></span>

<span data-ttu-id="e9189-113">Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="e9189-113">From Visual Studio, select  **File > New > Project**.</span></span>

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="e9189-115">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="e9189-115">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e9189-116">Dans le volet gauche, cliquez sur **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e9189-116">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="e9189-117">Dans le volet central, cliquez sur **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="e9189-117">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="e9189-118">Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).</span><span class="sxs-lookup"><span data-stu-id="e9189-118">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="e9189-119">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9189-119">Tap **OK**</span></span>

![<span data-ttu-id="e9189-120">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9189-120">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2-21.png)

<span data-ttu-id="e9189-121">Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :</span><span class="sxs-lookup"><span data-stu-id="e9189-121">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e9189-122">Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="e9189-122">In the version selector drop-down box select **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="e9189-123">Sélectionnez **Application web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="e9189-123">Select **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="e9189-124">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9189-124">Tap **OK**.</span></span>

![<span data-ttu-id="e9189-125">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9189-125">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22-21.png)

<span data-ttu-id="e9189-126">Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="e9189-126">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="e9189-127">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="e9189-127">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e9189-128">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="e9189-128">This is a basic starter project, and it's a good place to start.</span></span>

<span data-ttu-id="e9189-129">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl-F5** pour l’exécuter en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="e9189-129">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="e9189-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![application en cours d’exécution](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="e9189-130"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="e9189-131">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="e9189-131">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e9189-132">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e9189-132">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e9189-133">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="e9189-133">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e9189-134">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="e9189-134">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e9189-135">Dans l’image ci-dessus, le numéro de port est 5000.</span><span class="sxs-lookup"><span data-stu-id="e9189-135">In the image above, the port number is 5000.</span></span> <span data-ttu-id="e9189-136">L’URL dans le navigateur affiche `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9189-136">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="e9189-137">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="e9189-137">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e9189-138">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="e9189-138">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e9189-139">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="e9189-139">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e9189-140">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="e9189-140">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e9189-142">Vous pouvez déboguer l’application en appuyant sur le bouton **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="e9189-142">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="e9189-144">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="e9189-144">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e9189-145">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="e9189-145">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e9189-146">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="e9189-146">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icône de navigation en haut à droite](start-mvc/_static/2.png)

<span data-ttu-id="e9189-148">Si vous exécutiez l’application en mode débogage, appuyez sur **Maj-F5** pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="e9189-148">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="e9189-149">Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="e9189-149">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="e9189-150">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="e9189-150">Create a web app</span></span>

<span data-ttu-id="e9189-151">Dans Visual Studio, sélectionnez **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="e9189-151">From Visual Studio, select  **File > New > Project**.</span></span>

![Fichier > Nouveau > Projet](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="e9189-153">Renseignez la boîte de dialogue **Nouveau projet** :</span><span class="sxs-lookup"><span data-stu-id="e9189-153">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="e9189-154">Dans le volet gauche, cliquez sur **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e9189-154">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="e9189-155">Dans le volet central, cliquez sur **Application web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="e9189-155">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="e9189-156">Nommez le projet « MvcMovie » (ceci est important pour que l’espace de noms corresponde quand vous copierez le code).</span><span class="sxs-lookup"><span data-stu-id="e9189-156">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="e9189-157">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9189-157">Tap **OK**</span></span>

![<span data-ttu-id="e9189-158">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9189-158">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)

<span data-ttu-id="e9189-159">Renseignez la boîte de dialogue **Nouvelle application web ASP.NET Core (.NET Core) - MvcMovie** :</span><span class="sxs-lookup"><span data-stu-id="e9189-159">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="e9189-160">Dans la zone de liste déroulante du sélecteur de version, sélectionnez **ASP.NET Core 2.-**.</span><span class="sxs-lookup"><span data-stu-id="e9189-160">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="e9189-161">Sélectionnez **Application web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="e9189-161">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="e9189-162">Appuyez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="e9189-162">Tap **OK**.</span></span>

![<span data-ttu-id="e9189-163">Boîte de dialogue Nouveau projet, .Net Core dans le volet gauche, web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9189-163">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

<span data-ttu-id="e9189-164">Visual Studio a utilisé un modèle par défaut pour le projet MVC que vous venez de créer.</span><span class="sxs-lookup"><span data-stu-id="e9189-164">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="e9189-165">Vous disposez maintenant d’une application fonctionnelle en entrant un nom de projet et en sélectionnant quelques options.</span><span class="sxs-lookup"><span data-stu-id="e9189-165">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="e9189-166">Il s’agit d’un projet de démarrage de base qui constitue un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="e9189-166">This is a basic starter project, and it's a good place to start,</span></span>

<span data-ttu-id="e9189-167">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl-F5** pour l’exécuter en mode non-débogage.</span><span class="sxs-lookup"><span data-stu-id="e9189-167">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<span data-ttu-id="e9189-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![application en cours d’exécution](start-mvc/_static/1.png)</span><span class="sxs-lookup"><span data-stu-id="e9189-168"><!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![running app](start-mvc/_static/1.png)</span></span>

* <span data-ttu-id="e9189-169">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="e9189-169">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="e9189-170">Notez que la barre d’adresse affiche `localhost:port#`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="e9189-170">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="e9189-171">C’est parce que `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="e9189-171">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="e9189-172">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="e9189-172">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="e9189-173">Dans l’image ci-dessus, le numéro de port est 5000.</span><span class="sxs-lookup"><span data-stu-id="e9189-173">In the image above, the port number is 5000.</span></span> <span data-ttu-id="e9189-174">L’URL dans le navigateur affiche `localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e9189-174">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="e9189-175">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="e9189-175">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="e9189-176">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="e9189-176">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="e9189-177">De nombreux développeurs préfèrent utiliser le mode non-débogage pour lancer rapidement l’application et voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="e9189-177">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="e9189-178">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir de l’élément de menu **Déboguer** :</span><span class="sxs-lookup"><span data-stu-id="e9189-178">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![Menu Déboguer](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="e9189-180">Vous pouvez déboguer l’application en appuyant sur le bouton **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="e9189-180">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="e9189-182">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="e9189-182">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="e9189-183">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="e9189-183">The browser image above doesn't show these links.</span></span> <span data-ttu-id="e9189-184">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="e9189-184">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icône de navigation en haut à droite](start-mvc/_static/2.png)

<span data-ttu-id="e9189-186">Si vous exécutiez l’application en mode débogage, appuyez sur **Maj-F5** pour arrêter le débogage.</span><span class="sxs-lookup"><span data-stu-id="e9189-186">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="e9189-187">Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="e9189-187">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="e9189-188">Next</span><span class="sxs-lookup"><span data-stu-id="e9189-188">Next</span></span>](adding-controller.md)  
