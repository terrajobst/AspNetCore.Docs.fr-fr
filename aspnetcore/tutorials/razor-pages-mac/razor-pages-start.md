---
title: Bien démarrer avec les pages Razor dans ASP.NET Core sur macOS avec Visual Studio pour Mac
author: rick-anderson
description: Découvrez comment bien démarrer avec les pages Razor dans ASP.NET Core à l’aide de Visual Studio pour Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 0e1c2a9ab436968e2c24aa5f306ec69674fb025b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523270"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="b089c-103">Bien démarrer avec les pages Razor dans ASP.NET Core sur macOS avec Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b089c-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="b089c-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b089c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b089c-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b089c-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="b089c-106">Nous vous recommandons de revoir [Présentation des pages Razor](xref:razor-pages/index) avant de commencer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="b089c-106">We recommend you review [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="b089c-107">L’utilisation de pages Razor est la méthode recommandée pour générer l’IU d’applications web dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b089c-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b089c-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b089c-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="b089c-109">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="b089c-109">Create a Razor web app</span></span>

<span data-ttu-id="b089c-110">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b089c-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="b089c-111">Les commandes précédentes utilisent le [CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor.</span><span class="sxs-lookup"><span data-stu-id="b089c-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="b089c-112">Ouvrez un navigateur sur http://localhost:5000 pour voir l’application.</span><span class="sxs-lookup"><span data-stu-id="b089c-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Page d’accueil ou page d’index](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="b089c-114">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="b089c-114">Open the project</span></span>

<span data-ttu-id="b089c-115">Appuyez sur Ctrl+C pour arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="b089c-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="b089c-116">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b089c-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="b089c-117">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="b089c-117">Launch the app</span></span>

<span data-ttu-id="b089c-118">Dans Visual Studio, sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="b089c-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="b089c-119">Visual Studio démarre [Kestrel](xref:fundamentals/servers/kestrel), lance un navigateur, puis accède à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b089c-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="b089c-120">Dans le prochain didacticiel, nous allons ajouter un modèle au projet.</span><span class="sxs-lookup"><span data-stu-id="b089c-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b089c-121">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="b089c-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
