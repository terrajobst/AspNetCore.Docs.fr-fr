---
title: ASP.NET Core éblouissant de débogage
author: guardrex
description: Découvrez comment déboguer des applications éblouissantes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/debug
ms.openlocfilehash: 3519479d8058f013de23cc9cfa0f5574cd158053
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207213"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="4cc34-103">ASP.NET Core éblouissant de débogage</span><span class="sxs-lookup"><span data-stu-id="4cc34-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="4cc34-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="4cc34-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="4cc34-105">Une prise en charge *précoce* pour le débogage des applications webassembly éblouissantes s’exécutant sur webassembly dans Chrome.</span><span class="sxs-lookup"><span data-stu-id="4cc34-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="4cc34-106">Les fonctionnalités du débogueur sont limitées.</span><span class="sxs-lookup"><span data-stu-id="4cc34-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="4cc34-107">Les scénarios disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="4cc34-107">Available scenarios include:</span></span>

* <span data-ttu-id="4cc34-108">Définir et supprimer des points d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="4cc34-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="4cc34-109">Une étape (`F10`) à l’aide de l’exécution du`F8`code ou de la reprise ().</span><span class="sxs-lookup"><span data-stu-id="4cc34-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="4cc34-110">Dans l' *affichage des variables locales,* observez les valeurs des variables locales de `int`type `string`, et `bool`.</span><span class="sxs-lookup"><span data-stu-id="4cc34-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="4cc34-111">Consultez la pile des appels, y compris les chaînes d’appel qui passent de JavaScript à .NET et de .NET à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4cc34-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="4cc34-112">Vous *ne pouvez pas*:</span><span class="sxs-lookup"><span data-stu-id="4cc34-112">You *can't*:</span></span>

* <span data-ttu-id="4cc34-113">Observez les valeurs des variables locales qui ne sont `int`pas `string`un, `bool`ou.</span><span class="sxs-lookup"><span data-stu-id="4cc34-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="4cc34-114">Observez les valeurs de tous les champs ou propriétés de classe.</span><span class="sxs-lookup"><span data-stu-id="4cc34-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="4cc34-115">Pointez sur les variables pour afficher leurs valeurs.</span><span class="sxs-lookup"><span data-stu-id="4cc34-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="4cc34-116">Évaluer les expressions dans la console.</span><span class="sxs-lookup"><span data-stu-id="4cc34-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="4cc34-117">Effectuer un pas à pas détaillé dans les appels asynchrones.</span><span class="sxs-lookup"><span data-stu-id="4cc34-117">Step across async calls.</span></span>
* <span data-ttu-id="4cc34-118">Effectuez la plupart des autres scénarios de débogage ordinaires.</span><span class="sxs-lookup"><span data-stu-id="4cc34-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="4cc34-119">Le développement d’autres scénarios de débogage est l’un des objectifs de l’équipe d’ingénierie.</span><span class="sxs-lookup"><span data-stu-id="4cc34-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4cc34-120">Prérequis</span><span class="sxs-lookup"><span data-stu-id="4cc34-120">Prerequisites</span></span>

