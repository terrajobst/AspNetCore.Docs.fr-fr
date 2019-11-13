---
title: ASP.NET Core de débogage Blazor
author: guardrex
description: Découvrez comment déboguer des applications Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/debug
ms.openlocfilehash: 3096ad9b3a6904804f239d61f374adcd4d851978
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963149"
---
# <a name="debug-aspnet-core-opno-locblazor"></a><span data-ttu-id="4223f-103">ASP.NET Core de débogage Blazor</span><span class="sxs-lookup"><span data-stu-id="4223f-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="4223f-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="4223f-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="4223f-105">Il existe une prise en charge *précoce* pour le débogage Blazor les applications webassembly s’exécutant sur webassembly dans Chrome.</span><span class="sxs-lookup"><span data-stu-id="4223f-105">*Early* support exists for debugging Blazor WebAssembly apps running on WebAssembly in Chrome.</span></span>

<span data-ttu-id="4223f-106">Les fonctionnalités du débogueur sont limitées.</span><span class="sxs-lookup"><span data-stu-id="4223f-106">Debugger capabilities are limited.</span></span> <span data-ttu-id="4223f-107">Les scénarios disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="4223f-107">Available scenarios include:</span></span>

* <span data-ttu-id="4223f-108">Définir et supprimer des points d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="4223f-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="4223f-109">Une seule étape (`F10`) à l’aide du code ou de la reprise (`F8`) de l’exécution du code.</span><span class="sxs-lookup"><span data-stu-id="4223f-109">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="4223f-110">Dans l' *affichage des variables locales,* observez les valeurs des variables locales de type `int`, `string` et `bool`.</span><span class="sxs-lookup"><span data-stu-id="4223f-110">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="4223f-111">Consultez la pile des appels, y compris les chaînes d’appel qui passent de JavaScript à .NET et de .NET à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4223f-111">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="4223f-112">Vous *ne pouvez pas*:</span><span class="sxs-lookup"><span data-stu-id="4223f-112">You *can't*:</span></span>

* <span data-ttu-id="4223f-113">Observez les valeurs des variables locales qui ne sont pas un `int`, `string` ou `bool`.</span><span class="sxs-lookup"><span data-stu-id="4223f-113">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="4223f-114">Observez les valeurs de tous les champs ou propriétés de classe.</span><span class="sxs-lookup"><span data-stu-id="4223f-114">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="4223f-115">Pointez sur les variables pour afficher leurs valeurs.</span><span class="sxs-lookup"><span data-stu-id="4223f-115">Hover over variables to see their values.</span></span>
* <span data-ttu-id="4223f-116">Évaluer les expressions dans la console.</span><span class="sxs-lookup"><span data-stu-id="4223f-116">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="4223f-117">Effectuer un pas à pas détaillé dans les appels asynchrones.</span><span class="sxs-lookup"><span data-stu-id="4223f-117">Step across async calls.</span></span>
* <span data-ttu-id="4223f-118">Effectuez la plupart des autres scénarios de débogage ordinaires.</span><span class="sxs-lookup"><span data-stu-id="4223f-118">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="4223f-119">Le développement d’autres scénarios de débogage est l’un des objectifs de l’équipe d’ingénierie.</span><span class="sxs-lookup"><span data-stu-id="4223f-119">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4223f-120">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="4223f-120">Prerequisites</span></span>

