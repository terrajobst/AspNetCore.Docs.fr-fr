---
title: "Bien démarrer avec des pages Razor dans ASP.NET Core"
author: rick-anderson
description: "Bien démarrer avec des pages Razor dans ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 69a5bc439130ffacf2d267c79b1a6b0347171e49
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="0a3b9-103">Bien démarrer avec des pages Razor dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a3b9-103">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0a3b9-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a3b9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a3b9-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="0a3b9-106">L’utilisation de pages Razor est la méthode recommandée pour générer l’interface utilisateur d’applications web dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-106">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="0a3b9-107">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="0a3b9-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="0a3b9-108">Windows : ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="0a3b9-108">Windows: This tutorial</span></span>
* <span data-ttu-id="0a3b9-109">Mac OS : [Bien démarrer avec les pages Razor avec Visual Studio pour Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="0a3b9-109">MacOS: [Getting started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="0a3b9-110">Mac OS, Linux et Windows : [Bien démarrer avec les pages Razor dans ASP.NET Core avec Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="0a3b9-110">macOS, Linux, and Windows: [Getting started with Razor Pages in ASP.NET Core with Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="0a3b9-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a3b9-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a3b9-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="0a3b9-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="0a3b9-113">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="0a3b9-113">Create a Razor web app</span></span>

* <span data-ttu-id="0a3b9-114">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="0a3b9-115">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-115">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="0a3b9-116">Nommez le projet **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-116">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="0a3b9-117">Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-117">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="0a3b9-118">![Nouvelle application web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="0a3b9-118">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="0a3b9-119">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-119">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="0a3b9-120">Le modèle Visual Studio crée un projet de démarrage :</span><span class="sxs-lookup"><span data-stu-id="0a3b9-120">The Visual Studio template creates a starter project:</span></span>

![Explorateur de solutions](razor-pages-start/_static/se.png)

<span data-ttu-id="0a3b9-122">Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur</span><span class="sxs-lookup"><span data-stu-id="0a3b9-122">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Page d’accueil ou page d’index](razor-pages-start/_static/home.png)

* <span data-ttu-id="0a3b9-124">Visual Studio démarre [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-124">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="0a3b9-125">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-125">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0a3b9-126">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-126">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="0a3b9-127">Localhost traite uniquement les requêtes web de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-127">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="0a3b9-128">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0a3b9-129">Dans l’image précédente, le numéro de port est 5 000.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-129">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="0a3b9-130">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-130">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="0a3b9-131">Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-131">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="0a3b9-132">De nombreux développeurs préfèrent utiliser le mode sans débogage pour lancer rapidement l’application et afficher les changements.</span><span class="sxs-lookup"><span data-stu-id="0a3b9-132">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="0a3b9-133">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="0a3b9-133">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[<span data-ttu-id="0a3b9-134">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="0a3b9-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