<span data-ttu-id="4cc34-121">Le débogage requiert l’un des navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="4cc34-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="4cc34-122">Google Chrome (version 70 ou ultérieure)</span><span class="sxs-lookup"><span data-stu-id="4cc34-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="4cc34-123">Version préliminaire de Microsoft Edge ([canal de développement Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="4cc34-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="4cc34-124">Procédure</span><span class="sxs-lookup"><span data-stu-id="4cc34-124">Procedure</span></span>

1. <span data-ttu-id="4cc34-125">Exécutez une application de webassembly éblouissante dans `Debug` la configuration.</span><span class="sxs-lookup"><span data-stu-id="4cc34-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="4cc34-126">Transmettez `--configuration Debug` l’option à la commande [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="4cc34-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="4cc34-127">Accédez à l’application dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="4cc34-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="4cc34-128">Placez le focus clavier sur l’application, et non sur le panneau Outils de développement.</span><span class="sxs-lookup"><span data-stu-id="4cc34-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="4cc34-129">Le panneau Outils de développement peut être fermé lorsque le débogage est initié.</span><span class="sxs-lookup"><span data-stu-id="4cc34-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="4cc34-130">Sélectionnez le raccourci clavier de éblouissant spécifique suivant :</span><span class="sxs-lookup"><span data-stu-id="4cc34-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="4cc34-131">`Shift+Alt+D`sur Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="4cc34-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="4cc34-132">`Shift+Cmd+D`sur macOS</span><span class="sxs-lookup"><span data-stu-id="4cc34-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="4cc34-133">Suivez les étapes indiquées à l’écran pour redémarrer le navigateur en activant le débogage à distance.</span><span class="sxs-lookup"><span data-stu-id="4cc34-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="4cc34-134">Sélectionnez une nouvelle fois le raccourci clavier éblouissant suivant pour démarrer la session de débogage :</span><span class="sxs-lookup"><span data-stu-id="4cc34-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="4cc34-135">`Shift+Alt+D`sur Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="4cc34-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="4cc34-136">`Shift+Cmd+D`sur macOS</span><span class="sxs-lookup"><span data-stu-id="4cc34-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="4cc34-137">Activer le débogage distant</span><span class="sxs-lookup"><span data-stu-id="4cc34-137">Enable remote debugging</span></span>

<span data-ttu-id="4cc34-138">Si le débogage distant est désactivé, une page d’erreur **Impossible de trouver un onglet de navigateur pouvant être débogué** est générée par chrome.</span><span class="sxs-lookup"><span data-stu-id="4cc34-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="4cc34-139">La page d’erreur contient des instructions pour l’exécution de chrome avec le port de débogage ouvert, afin que le proxy de débogage éblouissant puisse se connecter à l’application.</span><span class="sxs-lookup"><span data-stu-id="4cc34-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="4cc34-140">*Fermez toutes les instances chrome* et redémarrez Chrome comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="4cc34-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="4cc34-141">Déboguer l’application</span><span class="sxs-lookup"><span data-stu-id="4cc34-141">Debug the app</span></span>

<span data-ttu-id="4cc34-142">Une fois que Chrome s’exécute avec le débogage distant activé, le raccourci clavier de débogage ouvre un nouvel onglet du débogueur. Après un moment, l’onglet **sources** affiche une liste des assemblys .net dans l’application.</span><span class="sxs-lookup"><span data-stu-id="4cc34-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="4cc34-143">Développez chaque assembly et recherchez les fichiers sources *. cs*/ *. Razor* disponibles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="4cc34-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="4cc34-144">Définissez des points d’arrêt, revenez à l’onglet de l’application, et les points d’arrêt sont atteints lorsque le code s’exécute.</span><span class="sxs-lookup"><span data-stu-id="4cc34-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="4cc34-145">Une fois qu’un point d’arrêt est atteint,`F10`vous devez effectuer une seule étape (`F8`) à l’aide de l’exécution du code ou de la reprise () normalement.</span><span class="sxs-lookup"><span data-stu-id="4cc34-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

<span data-ttu-id="4cc34-146">Éblouissant fournit un proxy de débogage qui implémente le [protocole chrome devtools](https://chromedevtools.github.io/devtools-protocol/) et augmente le protocole avec. Informations spécifiques à .net.</span><span class="sxs-lookup"><span data-stu-id="4cc34-146">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="4cc34-147">Quand vous appuyez sur le raccourci clavier, éblouissant pointe le DevTools chrome au niveau du proxy.</span><span class="sxs-lookup"><span data-stu-id="4cc34-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="4cc34-148">Le proxy se connecte à la fenêtre du navigateur que vous cherchez à déboguer (par conséquent, il est nécessaire d’activer le débogage distant).</span><span class="sxs-lookup"><span data-stu-id="4cc34-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="4cc34-149">Mappages des sources du navigateur</span><span class="sxs-lookup"><span data-stu-id="4cc34-149">Browser source maps</span></span>

<span data-ttu-id="4cc34-150">Les mappages de source de navigateur permettent au navigateur de mapper les fichiers compilés à leurs fichiers sources d’origine et sont couramment utilisés pour le débogage côté client.</span><span class="sxs-lookup"><span data-stu-id="4cc34-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="4cc34-151">Toutefois, éblouissant n’est actuellement pas C# mappé directement à JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="4cc34-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="4cc34-152">À la place, éblouissant effectue une interprétation IL dans le navigateur, de sorte que les mappages de source ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="4cc34-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="4cc34-153">Conseil de dépannage</span><span class="sxs-lookup"><span data-stu-id="4cc34-153">Troubleshooting tip</span></span>

<span data-ttu-id="4cc34-154">Si vous rencontrez des erreurs, le Conseil suivant peut vous aider :</span><span class="sxs-lookup"><span data-stu-id="4cc34-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="4cc34-155">Dans l’onglet **débogueur** , ouvrez les outils de développement de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="4cc34-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="4cc34-156">Dans la console, exécutez `localStorage.clear()` pour supprimer tous les points d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="4cc34-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
