---
title: Bien démarrer avec les pages Razor ASP.NET Core dans Visual Studio Code
author: rick-anderson
description: Découvrez les concepts de base de la génération d’applications web de pages Razor ASP.NET Core avec Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: b7f6ca377a892fce912dc0ee9d4b7378f40fbf24
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46522925"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="46093-103">Bien démarrer avec les pages Razor ASP.NET Core dans Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="46093-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="46093-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="46093-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="46093-105">Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="46093-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="46093-106">Nous vous recommandons d’effectuer l’étape [Présentation des pages Razor](xref:razor-pages/index) avant de commencer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="46093-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="46093-107">L’utilisation de pages Razor est la méthode recommandée pour générer l’IU d’applications web dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="46093-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46093-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="46093-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="46093-109">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="46093-109">Create a Razor web app</span></span>

<span data-ttu-id="46093-110">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="46093-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="46093-111">Les commandes précédentes utilisent le [CLI .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) pour créer et exécuter un projet de pages Razor.</span><span class="sxs-lookup"><span data-stu-id="46093-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="46093-112">Ouvrez un navigateur sur http://localhost:5000 pour voir l’application.</span><span class="sxs-lookup"><span data-stu-id="46093-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Page d’accueil ou page d’index](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="46093-114">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="46093-114">Open the project</span></span>

<span data-ttu-id="46093-115">Appuyez sur Ctrl+C pour arrêter l’application.</span><span class="sxs-lookup"><span data-stu-id="46093-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="46093-116">Dans Visual Studio Code (VS Code), sélectionnez **Fichier > Ouvrir le dossier**, puis sélectionnez le dossier *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="46093-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="46093-117">Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans ‘RazorPagesMovie’.</span><span class="sxs-lookup"><span data-stu-id="46093-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="46093-118">Faut-il les ajouter ? »</span><span class="sxs-lookup"><span data-stu-id="46093-118">Add them?"</span></span>
- <span data-ttu-id="46093-119">Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="46093-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="46093-120">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="46093-120">Launch the app</span></span>

<span data-ttu-id="46093-121">Appuyez sur Ctrl+F5 pour démarrer l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="46093-121">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="46093-122">Vous pouvez également aller dans le menu **Déboguer**, puis sélectionner **Exécuter sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="46093-122">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="46093-123">Dans le prochain didacticiel, nous allons ajouter un modèle au projet.</span><span class="sxs-lookup"><span data-stu-id="46093-123">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="46093-124">Suivant : Ajout d’un modèle</span><span class="sxs-lookup"><span data-stu-id="46093-124">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
