---
title: Bien démarrer avec des pages Razor dans ASP.NET Core
author: rick-anderson
description: Découvrez les concepts de base de génération d’une application web de pages Razor ASP.NET Core. Les pages Razor sont recommandées pour les charges de travail web dans ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: caf4376c0a02931eeec85e5067a082b37ef9da68
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="133ae-104">Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="133ae-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="133ae-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="133ae-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="133ae-106">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="133ae-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="133ae-107">L’utilisation de pages Razor est la méthode recommandée pour générer l’interface utilisateur d’applications web dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="133ae-107">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="133ae-108">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="133ae-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="133ae-109">Windows : ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="133ae-109">Windows: This tutorial</span></span>
* <span data-ttu-id="133ae-110">MacOS : [Bien démarrer avec les pages Razor avec Visual Studio pour Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="133ae-110">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="133ae-111">macOS, Linux et Windows : [Bien démarrer avec les pages Razor ASP.NET Core dans Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="133ae-111">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="133ae-112">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="133ae-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="133ae-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="133ae-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="133ae-114">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="133ae-114">Create a Razor web app</span></span>

* <span data-ttu-id="133ae-115">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="133ae-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="133ae-116">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="133ae-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="133ae-117">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="133ae-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="133ae-118">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.</span><span class="sxs-lookup"><span data-stu-id="133ae-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="133ae-119">![Nouvelle application web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="133ae-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="133ae-120">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="133ae-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="133ae-121">Le modèle Visual Studio crée un projet de démarrage :</span><span class="sxs-lookup"><span data-stu-id="133ae-121">The Visual Studio template creates a starter project:</span></span>

![Explorateur de solutions](razor-pages-start/_static/se.png)

<span data-ttu-id="133ae-123">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur</span><span class="sxs-lookup"><span data-stu-id="133ae-123">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home.png)

* <span data-ttu-id="133ae-125">Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="133ae-125">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="133ae-126">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="133ae-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="133ae-127">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="133ae-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="133ae-128">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="133ae-128">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="133ae-129">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="133ae-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="133ae-130">Dans l’image précédente, le numéro de port est 5 000.</span><span class="sxs-lookup"><span data-stu-id="133ae-130">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="133ae-131">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="133ae-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="133ae-132">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="133ae-132">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="133ae-133">De nombreux développeurs préfèrent utiliser le mode sans débogage pour lancer rapidement l’application et afficher les changements.</span><span class="sxs-lookup"><span data-stu-id="133ae-133">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [<span data-ttu-id="133ae-134">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="133ae-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
