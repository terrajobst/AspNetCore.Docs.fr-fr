---
title: ASP.NET Core de débogage Blazor
author: guardrex
description: Découvrez comment déboguer des applications Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661706"
---
# <a name="debug-aspnet-core-blazor"></a><span data-ttu-id="09104-103">ASP.NET Core éblouissant de débogage</span><span class="sxs-lookup"><span data-stu-id="09104-103">Debug ASP.NET Core Blazor</span></span>

[<span data-ttu-id="09104-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="09104-104">Daniel Roth</span></span>](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="09104-105">Il existe une prise en charge *précoce* pour le débogage de l’assembly Web éblouissant à l’aide des outils de développement de navigateur dans les navigateurs basés sur le chrome (chrome/Edge).</span><span class="sxs-lookup"><span data-stu-id="09104-105">*Early* support exists for debugging Blazor WebAssembly using the browser dev tools in Chromium-based browsers (Chrome/Edge).</span></span> <span data-ttu-id="09104-106">Le travail est en cours pour :</span><span class="sxs-lookup"><span data-stu-id="09104-106">Work is in progress to:</span></span>

* <span data-ttu-id="09104-107">Activez complètement le débogage dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09104-107">Fully enable debugging in Visual Studio.</span></span>
* <span data-ttu-id="09104-108">Activez le débogage dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="09104-108">Enable debugging in Visual Studio Code.</span></span>

<span data-ttu-id="09104-109">Les fonctionnalités du débogueur sont limitées.</span><span class="sxs-lookup"><span data-stu-id="09104-109">Debugger capabilities are limited.</span></span> <span data-ttu-id="09104-110">Les scénarios disponibles sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="09104-110">Available scenarios include:</span></span>

* <span data-ttu-id="09104-111">Définir et supprimer des points d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="09104-111">Set and remove breakpoints.</span></span>
* <span data-ttu-id="09104-112">Une seule étape (`F10`) par le biais de l’exécution du code ou de la reprise (`F8`).</span><span class="sxs-lookup"><span data-stu-id="09104-112">Single-step (`F10`) through the code or resume (`F8`) code execution.</span></span>
* <span data-ttu-id="09104-113">Dans l' *affichage des variables locales,* observez les valeurs des variables locales de type `int`, `string`et `bool`.</span><span class="sxs-lookup"><span data-stu-id="09104-113">In the *Locals* display, observe the values of any local variables of type `int`, `string`, and `bool`.</span></span>
* <span data-ttu-id="09104-114">Consultez la pile des appels, y compris les chaînes d’appel qui passent de JavaScript à .NET et de .NET à JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09104-114">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="09104-115">Vous *ne pouvez pas*:</span><span class="sxs-lookup"><span data-stu-id="09104-115">You *can't*:</span></span>

* <span data-ttu-id="09104-116">Observez les valeurs des variables locales qui ne sont pas un `int`, `string`ou `bool`.</span><span class="sxs-lookup"><span data-stu-id="09104-116">Observe the values of any locals that aren't an `int`, `string`, or `bool`.</span></span>
* <span data-ttu-id="09104-117">Observez les valeurs de tous les champs ou propriétés de classe.</span><span class="sxs-lookup"><span data-stu-id="09104-117">Observe the values of any class properties or fields.</span></span>
* <span data-ttu-id="09104-118">Pointez sur les variables pour afficher leurs valeurs.</span><span class="sxs-lookup"><span data-stu-id="09104-118">Hover over variables to see their values.</span></span>
* <span data-ttu-id="09104-119">Évaluer les expressions dans la console.</span><span class="sxs-lookup"><span data-stu-id="09104-119">Evaluate expressions in the console.</span></span>
* <span data-ttu-id="09104-120">Effectuer un pas à pas détaillé dans les appels asynchrones.</span><span class="sxs-lookup"><span data-stu-id="09104-120">Step across async calls.</span></span>
* <span data-ttu-id="09104-121">Effectuez la plupart des autres scénarios de débogage ordinaires.</span><span class="sxs-lookup"><span data-stu-id="09104-121">Perform most other ordinary debugging scenarios.</span></span>

<span data-ttu-id="09104-122">Le développement d’autres scénarios de débogage est l’un des objectifs de l’équipe d’ingénierie.</span><span class="sxs-lookup"><span data-stu-id="09104-122">Development of further debugging scenarios is an on-going focus of the engineering team.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09104-123">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="09104-123">Prerequisites</span></span>

<span data-ttu-id="09104-124">Le débogage requiert l’un des navigateurs suivants :</span><span class="sxs-lookup"><span data-stu-id="09104-124">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="09104-125">Google Chrome (version 70 ou ultérieure)</span><span class="sxs-lookup"><span data-stu-id="09104-125">Google Chrome (version 70 or later)</span></span>
* <span data-ttu-id="09104-126">Version préliminaire de Microsoft Edge ([canal de développement Edge](https://www.microsoftedgeinsider.com))</span><span class="sxs-lookup"><span data-stu-id="09104-126">Microsoft Edge preview ([Edge Dev Channel](https://www.microsoftedgeinsider.com))</span></span>

## <a name="procedure"></a><span data-ttu-id="09104-127">Procédure</span><span class="sxs-lookup"><span data-stu-id="09104-127">Procedure</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="09104-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09104-128">Visual Studio</span></span>](#tab/visual-studio)

> [!WARNING]
> <span data-ttu-id="09104-129">La prise en charge du débogage dans Visual Studio est une étape précoce du développement.</span><span class="sxs-lookup"><span data-stu-id="09104-129">Debugging support in Visual Studio is at an early stage of development.</span></span> <span data-ttu-id="09104-130">Le débogage **F5** n’est pas pris en charge actuellement.</span><span class="sxs-lookup"><span data-stu-id="09104-130">**F5** debugging isn't currently supported.</span></span>

1. <span data-ttu-id="09104-131">Exécutez une application webassembly éblouissante dans `Debug` configuration sans débogage (**Ctrl**+**F5** au lieu de **F5**).</span><span class="sxs-lookup"><span data-stu-id="09104-131">Run a Blazor WebAssembly app in `Debug` configuration without debugging (**Ctrl**+**F5** instead of **F5**).</span></span>
1. <span data-ttu-id="09104-132">Ouvrez les propriétés de débogage de l’application (dernière entrée dans le menu **Déboguer** ) et copiez l’URL de l' **application**http.</span><span class="sxs-lookup"><span data-stu-id="09104-132">Open the Debug properties of the app (last entry in the **Debug** menu) and copy the HTTP **App URL**.</span></span> <span data-ttu-id="09104-133">Accédez à l’adresse HTTP (et non à l’adresse HTTPs) de l’application à l’aide d’un navigateur basé sur le chrome (Edge Beta ou chrome).</span><span class="sxs-lookup"><span data-stu-id="09104-133">Browse to the HTTP address (not the HTTPS address) of the app using a Chromium-based browser (Edge Beta or Chrome).</span></span>
1. <span data-ttu-id="09104-134">Placez le focus clavier sur l’application dans la fenêtre du navigateur, et non dans le panneau Outils de développement.</span><span class="sxs-lookup"><span data-stu-id="09104-134">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span> <span data-ttu-id="09104-135">Il est préférable de garder le panneau Outils de développement fermé pour cette procédure.</span><span class="sxs-lookup"><span data-stu-id="09104-135">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="09104-136">Une fois le débogage démarré, vous pouvez rouvrir le panneau Outils de développement.</span><span class="sxs-lookup"><span data-stu-id="09104-136">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="09104-137">Sélectionnez le raccourci clavier de éblouissant spécifique suivant :</span><span class="sxs-lookup"><span data-stu-id="09104-137">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="09104-138">`Shift+Alt+D` sur Windows</span><span class="sxs-lookup"><span data-stu-id="09104-138">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="09104-139">`Shift+Cmd+D` sur macOS</span><span class="sxs-lookup"><span data-stu-id="09104-139">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="09104-140">Si vous recevez l' **onglet impossible de trouver le navigateur pouvant être débogué**, consultez [activer le débogage distant](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="09104-140">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="09104-141">Après l’activation du débogage distant :</span><span class="sxs-lookup"><span data-stu-id="09104-141">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="09104-142">1 \.</span><span class="sxs-lookup"><span data-stu-id="09104-142">1\.</span></span> <span data-ttu-id="09104-143">Une nouvelle fenêtre de navigateur s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="09104-143">A new browser window opens.</span></span> <span data-ttu-id="09104-144">Fermez la fenêtre précédente.</span><span class="sxs-lookup"><span data-stu-id="09104-144">Close the prior window.</span></span>

   <span data-ttu-id="09104-145">2 \.</span><span class="sxs-lookup"><span data-stu-id="09104-145">2\.</span></span> <span data-ttu-id="09104-146">Placez le focus clavier sur l’application dans la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="09104-146">Place the keyboard focus on the app in the browser window.</span></span>

   <span data-ttu-id="09104-147">3 \.</span><span class="sxs-lookup"><span data-stu-id="09104-147">3\.</span></span> <span data-ttu-id="09104-148">Sélectionnez le raccourci clavier de la nouvelle fenêtre de navigateur : `Shift+Alt+D` sur Windows ou sur `Shift+Cmd+D` sur macOS.</span><span class="sxs-lookup"><span data-stu-id="09104-148">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

   <span data-ttu-id="09104-149">4 \.</span><span class="sxs-lookup"><span data-stu-id="09104-149">4\.</span></span> <span data-ttu-id="09104-150">L’onglet **devtools** s’ouvre dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="09104-150">The **DevTools** tab opens in the browser.</span></span> <span data-ttu-id="09104-151">**Resélectionnez l’onglet de l’application dans la fenêtre du navigateur.**</span><span class="sxs-lookup"><span data-stu-id="09104-151">**Reselect the app's tab in the browser window.**</span></span>

   <span data-ttu-id="09104-152">Pour attacher l’application à Visual Studio, consultez la section [attacher au processus dans Visual Studio](#attach-to-process-in-visual-studio) .</span><span class="sxs-lookup"><span data-stu-id="09104-152">To attach the app to Visual Studio, see the [Attach to process in Visual Studio](#attach-to-process-in-visual-studio) section.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="09104-153">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="09104-153">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="09104-154">Exécutez une application webassembly éblouissante dans `Debug` configuration en passant l’option `--configuration Debug` à la commande [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.</span><span class="sxs-lookup"><span data-stu-id="09104-154">Run a Blazor WebAssembly app in `Debug` configuration by passing the `--configuration Debug` option to the [dotnet run](/dotnet/core/tools/dotnet-run) command: `dotnet run --configuration Debug`.</span></span>
1. <span data-ttu-id="09104-155">Accédez à l’application à l’URL HTTP indiquée dans la fenêtre de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="09104-155">Navigate to the app at the HTTP URL shown in the shell's window.</span></span>
1. <span data-ttu-id="09104-156">Placez le focus clavier sur l’application, et non sur le panneau Outils de développement.</span><span class="sxs-lookup"><span data-stu-id="09104-156">Place the keyboard focus on the app, not the developer tools panel.</span></span> <span data-ttu-id="09104-157">Il est préférable de garder le panneau Outils de développement fermé pour cette procédure.</span><span class="sxs-lookup"><span data-stu-id="09104-157">It's best to keep the developer tools panel closed for this procedure.</span></span> <span data-ttu-id="09104-158">Une fois le débogage démarré, vous pouvez rouvrir le panneau Outils de développement.</span><span class="sxs-lookup"><span data-stu-id="09104-158">After debugging has started, you can re-open the developer tools panel.</span></span>
1. <span data-ttu-id="09104-159">Sélectionnez le raccourci clavier de éblouissant spécifique suivant :</span><span class="sxs-lookup"><span data-stu-id="09104-159">Select the following Blazor-specific keyboard shortcut:</span></span>

   * <span data-ttu-id="09104-160">`Shift+Alt+D` sur Windows</span><span class="sxs-lookup"><span data-stu-id="09104-160">`Shift+Alt+D` on Windows</span></span>
   * <span data-ttu-id="09104-161">`Shift+Cmd+D` sur macOS</span><span class="sxs-lookup"><span data-stu-id="09104-161">`Shift+Cmd+D` on macOS</span></span>

   <span data-ttu-id="09104-162">Si vous recevez l' **onglet impossible de trouver le navigateur pouvant être débogué**, consultez [activer le débogage distant](#enable-remote-debugging).</span><span class="sxs-lookup"><span data-stu-id="09104-162">If you receive the **Unable to find debuggable browser tab**, see [Enable remote debugging](#enable-remote-debugging).</span></span>
   
   <span data-ttu-id="09104-163">Après l’activation du débogage distant :</span><span class="sxs-lookup"><span data-stu-id="09104-163">After enabling remote debugging:</span></span>
   
   <span data-ttu-id="09104-164">1 \.</span><span class="sxs-lookup"><span data-stu-id="09104-164">1\.</span></span> <span data-ttu-id="09104-165">Une nouvelle fenêtre de navigateur s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="09104-165">A new browser window opens.</span></span> <span data-ttu-id="09104-166">Fermez la fenêtre précédente.</span><span class="sxs-lookup"><span data-stu-id="09104-166">Close the prior window.</span></span>

   <span data-ttu-id="09104-167">2 \.</span><span class="sxs-lookup"><span data-stu-id="09104-167">2\.</span></span> <span data-ttu-id="09104-168">Placez le focus clavier sur l’application dans la fenêtre du navigateur, et non dans le panneau Outils de développement.</span><span class="sxs-lookup"><span data-stu-id="09104-168">Place the keyboard focus on the app in the browser window, not the developer tools panel.</span></span>

   <span data-ttu-id="09104-169">3 \.</span><span class="sxs-lookup"><span data-stu-id="09104-169">3\.</span></span> <span data-ttu-id="09104-170">Sélectionnez le raccourci clavier de la nouvelle fenêtre de navigateur : `Shift+Alt+D` sur Windows ou sur `Shift+Cmd+D` sur macOS.</span><span class="sxs-lookup"><span data-stu-id="09104-170">Select the Blazor-specific keyboard shortcut in the new browser window: `Shift+Alt+D` on Windows or `Shift+Cmd+D` on macOS.</span></span>

---

## <a name="enable-remote-debugging"></a><span data-ttu-id="09104-171">Activer le débogage à distance</span><span class="sxs-lookup"><span data-stu-id="09104-171">Enable remote debugging</span></span>

<span data-ttu-id="09104-172">Si le débogage distant est désactivé, une page d’erreur **Impossible de trouver un onglet de navigateur pouvant être débogué** est générée par chrome.</span><span class="sxs-lookup"><span data-stu-id="09104-172">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is generated by Chrome.</span></span> <span data-ttu-id="09104-173">La page d’erreur contient des instructions pour l’exécution de chrome avec le port de débogage ouvert, afin que le Blazor le proxy de débogage puissent se connecter à l’application.</span><span class="sxs-lookup"><span data-stu-id="09104-173">The error page contains instructions for running Chrome with the debugging port open so that the Blazor debugging proxy can connect to the app.</span></span> <span data-ttu-id="09104-174">*Fermez toutes les instances chrome* et redémarrez Chrome comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="09104-174">*Close all Chrome instances* and restart Chrome as instructed.</span></span>

## <a name="debug-the-app"></a><span data-ttu-id="09104-175">Déboguer l’application</span><span class="sxs-lookup"><span data-stu-id="09104-175">Debug the app</span></span>

<span data-ttu-id="09104-176">Une fois que Chrome s’exécute avec le débogage distant activé, le raccourci clavier de débogage ouvre un nouvel onglet du débogueur. Après un moment, l’onglet **sources** affiche une liste des assemblys .net dans l’application.</span><span class="sxs-lookup"><span data-stu-id="09104-176">Once Chrome is running with remote debugging enabled, the debugging keyboard shortcut opens a new debugger tab. After a moment, the **Sources** tab shows a list of the .NET assemblies in the app.</span></span> <span data-ttu-id="09104-177">Développez chaque assembly et recherchez les fichiers sources *. cs*/ *. Razor* disponibles pour le débogage.</span><span class="sxs-lookup"><span data-stu-id="09104-177">Expand each assembly and find the *.cs*/*.razor* source files available for debugging.</span></span> <span data-ttu-id="09104-178">Définissez des points d’arrêt, revenez à l’onglet de l’application, et les points d’arrêt sont atteints lorsque le code s’exécute.</span><span class="sxs-lookup"><span data-stu-id="09104-178">Set breakpoints, switch back to the app's tab, and the breakpoints are hit when the code executes.</span></span> <span data-ttu-id="09104-179">Après l’accès à un point d’arrêt, vous devez effectuer une seule étape (`F10`) par le biais de l’exécution du code ou de la reprise (`F8`) normalement.</span><span class="sxs-lookup"><span data-stu-id="09104-179">After a breakpoint is hit, single-step (`F10`) through the code or resume (`F8`) code execution normally.</span></span>

Blazor<span data-ttu-id="09104-180"> fournit un proxy de débogage qui implémente le [protocole chrome devtools](https://chromedevtools.github.io/devtools-protocol/) et qui augmente le protocole avec. Informations spécifiques à .net.</span><span class="sxs-lookup"><span data-stu-id="09104-180"> provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="09104-181">Quand l’utilisateur clique sur le raccourci de débogage, Blazor pointe le chrome DevTools au niveau du proxy.</span><span class="sxs-lookup"><span data-stu-id="09104-181">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="09104-182">Le proxy se connecte à la fenêtre du navigateur que vous cherchez à déboguer (par conséquent, il est nécessaire d’activer le débogage distant).</span><span class="sxs-lookup"><span data-stu-id="09104-182">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="attach-to-process-in-visual-studio"></a><span data-ttu-id="09104-183">Attacher au processus dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09104-183">Attach to process in Visual Studio</span></span>

<span data-ttu-id="09104-184">L’attachement au processus de l’application dans Visual Studio est un scénario de débogage *temporaire* pour Blazor webassembly pendant le débogage **F5** en cours de développement.</span><span class="sxs-lookup"><span data-stu-id="09104-184">Attaching to the app's process in Visual Studio is a *temporary* debugging scenario for Blazor WebAssembly while **F5** debugging is in development.</span></span>

<span data-ttu-id="09104-185">Pour attacher le processus de l’application en cours d’exécution à Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="09104-185">To attach the running app's process to Visual Studio:</span></span>

1. <span data-ttu-id="09104-186">Dans Visual Studio, sélectionnez **Déboguer** > **attacher au processus**.</span><span class="sxs-lookup"><span data-stu-id="09104-186">In Visual Studio, select **Debug** > **Attach to Process**.</span></span>
1. <span data-ttu-id="09104-187">Pour le **type de connexion**, sélectionnez **chrome devtools protocole WebSocket (aucune authentification)** .</span><span class="sxs-lookup"><span data-stu-id="09104-187">For the **Connection type**, select **Chrome devtools protocol websocket (no authentication)**.</span></span>
1. <span data-ttu-id="09104-188">Pour la **cible de connexion**, collez l’adresse http (et non l’adresse https) de l’application.</span><span class="sxs-lookup"><span data-stu-id="09104-188">For the **Connection target**, paste in the HTTP address (not the HTTPS address) of the app.</span></span>
1. <span data-ttu-id="09104-189">Sélectionnez **Actualiser** pour actualiser les entrées sous **processus disponibles**.</span><span class="sxs-lookup"><span data-stu-id="09104-189">Select **Refresh** to refresh the entries under **Available processes**.</span></span>
1. <span data-ttu-id="09104-190">Sélectionnez le processus du navigateur à déboguer, puis sélectionnez **attacher**.</span><span class="sxs-lookup"><span data-stu-id="09104-190">Select the browser process to debug and select **Attach**.</span></span>
1. <span data-ttu-id="09104-191">Dans la boîte de dialogue **Sélectionner le type de code** , sélectionnez le type de code pour le navigateur spécifique auquel vous voulez attacher (Edge ou chrome), puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="09104-191">In the **Select Code Type** dialog, select the code type for the specific browser you're attaching to (Edge or Chrome) and then select **OK**.</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="09104-192">Mappages des sources du navigateur</span><span class="sxs-lookup"><span data-stu-id="09104-192">Browser source maps</span></span>

<span data-ttu-id="09104-193">Les mappages de source de navigateur permettent au navigateur de mapper les fichiers compilés à leurs fichiers sources d’origine et sont couramment utilisés pour le débogage côté client.</span><span class="sxs-lookup"><span data-stu-id="09104-193">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="09104-194">Toutefois, Blazor n’est pas C# actuellement mappé directement à JavaScript/WASM.</span><span class="sxs-lookup"><span data-stu-id="09104-194">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="09104-195">Au lieu de cela, Blazor effectue une interprétation IL dans le navigateur, de sorte que les mappages de source ne sont pas pertinents.</span><span class="sxs-lookup"><span data-stu-id="09104-195">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="09104-196">Dépanner</span><span class="sxs-lookup"><span data-stu-id="09104-196">Troubleshoot</span></span>

<span data-ttu-id="09104-197">Si vous rencontrez des erreurs, le Conseil suivant peut vous aider :</span><span class="sxs-lookup"><span data-stu-id="09104-197">If you're running into errors, the following tip may help:</span></span>

<span data-ttu-id="09104-198">Dans l’onglet **débogueur** , ouvrez les outils de développement de votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="09104-198">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="09104-199">Dans la console, exécutez `localStorage.clear()` pour supprimer les points d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="09104-199">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
