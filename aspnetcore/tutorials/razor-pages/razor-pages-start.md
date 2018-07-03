---
title: Bien démarrer avec des pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base de génération d’une application web de pages Razor ASP.NET Core. Les pages Razor sont recommandées pour les charges de travail web dans ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 7148f2d944bd1978b1a83278dfed9051f192e4dd
ms.sourcegitcommit: 08f1a9baa97060da5168840b332c9c0805b5f901
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37144935"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="942c2-104">Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="942c2-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="942c2-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="942c2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="942c2-106">Nous vous recommandons de suivre la version ASP.NET Core 2.1 de ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="942c2-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="942c2-107">Elle est **beaucoup** plus facile à suivre et couvre plus de fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="942c2-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="942c2-108">Sélectionnez **ASP.NET Core 2.1** dans le sélecteur de version.</span><span class="sxs-lookup"><span data-stu-id="942c2-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Sélecteur de version dans la table des matières](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="942c2-110">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="942c2-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="942c2-111">L’utilisation de pages Razor est la méthode recommandée pour générer l’interface utilisateur d’applications web dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="942c2-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="942c2-112">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="942c2-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="942c2-113">Windows : ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="942c2-113">Windows: This tutorial</span></span>
* <span data-ttu-id="942c2-114">MacOS : [Bien démarrer avec les pages Razor avec Visual Studio pour Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="942c2-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="942c2-115">macOS, Linux et Windows : [Bien démarrer avec les pages Razor ASP.NET Core dans Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="942c2-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="942c2-116">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="942c2-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="942c2-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="942c2-117">Prerequisites</span></span>

<span data-ttu-id="942c2-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="942c2-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="942c2-119">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="942c2-119">Create a Razor web app</span></span>

* <span data-ttu-id="942c2-120">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="942c2-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="942c2-121">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="942c2-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="942c2-122">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="942c2-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="942c2-123">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.</span><span class="sxs-lookup"><span data-stu-id="942c2-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="942c2-124">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="942c2-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="942c2-125">Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="942c2-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="942c2-127">Le modèle Visual Studio crée un projet de démarrage :</span><span class="sxs-lookup"><span data-stu-id="942c2-127">The Visual Studio template creates a starter project:</span></span>

![Explorateur de solutions](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="942c2-129">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur.</span><span class="sxs-lookup"><span data-stu-id="942c2-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="942c2-130">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="942c2-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="942c2-131">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="942c2-131">This app doesn't track personal information.</span></span> <span data-ttu-id="942c2-132">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="942c2-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="942c2-134">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="942c2-134">The following image shows the app after accepting tracking:</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="942c2-136">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="942c2-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="942c2-137">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="942c2-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="942c2-138">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="942c2-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="942c2-139">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="942c2-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="942c2-140">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="942c2-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="942c2-141">Dans l’image précédente, le numéro de port est 5 000.</span><span class="sxs-lookup"><span data-stu-id="942c2-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="942c2-142">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="942c2-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="942c2-143">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="942c2-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="942c2-144">De nombreux développeurs préfèrent utiliser le mode sans débogage pour lancer rapidement l’application et afficher les changements.</span><span class="sxs-lookup"><span data-stu-id="942c2-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="942c2-145">Prérequis</span><span class="sxs-lookup"><span data-stu-id="942c2-145">Prerequisites</span></span>

<span data-ttu-id="942c2-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="942c2-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="942c2-147">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="942c2-147">Create a Razor web app</span></span>

* <span data-ttu-id="942c2-148">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="942c2-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="942c2-149">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="942c2-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="942c2-150">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="942c2-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="942c2-151">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.</span><span class="sxs-lookup"><span data-stu-id="942c2-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="942c2-152">![Nouvelle application web ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="942c2-152">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="942c2-153">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="942c2-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="942c2-154">Le modèle Visual Studio crée un projet de démarrage :</span><span class="sxs-lookup"><span data-stu-id="942c2-154">The Visual Studio template creates a starter project:</span></span>

![Explorateur de solutions](razor-pages-start/_static/se.png)

<span data-ttu-id="942c2-156">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur</span><span class="sxs-lookup"><span data-stu-id="942c2-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home.png)

* <span data-ttu-id="942c2-158">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="942c2-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="942c2-159">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="942c2-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="942c2-160">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="942c2-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="942c2-161">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="942c2-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="942c2-162">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="942c2-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="942c2-163">Dans l’image précédente, le numéro de port est 5 000.</span><span class="sxs-lookup"><span data-stu-id="942c2-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="942c2-164">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="942c2-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="942c2-165">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="942c2-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="942c2-166">De nombreux développeurs préfèrent utiliser le mode sans débogage pour lancer rapidement l’application et afficher les changements.</span><span class="sxs-lookup"><span data-stu-id="942c2-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="942c2-167">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="942c2-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
