---
title: "Introduction à ASP.NET Core MVC sur Mac, Linux ou Windows"
author: rick-anderson
description: "Bien démarrer avec ASP.NET Core MVC et Visual Studio Code sur Mac, Linux et Windows"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: dae1b0f7c8700bbd99080752fb4bb37f93441c9a
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="dcd88-103">Bien démarrer avec ASP.NET Core MVC sur Mac, Linux ou Windows</span><span class="sxs-lookup"><span data-stu-id="dcd88-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="dcd88-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dcd88-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dcd88-105">Ce didacticiel présente les principes de base de la génération d’une application web ASP.NET Core MVC à l’aide de [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="dcd88-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="dcd88-106">Il part du principe que vous connaissez déjà VS Code.</span><span class="sxs-lookup"><span data-stu-id="dcd88-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="dcd88-107">Pour plus d’informations, consultez [Bien démarrer avec VS Code](https://code.visualstudio.com/docs) et [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="dcd88-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="dcd88-108">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="dcd88-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="dcd88-109">macOS : [Créer une application ASP.NET Core MVC avec Visual Studio pour Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="dcd88-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="dcd88-110">Windows : [Créer une application ASP.NET Core MVC avec Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="dcd88-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="dcd88-111">macOS, Linux et Windows : [Créer une application ASP.NET Core MVC avec Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="dcd88-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="dcd88-112">Installer VS Code et .NET Core</span><span class="sxs-lookup"><span data-stu-id="dcd88-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="dcd88-113">Ce didacticiel nécessite le [SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="dcd88-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="dcd88-114">Consultez [ce fichier PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) pour la version ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="dcd88-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="dcd88-115">Installez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="dcd88-115">Install the following:</span></span>

* <span data-ttu-id="dcd88-116">[SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="dcd88-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="dcd88-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dcd88-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="dcd88-118">[Extension C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) VS Code</span><span class="sxs-lookup"><span data-stu-id="dcd88-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="dcd88-119">Créer une application web avec dotnet</span><span class="sxs-lookup"><span data-stu-id="dcd88-119">Create a web app with dotnet</span></span>

<span data-ttu-id="dcd88-120">À partir d’un terminal, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dcd88-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="dcd88-121">Ouvrez le dossier *MvcMovie* dans Visual Studio Code (VS Code), puis sélectionnez le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="dcd88-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="dcd88-122">Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="dcd88-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="dcd88-123">Faut-il les ajouter ? »</span><span class="sxs-lookup"><span data-stu-id="dcd88-123">Add them?"</span></span>
- <span data-ttu-id="dcd88-124">Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="dcd88-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'MvcMovie'.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="dcd88-128">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="dcd88-128">Press **Debug** (F5) to build and run the program.</span></span>

![application en cours d’exécution](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="dcd88-130">VS Code démarre le serveur web [Kestrel](xref:fundamentals/servers/kestrel) et exécute votre application.</span><span class="sxs-lookup"><span data-stu-id="dcd88-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="dcd88-131">Notez que la barre d’adresse affiche `localhost:5000`, et non quelque chose comme `example.com`.</span><span class="sxs-lookup"><span data-stu-id="dcd88-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="dcd88-132">En effet, `localhost` est le nom d’hôte standard de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="dcd88-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="dcd88-133">Le modèle par défaut donne des liens **Accueil, À propos** et **Contact** fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="dcd88-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="dcd88-134">L’image de navigateur ci-dessus ne montre pas ces liens.</span><span class="sxs-lookup"><span data-stu-id="dcd88-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="dcd88-135">En fonction de la taille de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour les afficher.</span><span class="sxs-lookup"><span data-stu-id="dcd88-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icône de navigation en haut à droite](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="dcd88-137">Dans la prochaine partie de ce didacticiel, nous allons découvrir MVC et commencer à écrire du code.</span><span class="sxs-lookup"><span data-stu-id="dcd88-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="dcd88-138">Aide de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dcd88-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="dcd88-139">Prise en main</span><span class="sxs-lookup"><span data-stu-id="dcd88-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="dcd88-140">Débogage</span><span class="sxs-lookup"><span data-stu-id="dcd88-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="dcd88-141">Terminal intégré</span><span class="sxs-lookup"><span data-stu-id="dcd88-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="dcd88-142">Raccourcis clavier</span><span class="sxs-lookup"><span data-stu-id="dcd88-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="dcd88-143">Raccourcis clavier Mac</span><span class="sxs-lookup"><span data-stu-id="dcd88-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="dcd88-144">Raccourcis clavier Linux</span><span class="sxs-lookup"><span data-stu-id="dcd88-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="dcd88-145">Raccourcis clavier Windows</span><span class="sxs-lookup"><span data-stu-id="dcd88-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="dcd88-146">Suivant - Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="dcd88-146">Next - Add a controller</span></span>](adding-controller.md)
