---
title: "Bien démarrer avec ASP.NET Core MVC et Visual Studio pour Mac"
author: rick-anderson
description: "Bien démarrer avec ASP.NET Core MVC et Visual Studio"
manager: wpickett
ms.author: riande
ms.date: 8/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 05a2323851c58c95667066a74c11f1d015405e6f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="b717d-103">Bien démarrer avec ASP.NET Core MVC et Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b717d-103">Getting started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="b717d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b717d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b717d-105">Ce didacticiel présente les principes de base de la génération d’une application web ASP.NET Core MVC à l’aide de [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="b717d-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="b717d-106">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="b717d-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="b717d-107">macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="b717d-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="b717d-108">Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="b717d-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="b717d-109">Linux, macOS et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="b717d-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b717d-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="b717d-110">Prerequisites</span></span>

<span data-ttu-id="b717d-111">Ce didacticiel nécessite le [SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="b717d-111">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

<span data-ttu-id="b717d-112">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b717d-112">Install the following:</span></span>

- <span data-ttu-id="b717d-113">[SDK .NET Core 2.0.0 ](https://www.microsoft.com/net/core) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="b717d-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="b717d-114">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b717d-114">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a><span data-ttu-id="b717d-115">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="b717d-115">Create a web app</span></span>

<span data-ttu-id="b717d-116">Dans Visual Studio, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="b717d-116">From Visual Studio, select **File > New Solution**.</span></span>

![macOS - Nouvelle solution](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="b717d-118">Sélectionnez **Application .NET Core > ASP.NET Core > Application web > Suivant**.</span><span class="sxs-lookup"><span data-stu-id="b717d-118">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![macOS - Boîte de dialogue Nouveau projet](start-mvc/1.png)

<span data-ttu-id="b717d-120">Nommez le projet **MvcMovie**, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="b717d-120">Name the project **MvcMovie**, and then select **Create**.</span></span>

![macOS - Boîte de dialogue Nouveau projet](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="b717d-122">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="b717d-122">Launch the app</span></span>

<span data-ttu-id="b717d-123">Dans Visual Studio, sélectionnez **Exécuter > Exécuter sans débogage** pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="b717d-123">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="b717d-124">Visual Studio démarre [Kestrel](xref:fundamentals/servers/index#kestrel), lance un navigateur et accède à `http://localhost:port`, où *port* est un numéro de port choisi au hasard.</span><span class="sxs-lookup"><span data-stu-id="b717d-124">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Navigateur avec le nouveau projet](start-mvc/b1.png)

* <span data-ttu-id="b717d-126">La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b717d-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b717d-127">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="b717d-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="b717d-128">Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web.</span><span class="sxs-lookup"><span data-stu-id="b717d-128">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b717d-129">Quand vous exécutez l’application, vous voyez un autre numéro de port.</span><span class="sxs-lookup"><span data-stu-id="b717d-129">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="b717d-130">Vous pouvez lancer l’application en mode débogage ou non-débogage à partir du menu **Exécuter**.</span><span class="sxs-lookup"><span data-stu-id="b717d-130">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="b717d-131">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact**.</span><span class="sxs-lookup"><span data-stu-id="b717d-131">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="b717d-132">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="b717d-132">The browser image above doesn't show these links.</span></span> <span data-ttu-id="b717d-133">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="b717d-133">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Navigateur avec le nouveau projet](start-mvc/b2.png)

<span data-ttu-id="b717d-135">Dans la prochaine partie de ce didacticiel, vous allez découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="b717d-135">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b717d-136">Next</span><span class="sxs-lookup"><span data-stu-id="b717d-136">Next</span></span>](adding-controller.md)  
