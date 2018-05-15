---
title: Bien démarrer avec ASP.NET Core 1.1
author: rick-anderson
description: Suivez ce tutoriel rapide pour créer et exécuter une application Hello World simple à l’aide d’ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a><span data-ttu-id="138a7-103">Bien démarrer avec ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="138a7-103">Get Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="138a7-104">Les instructions suivantes concernent ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="138a7-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="138a7-105">Si vous recherchez la dernière version,</span><span class="sxs-lookup"><span data-stu-id="138a7-105">Looking for the latest version?</span></span> <span data-ttu-id="138a7-106">consultez [la version actuelle de ce tutoriel](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="138a7-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="138a7-107">Installez le **programme d’installation du SDK** .NET Core pour le SDK 1.0.4 à partir de la [page de tous les téléchargements .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="138a7-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="138a7-108">Créez un dossier pour un nouveau projet .NET Core.</span><span class="sxs-lookup"><span data-stu-id="138a7-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="138a7-109">Sur macOS et Linux, ouvrez une fenêtre de terminal.</span><span class="sxs-lookup"><span data-stu-id="138a7-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="138a7-110">Sur Windows, ouvrez une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="138a7-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="138a7-111">Si vous avez installé une version ultérieure du SDK sur votre ordinateur, créez un fichier *global.json* pour sélectionner le SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="138a7-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="138a7-112">Créez un projet .NET Core.</span><span class="sxs-lookup"><span data-stu-id="138a7-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="138a7-113">Restaurez les packages.</span><span class="sxs-lookup"><span data-stu-id="138a7-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="138a7-114">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="138a7-114">Run the app.</span></span>

   <span data-ttu-id="138a7-115">La commande [dotnet run](/dotnet/core/tools/dotnet-run) commence par créer l’application, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="138a7-115">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="138a7-116">Accédez à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="138a7-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="138a7-117">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="138a7-117">Next steps</span></span>

<span data-ttu-id="138a7-118">Pour consulter des didacticiels permettant de bien démarrer, consultez [Didacticiels ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="138a7-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="138a7-119">Pour obtenir une introduction aux concepts et à l’architecture d’ASP.NET Core, consultez [Introduction à ASP.NET Core](index.md) et [Notions de base d’ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="138a7-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="138a7-120">Une application ASP.NET Core peut utiliser la bibliothèque de classes de base et le runtime .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="138a7-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="138a7-121">Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="138a7-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
