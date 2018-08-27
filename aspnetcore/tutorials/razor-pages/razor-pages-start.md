---
title: Bien démarrer avec des pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base de génération d’une application web de pages Razor ASP.NET Core. Les pages Razor sont recommandées pour les charges de travail web dans ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: bc18ec3ad3bb7e3afe38030a34b2e748ce9e341b
ms.sourcegitcommit: 74c09caec8992635825b45b7f065f871d33c077a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42634975"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="8eec5-104">Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8eec5-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="8eec5-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8eec5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8eec5-106">Nous vous recommandons de suivre la version ASP.NET Core 2.1 de ce tutoriel.</span><span class="sxs-lookup"><span data-stu-id="8eec5-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="8eec5-107">Elle est **beaucoup** plus facile à suivre et couvre plus de fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="8eec5-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="8eec5-108">Sélectionnez **ASP.NET Core 2.1** dans le sélecteur de version.</span><span class="sxs-lookup"><span data-stu-id="8eec5-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![Sélecteur de version dans la table des matières](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="8eec5-110">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8eec5-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="8eec5-111">L’utilisation de pages Razor est la méthode recommandée pour générer l’interface utilisateur d’applications web dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8eec5-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="8eec5-112">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="8eec5-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="8eec5-113">Windows : ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="8eec5-113">Windows: This tutorial</span></span>
* <span data-ttu-id="8eec5-114">MacOS : [Bien démarrer avec les pages Razor avec Visual Studio pour Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="8eec5-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="8eec5-115">macOS, Linux et Windows : [Bien démarrer avec les pages Razor ASP.NET Core dans Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="8eec5-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="8eec5-116">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8eec5-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="8eec5-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="8eec5-117">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="8eec5-118">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="8eec5-118">Create a Razor web app</span></span>

* <span data-ttu-id="8eec5-119">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="8eec5-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8eec5-120">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8eec5-120">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="8eec5-121">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="8eec5-121">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="8eec5-122">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.</span><span class="sxs-lookup"><span data-stu-id="8eec5-122">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="8eec5-123">![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="8eec5-123">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="8eec5-124">Sélectionnez **ASP.NET Core 2.1** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="8eec5-124">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![Nouvelle application web ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="8eec5-126">Le modèle Visual Studio crée un projet de démarrage :</span><span class="sxs-lookup"><span data-stu-id="8eec5-126">The Visual Studio template creates a starter project:</span></span>

![Explorateur de solutions](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="8eec5-128">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur.</span><span class="sxs-lookup"><span data-stu-id="8eec5-128">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="8eec5-129">Sélectionnez **Accepter** pour accepter le suivi.</span><span class="sxs-lookup"><span data-stu-id="8eec5-129">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="8eec5-130">Cette application n’effectue pas le suivi d’informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="8eec5-130">This app doesn't track personal information.</span></span> <span data-ttu-id="8eec5-131">Le code généré par le modèle inclut des ressources qui aident à satisfaire au [Règlement général sur la protection des données (RGPD)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="8eec5-131">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="8eec5-133">L’illustration suivante montre l’application une fois le suivi accepté :</span><span class="sxs-lookup"><span data-stu-id="8eec5-133">The following image shows the app after accepting tracking:</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="8eec5-135">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="8eec5-135">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="8eec5-136">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8eec5-136">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8eec5-137">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="8eec5-137">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8eec5-138">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="8eec5-138">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="8eec5-139">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="8eec5-139">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8eec5-140">Dans l’image précédente, le numéro de port est 5 000.</span><span class="sxs-lookup"><span data-stu-id="8eec5-140">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="8eec5-141">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="8eec5-141">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8eec5-142">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="8eec5-142">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8eec5-143">De nombreux développeurs préfèrent utiliser le mode sans débogage pour lancer rapidement l’application et afficher les changements.</span><span class="sxs-lookup"><span data-stu-id="8eec5-143">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="8eec5-144">Prérequis</span><span class="sxs-lookup"><span data-stu-id="8eec5-144">Prerequisites</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="8eec5-145">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="8eec5-145">Create a Razor web app</span></span>

* <span data-ttu-id="8eec5-146">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="8eec5-146">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8eec5-147">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8eec5-147">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="8eec5-148">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="8eec5-148">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="8eec5-149">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.</span><span class="sxs-lookup"><span data-stu-id="8eec5-149">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="8eec5-150">![Nouvelle application web ASP.NET Core](../../razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="8eec5-150">![new ASP.NET Core Web Application](../../razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="8eec5-151">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="8eec5-151">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="8eec5-152">Le modèle Visual Studio crée un projet de démarrage :</span><span class="sxs-lookup"><span data-stu-id="8eec5-152">The Visual Studio template creates a starter project:</span></span>

![Explorateur de solutions](razor-pages-start/_static/se.png)

<span data-ttu-id="8eec5-154">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur</span><span class="sxs-lookup"><span data-stu-id="8eec5-154">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home.png)

* <span data-ttu-id="8eec5-156">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="8eec5-156">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="8eec5-157">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="8eec5-157">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8eec5-158">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="8eec5-158">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="8eec5-159">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="8eec5-159">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="8eec5-160">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="8eec5-160">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8eec5-161">Dans l’image précédente, le numéro de port est 5 000.</span><span class="sxs-lookup"><span data-stu-id="8eec5-161">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="8eec5-162">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="8eec5-162">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="8eec5-163">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="8eec5-163">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="8eec5-164">De nombreux développeurs préfèrent utiliser le mode sans débogage pour lancer rapidement l’application et afficher les changements.</span><span class="sxs-lookup"><span data-stu-id="8eec5-164">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="8eec5-165">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="8eec5-165">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
