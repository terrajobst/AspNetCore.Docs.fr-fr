---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="58f3a-103">Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58f3a-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="58f3a-104">Les instructions suivantes concernent la dernière version d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="58f3a-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="58f3a-105">Consultez [Bien démarrer avec ASP.NET Core 1.1](xref:getting-started-1.1) pour la version 1.1 de ce document.</span><span class="sxs-lookup"><span data-stu-id="58f3a-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="58f3a-106">Installez le [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="58f3a-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="58f3a-107">Créez un projet .NET Core.</span><span class="sxs-lookup"><span data-stu-id="58f3a-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="58f3a-108">Sur macOS et Linux, ouvrez une fenêtre de terminal.</span><span class="sxs-lookup"><span data-stu-id="58f3a-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="58f3a-109">Sur Windows, ouvrez une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="58f3a-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="58f3a-110">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="58f3a-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="58f3a-111">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="58f3a-111">Run the app.</span></span>

    <span data-ttu-id="58f3a-112">Exécutez les commandes suivantes pour exécuter l’application :</span><span class="sxs-lookup"><span data-stu-id="58f3a-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="58f3a-113">Accédez à [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="58f3a-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="58f3a-114">Ouvrez <em>Pages/About.cshtml</em>, puis modifiez la page de façon à afficher le message « Hello, world!</span><span class="sxs-lookup"><span data-stu-id="58f3a-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="58f3a-115">The time on the server is @DateTime.Now » :</span><span class="sxs-lookup"><span data-stu-id="58f3a-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="58f3a-116">Accédez à [http://localhost:5000/About](http://localhost:5000/About) et vérifiez les changements.</span><span class="sxs-lookup"><span data-stu-id="58f3a-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="58f3a-117">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="58f3a-117">Next steps</span></span>

<span data-ttu-id="58f3a-118">Pour consulter des didacticiels permettant de bien démarrer, consultez [Didacticiels ASP.NET Core](tutorials/index.md).</span><span class="sxs-lookup"><span data-stu-id="58f3a-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="58f3a-119">Pour obtenir une introduction aux concepts et à l’architecture d’ASP.NET Core, consultez [Introduction à ASP.NET Core](index.md) et [Notions de base d’ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="58f3a-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="58f3a-120">Une application ASP.NET Core peut utiliser la bibliothèque de classes de base et le runtime .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="58f3a-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="58f3a-121">Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="58f3a-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
