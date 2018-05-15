---
title: Présentation d’ASP.NET Core MVC sur macOS, Linux ou Windows
author: rick-anderson
description: Découvrez comment bien démarrer avec ASP.NET Core MVC et Visual Studio Code sur macOS, Linux et Windows
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 50fbd54c6b0cc1146271afda7e45a0dab590dd7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="48842-103">Présentation d’ASP.NET Core MVC sur macOS, Linux ou Windows</span><span class="sxs-lookup"><span data-stu-id="48842-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="48842-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48842-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="48842-105">Ce didacticiel présente les principes de base de la génération d’une application web ASP.NET Core MVC à l’aide de [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="48842-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="48842-106">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="48842-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="48842-107">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="48842-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="48842-108">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="48842-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="48842-109">macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="48842-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="48842-110">Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="48842-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="48842-111">macOS, Linux et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="48842-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="48842-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="48842-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="48842-113">Créer une application web avec dotnet</span><span class="sxs-lookup"><span data-stu-id="48842-113">Create a web app with dotnet</span></span>

<span data-ttu-id="48842-114">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="48842-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="48842-115">Ouvrez le dossier *MvcMovie* dans Visual Studio Code (VS Code), puis sélectionnez le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="48842-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="48842-116">Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="48842-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="48842-117">Faut-il les ajouter ? »</span><span class="sxs-lookup"><span data-stu-id="48842-117">Add them?"</span></span>
- <span data-ttu-id="48842-118">Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="48842-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'MvcMovie'.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="48842-122">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="48842-122">Press **Debug** (F5) to build and run the program.</span></span>

![application en cours d’exécution](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="48842-124">VS Code démarre le serveur web [Kestrel](xref:fundamentals/servers/kestrel) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="48842-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="48842-125">Notez que la barre d’adresse affiche `localhost:5000`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="48842-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="48842-126">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="48842-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="48842-127">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="48842-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="48842-128">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="48842-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="48842-129">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="48842-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icône de navigation en haut à droite](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="48842-131">Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="48842-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="48842-132">Aide de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="48842-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="48842-133">Prise en main</span><span class="sxs-lookup"><span data-stu-id="48842-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="48842-134">Débogage</span><span class="sxs-lookup"><span data-stu-id="48842-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="48842-135">Terminal intégré</span><span class="sxs-lookup"><span data-stu-id="48842-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="48842-136">Raccourcis clavier</span><span class="sxs-lookup"><span data-stu-id="48842-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="48842-137">Raccourcis clavier macOS</span><span class="sxs-lookup"><span data-stu-id="48842-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="48842-138">Raccourcis clavier Linux</span><span class="sxs-lookup"><span data-stu-id="48842-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="48842-139">Raccourcis clavier Windows</span><span class="sxs-lookup"><span data-stu-id="48842-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="48842-140">Suivant - Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="48842-140">Next - Add a controller</span></span>](adding-controller.md)