<span data-ttu-id="4223f-121">Le débogage requiert l’un des navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="4223f-121">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="4223f-122">Google Chrome (version 70 ou ultérieure)</span><span class="sxs-lookup"><span data-stu-id="4223f-122">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="4223f-123">Version préliminaire de Microsoft Edge ([canal de développement Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="4223f-123">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="4223f-124">Procédure</span><span class="sxs-lookup"><span data-stu-id="4223f-124">Procedure</span></span>

1. <span data-ttu-id="4223f-125">Exécutez une application Blazor webassembly dans la configuration `Debug`.</span><span class="sxs-lookup"><span data-stu-id="4223f-125">Run a Blazor WebAssembly app in `Debug` configuration.</span></span> <span data-ttu-id="4223f-126">Transmettez l’option `--configuration Debug` à la commande [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="4223f-126">Pass the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="4223f-127">Accédez à l’application dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="4223f-127">Access the app in the browser.</span></span>
1. <span data-ttu-id="4223f-128">Placez le focus clavier sur l’application, et non sur le panneau Outils de développement.</span><span class="sxs-lookup"><span data-stu-id="4223f-128">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="4223f-129">Le panneau Outils de développement peut être fermé lorsque le débogage est initié.</span><span class="sxs-lookup"><span data-stu-id="4223f-129">The developer tools panel can be closed when debugging is initiated.</span></span>
1. <span data-ttu-id="4223f-130">Sélectionnez le raccourci clavier spécifique à Blazorsuivant :</span><span class="sxs-lookup"><span data-stu-id="4223f-130">Select the following Blazor-specific keyboard shortcut:</span></span>
   * <span data-ttu-id="4223f-131">`Shift+Alt+D` sur Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="4223f-131">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="4223f-132">`Shift+Cmd+D` sur macOS</span><span class="sxs-lookup"><span data-stu-id="4223f-132">`Shift+Cmd+D` on macOS</span></span>
1. <span data-ttu-id="4223f-133">Suivez les étapes indiquées à l’écran pour redémarrer le navigateur en activant le débogage à distance.</span><span class="sxs-lookup"><span data-stu-id="4223f-133">Follow the steps listed on the screen to restart the browser with remote debugging enabled.</span></span>
1. <span data-ttu-id="4223f-134">Sélectionnez une nouvelle fois le raccourci clavier suivant Blazorpour démarrer la session de débogage :</span><span class="sxs-lookup"><span data-stu-id="4223f-134">Select the following Blazor-specific keyboard shortcut once again to start the debug session:</span></span>
   * <span data-ttu-id="4223f-135">`Shift+Alt+D` sur Windows/Linux</span><span class="sxs-lookup"><span data-stu-id="4223f-135">`Shift+Alt+D` on Windows/Linux</span></span>
   * <span data-ttu-id="4223f-136">`Shift+Cmd+D` sur macOS</span><span class="sxs-lookup"><span data-stu-id="4223f-136">`Shift+Cmd+D` on macOS</span></span>

## <a name="enable-remote-debugging"></a><span data-ttu-id="4223f-137">Activer le débogage distant</span><span class="sxs-lookup"><span data-stu-id="4223f-137">Enable remote debugging</span></span>

<span data-ttu-id="4223f-138">Si le débogage distant est désactivé, une page d’erreur **Impossible de trouver un onglet de navigateur pouvant être débogué** est générée par chrome.</span><span class="sxs-lookup"><span data-stu-id="4223f-138">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="4223f-139">La page d’erreur contient des instructions pour l’exécution de chrome avec le port de débogage ouvert, afin que le Blazor le proxy de débogage puissent se connecter à l’application.</span><span class="sxs-lookup"><span data-stu-id="4223f-139">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="4223f-140">*Fermez toutes les instances chrome* et redémarrez Chrome comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="4223f-140">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="4223f-141">Déboguer l’application</span><span class="sxs-lookup"><span data-stu-id="4223f-141">Debug the app</span></span>

<span data-ttu-id="4223f-142">Une fois que Chrome s’exécute avec le débogage distant activé, le raccourci clavier de débogage ouvre un nouvel onglet du débogueur. Après un moment, l’onglet **sources** affiche une liste des assemblys .net dans l’application.</span><span class="sxs-lookup"><span data-stu-id="4223f-142">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="4223f-143">Développez chaque assembly et recherchez les fichiers sources *. cs*/ *. Razor* disponibles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="4223f-143">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="4223f-144">Définissez des points d’arrêt, revenez à l’onglet de l’application, et les points d’arrêt sont atteints lorsque le code s’exécute.</span><span class="sxs-lookup"><span data-stu-id="4223f-144">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="4223f-145">Après l’accès à un point d’arrêt, vous devez effectuer une seule étape (`F10`) par le biais de l’exécution du code ou de la reprise (`F8`) normalement.</span><span class="sxs-lookup"><span data-stu-id="4223f-145">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="4223f-146"> fournit un proxy de débogage qui implémente le [protocole chrome devtools](https://chromedevtools.github.io/devtools-protocol/) et qui augmente le protocole avec. Informations spécifiques à .net.</span><span class="sxs-lookup"><span data-stu-id="4223f-146"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="4223f-147">Quand l’utilisateur clique sur le raccourci de débogage, Blazor pointe le chrome DevTools au niveau du proxy.</span><span class="sxs-lookup"><span data-stu-id="4223f-147">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="4223f-148">Le proxy se connecte à la fenêtre du navigateur que vous cherchez à déboguer (par conséquent, il est nécessaire d’activer le débogage distant).</span><span class="sxs-lookup"><span data-stu-id="4223f-148">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="4223f-149">Mappages des sources du navigateur</span><span class="sxs-lookup"><span data-stu-id="4223f-149">Browser source maps</span></span>

<span data-ttu-id="4223f-150">Les mappages de source de navigateur permettent au navigateur de mapper les fichiers compilés à leurs fichiers sources d’origine et sont couramment utilisés pour le débogage côté client.</span><span class="sxs-lookup"><span data-stu-id="4223f-150">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="4223f-151">Toutefois, Blazor n’est pas C# actuellement mappé directement à JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="4223f-151">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="4223f-152">Au lieu de cela, Blazor effectue une interprétation IL dans le navigateur, de sorte que les mappages de source ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="4223f-152">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshooting-tip"></a><span data-ttu-id="4223f-153">Conseil de dépannage</span><span class="sxs-lookup"><span data-stu-id="4223f-153">Troubleshooting tip</span></span>

<span data-ttu-id="4223f-154">Si vous rencontrez des erreurs, le Conseil suivant peut vous aider :</span><span class="sxs-lookup"><span data-stu-id="4223f-154">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="4223f-155">Dans l’onglet **débogueur** , ouvrez les outils de développement de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="4223f-155">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="4223f-156">Dans la console, exécutez `localStorage.clear()` pour supprimer les points d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="4223f-156">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
