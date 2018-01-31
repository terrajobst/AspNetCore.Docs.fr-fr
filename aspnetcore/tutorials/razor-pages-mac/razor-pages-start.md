---
title: "Bien démarrer avec des pages Razor dans ASP.NET Core sur Mac"
author: rick-anderson
description: "Bien démarrer avec des pages Razor dans ASP.NET Core à l’aide de Visual Studio pour Mac"
manager: wpickett
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 9e7d1db47e4cc9d753b1629e20345ca1f4403b2f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="aa69a-103">Bien démarrer avec des pages Razor dans ASP.NET Core avec Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="aa69a-103">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="aa69a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa69a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa69a-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa69a-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="aa69a-106">Nous vous recommandons de revoir [Présentation des pages Razor](xref:mvc/razor-pages/index) avant de commencer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="aa69a-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="aa69a-107">L’utilisation de pages Razor est la méthode recommandée pour générer l’IU d’applications web dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa69a-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa69a-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="aa69a-108">Prerequisites</span></span>

<span data-ttu-id="aa69a-109">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="aa69a-109">Install the following:</span></span>

* <span data-ttu-id="aa69a-110">[SDK .NET Core 2.0.0 ](https://www.microsoft.com/net/core) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="aa69a-110">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="aa69a-111">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="aa69a-111">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="aa69a-112">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="aa69a-112">Create a Razor web app</span></span>

<span data-ttu-id="aa69a-113">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="aa69a-113">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="aa69a-114">Les commandes précédentes utilisent le [CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor.</span><span class="sxs-lookup"><span data-stu-id="aa69a-114">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="aa69a-115">Ouvrez un navigateur à l’adresse http://localhost:5000 pour afficher l’application.</span><span class="sxs-lookup"><span data-stu-id="aa69a-115">Open a browser to http://localhost:5000 to view the application.</span></span>

![Page d’accueil ou page d’index](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="aa69a-117">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="aa69a-117">Open the project</span></span>

<span data-ttu-id="aa69a-118">Appuyez sur Ctrl+C pour arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="aa69a-118">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="aa69a-119">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="aa69a-119">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="aa69a-120">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="aa69a-120">Launch the app</span></span>

<span data-ttu-id="aa69a-121">Dans Visual Studio, sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="aa69a-121">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="aa69a-122">Visual Studio démarre [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), lance un navigateur, puis accède à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="aa69a-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="aa69a-123">Dans le prochain didacticiel, nous allons ajouter un modèle au projet.</span><span class="sxs-lookup"><span data-stu-id="aa69a-123">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="aa69a-124">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="aa69a-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
