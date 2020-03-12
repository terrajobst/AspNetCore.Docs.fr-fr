---
title: Prise en main de ASP.NET Core Blazor
author: guardrex
description: Commencez avec Blazor en créant une application Blazor avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083237"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="4d6a0-103">Prise en main d’ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="4d6a0-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="4d6a0-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4d6a0-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="4d6a0-105">Prise en main de éblouissant :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="4d6a0-106">Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="4d6a0-107">Installez éventuellement le modèle de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="4d6a0-108">Installez le [Kit de développement logiciel (SDK) .net Core 3.1.102 ou version ultérieure (version préliminaire)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-108">Install the [.NET Core 3.1.102 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="4d6a0-109">Exécutez la commande suivante dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-109">Run the following command in a command shell.</span></span> <span data-ttu-id="4d6a0-110">Le package [Microsoft. AspNetCore. Components. Webassembly. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) a une préversion alors que le composant webassembly éblouissant est en version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-110">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > <span data-ttu-id="4d6a0-111">Kit SDK .NET Core version 3.1.102 ou ultérieure est **nécessaire** pour utiliser le modèle webassembly 3,2 Preview 2.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-111">.NET Core SDK version 3.1.102 or later is **required** to use the 3.2 Preview 2 Blazor WebAssembly template.</span></span> <span data-ttu-id="4d6a0-112">Confirmez la version de kit SDK .NET Core installée en exécutant `dotnet --version` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-112">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="4d6a0-113">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-113">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studio"></a>[<span data-ttu-id="4d6a0-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d6a0-114">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="4d6a0-115">1 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-115">1\.</span></span> <span data-ttu-id="4d6a0-116">Installez [Visual Studio 2019 version 16,4 ou ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="4d6a0-116">Install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="4d6a0-117">2 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-117">2\.</span></span> <span data-ttu-id="4d6a0-118">Créez un projet.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-118">Create a new project.</span></span>

   <span data-ttu-id="4d6a0-119">3 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-119">3\.</span></span> <span data-ttu-id="4d6a0-120">Sélectionnez l' **application éblouissant**.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-120">Select **Blazor App**.</span></span> <span data-ttu-id="4d6a0-121">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-121">Select **Next**.</span></span>

   <span data-ttu-id="4d6a0-122">4 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-122">4\.</span></span> <span data-ttu-id="4d6a0-123">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-123">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="4d6a0-124">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="4d6a0-125">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-125">Select **Create**.</span></span>

   <span data-ttu-id="4d6a0-126">5 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-126">5\.</span></span> <span data-ttu-id="4d6a0-127">Pour une expérience de webassembly éblouissant, choisissez le modèle **application éblouissant Webassembly** .</span><span class="sxs-lookup"><span data-stu-id="4d6a0-127">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="4d6a0-128">Pour une expérience de serveur éblouissant, choisissez le modèle **application de serveur éblouissant** .</span><span class="sxs-lookup"><span data-stu-id="4d6a0-128">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="4d6a0-129">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-129">Select **Create**.</span></span> <span data-ttu-id="4d6a0-130">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-130">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span> <span data-ttu-id="4d6a0-131">Si le modèle de webassembly éblouissant n’est pas présent, revenez à l’étape précédente et réinstallez le modèle.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-131">If the Blazor WebAssembly template isn't present, return to the previous step and reinstall the template.</span></span>

   <span data-ttu-id="4d6a0-132">6 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-132">6\.</span></span> <span data-ttu-id="4d6a0-133">Appuyez sur **Ctrl**+**F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-133">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4d6a0-134">Si vous avez installé l’extension Visual Studio Blazor pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-134">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="4d6a0-135">L’installation des modèles Blazor dans un interpréteur de commandes est désormais suffisante pour faire apparaître les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-135">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-code"></a>[<span data-ttu-id="4d6a0-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4d6a0-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="4d6a0-137">1 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-137">1\.</span></span> <span data-ttu-id="4d6a0-138">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-138">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="4d6a0-139">2 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-139">2\.</span></span> <span data-ttu-id="4d6a0-140">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-140">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

   <span data-ttu-id="4d6a0-141">3 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-141">3\.</span></span> <span data-ttu-id="4d6a0-142">Pour une expérience de webassembly Blazor, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-142">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="4d6a0-143">Pour une expérience de serveur Blazor, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-143">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="4d6a0-144">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-144">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="4d6a0-145">4 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-145">4\.</span></span> <span data-ttu-id="4d6a0-146">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-146">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="4d6a0-147">5 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-147">5\.</span></span> <span data-ttu-id="4d6a0-148">Pour un projet de serveur Blazor, l’IDE demande que vous ajoutiez des ressources pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-148">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="4d6a0-149">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-149">Select **Yes**.</span></span>

   <span data-ttu-id="4d6a0-150">6 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-150">6\.</span></span> <span data-ttu-id="4d6a0-151">Si vous utilisez une application de serveur Blazor, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-151">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="4d6a0-152">Si vous utilisez une application de webassembly éblouissante, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-152">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="4d6a0-153">7 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-153">7\.</span></span> <span data-ttu-id="4d6a0-154">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mac"></a>[<span data-ttu-id="4d6a0-155">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="4d6a0-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="4d6a0-156">1 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-156">1\.</span></span> <span data-ttu-id="4d6a0-157">Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-157">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="4d6a0-158">2 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-158">2\.</span></span> <span data-ttu-id="4d6a0-159">Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-159">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="4d6a0-160">3 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-160">3\.</span></span> <span data-ttu-id="4d6a0-161">Dans la barre latérale, sélectionnez **.net Core** > **application**.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-161">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="4d6a0-162">4 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-162">4\.</span></span> <span data-ttu-id="4d6a0-163">Sélectionnez le modèle **application de serveur éblouissant** .</span><span class="sxs-lookup"><span data-stu-id="4d6a0-163">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="4d6a0-164">Seul le modèle de serveur éblouissant est disponible dans Visual Studio pour Mac pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-164">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="4d6a0-165">Pour une expérience de webassembly éblouissant, suivez les instructions de l’onglet **CLI .net Core** . Après avoir sélectionné le modèle de serveur éblouissant, sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-165">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="4d6a0-166">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-166">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="4d6a0-167">5 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-167">5\.</span></span> <span data-ttu-id="4d6a0-168">Définissez la version **cible** de **.NET Framework sur .net Core 3,1** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-168">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="4d6a0-169">6 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-169">6\.</span></span> <span data-ttu-id="4d6a0-170">Dans le champ **nom du projet** , nommez l’application `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-170">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="4d6a0-171">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-171">Select **Create**.</span></span>

   <span data-ttu-id="4d6a0-172">7 \.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-172">7\.</span></span> <span data-ttu-id="4d6a0-173">Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-173">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="4d6a0-174">Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-174">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="4d6a0-175">Si une invite s’affiche pour faire confiance au certificat de développement, approuvez le certificat et continuez.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-175">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-cli"></a>[<span data-ttu-id="4d6a0-176">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4d6a0-176">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="4d6a0-177">Pour une expérience de webassembly Blazor, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-177">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4d6a0-178">Pour une expérience de serveur Blazor, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-178">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="4d6a0-179">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-179">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="4d6a0-180">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-180">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="4d6a0-181">Plusieurs pages sont disponibles à partir des onglets de la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-181">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="4d6a0-182">Accueil</span><span class="sxs-lookup"><span data-stu-id="4d6a0-182">Home</span></span>
* <span data-ttu-id="4d6a0-183">Compteur</span><span class="sxs-lookup"><span data-stu-id="4d6a0-183">Counter</span></span>
* <span data-ttu-id="4d6a0-184">Extraire les données</span><span class="sxs-lookup"><span data-stu-id="4d6a0-184">Fetch data</span></span>

<span data-ttu-id="4d6a0-185">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-185">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="4d6a0-186">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais C#avec Blazor vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-186">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="4d6a0-187">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-187">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="4d6a0-188">Une demande d' `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-188">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="4d6a0-189">Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-189">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="4d6a0-190">Chaque fois que le bouton **Click Me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-190">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="4d6a0-191">L’événement `onclick` est déclenché.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-191">The `onclick` event is fired.</span></span>
* <span data-ttu-id="4d6a0-192">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-192">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="4d6a0-193">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-193">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="4d6a0-194">Le composant est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-194">The component is rendered again.</span></span>

<span data-ttu-id="4d6a0-195">Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="4d6a0-195">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="4d6a0-196">Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-196">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="4d6a0-197">Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-197">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="4d6a0-198">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-198">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="4d6a0-199">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-199">Run the app.</span></span> <span data-ttu-id="4d6a0-200">La page d’accueil possède son propre compteur fourni par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-200">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="4d6a0-201">Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-201">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="4d6a0-202">Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-202">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="4d6a0-203">Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-203">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="4d6a0-204">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-204">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="4d6a0-205">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-205">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="4d6a0-206">Spécifiez le `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-206">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="4d6a0-207">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="4d6a0-207">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="4d6a0-208">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-208">Run the app.</span></span> <span data-ttu-id="4d6a0-209">Le composant `Index` possède son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-209">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="4d6a0-210">Le composant `Counter` (*Counter. Razor*) sur `/counter` continue d’être incrémenté d’une unité.</span><span class="sxs-lookup"><span data-stu-id="4d6a0-210">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d6a0-211">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="4d6a0-211">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="4d6a0-212">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4d6a0-212">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
