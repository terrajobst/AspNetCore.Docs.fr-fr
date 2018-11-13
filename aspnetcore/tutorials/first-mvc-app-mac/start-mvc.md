---
title: Bien démarrer avec ASP.NET Core MVC et Visual Studio pour Mac
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC et Visual Studio.
ms.author: riande
ms.date: 8/23/2017
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 059ac1f7fa94d97adc958be3c0b936cdfa7f6d3e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225471"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="235a9-103">Bien démarrer avec ASP.NET Core MVC et Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="235a9-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="235a9-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="235a9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="235a9-105">Ce didacticiel présente les principes de base de la génération d’une application web ASP.NET Core MVC à l’aide de [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="235a9-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="235a9-106">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="235a9-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="235a9-107">macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="235a9-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="235a9-108">Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="235a9-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="235a9-109">Linux, macOS et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="235a9-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="235a9-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="235a9-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="235a9-111">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="235a9-111">Create a web app</span></span>

<span data-ttu-id="235a9-112">Dans Visual Studio, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="235a9-112">From Visual Studio, select **File > New Solution**.</span></span>

![macOS - Nouvelle solution](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="235a9-114">Sélectionnez **Application .NET Core > ASP.NET Core > Application web ASP.NET Core (MVC) > Suivant**.</span><span class="sxs-lookup"><span data-stu-id="235a9-114">Select **.NET Core App >  ASP.NET Core > ASP.NET Core Web App (MVC) > Next**.</span></span>

![macOS - Boîte de dialogue Nouveau projet](start-mvc/1.png)

<span data-ttu-id="235a9-116">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="235a9-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS - Boîte de dialogue Nouveau projet](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="235a9-118">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="235a9-118">Launch the app</span></span>

<span data-ttu-id="235a9-119">Dans Visual Studio, sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="235a9-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="235a9-120">Visual Studio démarre [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi au hasard.</span><span class="sxs-lookup"><span data-stu-id="235a9-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Navigateur avec le nouveau projet](start-mvc/b1.png)

* <span data-ttu-id="235a9-122">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="235a9-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="235a9-123">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="235a9-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="235a9-124">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="235a9-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="235a9-125">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="235a9-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="235a9-126">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="235a9-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="235a9-127">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact**.</span><span class="sxs-lookup"><span data-stu-id="235a9-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="235a9-128">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="235a9-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="235a9-129">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="235a9-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigateur avec le nouveau projet](start-mvc/b2.png)

<span data-ttu-id="235a9-131">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="235a9-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="235a9-132">Next</span><span class="sxs-lookup"><span data-stu-id="235a9-132">Next</span></span>](adding-controller.md)  
