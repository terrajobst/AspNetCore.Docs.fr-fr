---
title: Prise en main de ASP.NET Core Blazor
author: guardrex
description: Commencez avec Blazor en créant une application Blazor avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: e368ecaf931d392de7e52ec2d5a2dfd171c2c86f
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943761"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="af050-103">Prise en main de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="af050-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="af050-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="af050-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="af050-105">Prise en main de Blazor:</span><span class="sxs-lookup"><span data-stu-id="af050-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="af050-106">Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="af050-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="af050-107">Installez éventuellement le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="af050-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="af050-108">Installez le [Kit de développement logiciel (SDK) .net Core 3,1 ou ultérieur (version préliminaire)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="af050-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="af050-109">Exécutez la commande suivante dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="af050-109">Run the following command in a command shell.</span></span> <span data-ttu-id="af050-110">[Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Le package de modèles a une version préliminaire alors que Blazor Webassembly est en préversion.</span><span class="sxs-lookup"><span data-stu-id="af050-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="af050-111">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="af050-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af050-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af050-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="af050-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="af050-113">1\.</span></span> <span data-ttu-id="af050-114">Installez [Visual Studio 16,4 ou une version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="af050-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="af050-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="af050-115">2\.</span></span> <span data-ttu-id="af050-116">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="af050-116">Create a new project.</span></span>

   <span data-ttu-id="af050-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="af050-117">3\.</span></span> <span data-ttu-id="af050-118">Sélectionnez **Blazor application**.</span><span class="sxs-lookup"><span data-stu-id="af050-118">Select **Blazor App**.</span></span> <span data-ttu-id="af050-119">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="af050-119">Select **Next**.</span></span>

   <span data-ttu-id="af050-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="af050-120">4\.</span></span> <span data-ttu-id="af050-121">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="af050-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="af050-122">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="af050-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="af050-123">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="af050-123">Select **Create**.</span></span>

   <span data-ttu-id="af050-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="af050-124">5\.</span></span> <span data-ttu-id="af050-125">Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="af050-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="af050-126">Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="af050-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="af050-127">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="af050-127">Select **Create**.</span></span> <span data-ttu-id="af050-128">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="af050-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="af050-129">6\.</span></span> <span data-ttu-id="af050-130">Appuyez sur **Ctrl**+**F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="af050-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="af050-131">Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="af050-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="af050-132">L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af050-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="af050-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="af050-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="af050-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="af050-134">1\.</span></span> <span data-ttu-id="af050-135">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="af050-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="af050-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="af050-136">2\.</span></span> <span data-ttu-id="af050-137">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="af050-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="af050-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="af050-138">3\.</span></span> <span data-ttu-id="af050-139">Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="af050-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="af050-140">Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="af050-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="af050-141">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="af050-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="af050-142">4\.</span></span> <span data-ttu-id="af050-143">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="af050-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="af050-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="af050-144">5\.</span></span> <span data-ttu-id="af050-145">Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="af050-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="af050-146">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="af050-146">Select **Yes**.</span></span>

   <span data-ttu-id="af050-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="af050-147">6\.</span></span> <span data-ttu-id="af050-148">Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="af050-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="af050-149">Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="af050-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="af050-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="af050-150">7\.</span></span> <span data-ttu-id="af050-151">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="af050-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="af050-152">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="af050-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="af050-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="af050-153">1\.</span></span> <span data-ttu-id="af050-154">Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="af050-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="af050-155">Basculez le [canal de mise à jour vers](/visualstudio/mac/install-preview)la version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="af050-155">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="af050-156">2 \.</span><span class="sxs-lookup"><span data-stu-id="af050-156">2\.</span></span> <span data-ttu-id="af050-157">Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="af050-157">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="af050-158">3 \.</span><span class="sxs-lookup"><span data-stu-id="af050-158">3\.</span></span> <span data-ttu-id="af050-159">Dans la barre latérale, sélectionnez **.net Core** > **application**.</span><span class="sxs-lookup"><span data-stu-id="af050-159">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="af050-160">4 \.</span><span class="sxs-lookup"><span data-stu-id="af050-160">4\.</span></span> <span data-ttu-id="af050-161">Sélectionnez le modèle **application serveurBlazor** .</span><span class="sxs-lookup"><span data-stu-id="af050-161">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="af050-162">Seul le modèle Blazor Server est disponible dans Visual Studio pour Mac pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="af050-162">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="af050-163">Pour une expérience Blazor webassembly, suivez les instructions de l’onglet **CLI .net Core** . Après avoir sélectionné le modèle Blazor Server, sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="af050-163">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="af050-164">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-164">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="af050-165">5 \.</span><span class="sxs-lookup"><span data-stu-id="af050-165">5\.</span></span> <span data-ttu-id="af050-166">Définissez la version **cible** de **.NET Framework sur .net Core 3,1** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="af050-166">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="af050-167">6 \.</span><span class="sxs-lookup"><span data-stu-id="af050-167">6\.</span></span> <span data-ttu-id="af050-168">Dans le champ **nom du projet** , nommez l’application `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="af050-168">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="af050-169">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="af050-169">Select **Create**.</span></span>

   <span data-ttu-id="af050-170">7 \.</span><span class="sxs-lookup"><span data-stu-id="af050-170">7\.</span></span> <span data-ttu-id="af050-171">Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="af050-171">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="af050-172">Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="af050-172">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af050-173">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="af050-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="af050-174">Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="af050-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="af050-175">Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="af050-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="af050-176">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="af050-177">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="af050-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="af050-178">Installez le dernier [Kit de développement logiciel (SDK) .net Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="af050-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="af050-179">Installez éventuellement le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="af050-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="af050-180">Installez le [Kit de développement logiciel (SDK) .net Core 3,1 ou ultérieur (version préliminaire)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="af050-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="af050-181">Exécutez la commande suivante dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="af050-181">Run the following command in a command shell.</span></span> <span data-ttu-id="af050-182">[Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Le package de modèles a une version préliminaire alors que Blazor Webassembly est en préversion.</span><span class="sxs-lookup"><span data-stu-id="af050-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="af050-183">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="af050-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="af050-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af050-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="af050-185">1 \.</span><span class="sxs-lookup"><span data-stu-id="af050-185">1\.</span></span> <span data-ttu-id="af050-186">Installez la dernière version de [Visual Studio](https://visualstudio.com/vs/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="af050-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="af050-187">2 \.</span><span class="sxs-lookup"><span data-stu-id="af050-187">2\.</span></span> <span data-ttu-id="af050-188">Si vous le souhaitez, vous pouvez installer [Visual Studio 16,4 Preview 2 ou version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** pour Blazor développement d’applications webassembly.</span><span class="sxs-lookup"><span data-stu-id="af050-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="af050-189">3 \.</span><span class="sxs-lookup"><span data-stu-id="af050-189">3\.</span></span> <span data-ttu-id="af050-190">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="af050-190">Create a new project.</span></span>

   <span data-ttu-id="af050-191">4 \.</span><span class="sxs-lookup"><span data-stu-id="af050-191">4\.</span></span> <span data-ttu-id="af050-192">Sélectionnez **Blazor application**.</span><span class="sxs-lookup"><span data-stu-id="af050-192">Select **Blazor App**.</span></span> <span data-ttu-id="af050-193">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="af050-193">Select **Next**.</span></span>

   <span data-ttu-id="af050-194">5 \.</span><span class="sxs-lookup"><span data-stu-id="af050-194">5\.</span></span> <span data-ttu-id="af050-195">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="af050-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="af050-196">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="af050-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="af050-197">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="af050-197">Select **Create**.</span></span>

   <span data-ttu-id="af050-198">6 \.</span><span class="sxs-lookup"><span data-stu-id="af050-198">6\.</span></span> <span data-ttu-id="af050-199">Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="af050-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="af050-200">Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="af050-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="af050-201">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="af050-201">Select **Create**.</span></span> <span data-ttu-id="af050-202">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="af050-203">7 \.</span><span class="sxs-lookup"><span data-stu-id="af050-203">7\.</span></span> <span data-ttu-id="af050-204">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="af050-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="af050-205">Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="af050-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="af050-206">L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af050-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="af050-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="af050-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="af050-208">1 \.</span><span class="sxs-lookup"><span data-stu-id="af050-208">1\.</span></span> <span data-ttu-id="af050-209">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="af050-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="af050-210">2 \.</span><span class="sxs-lookup"><span data-stu-id="af050-210">2\.</span></span> <span data-ttu-id="af050-211">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="af050-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="af050-212">3 \.</span><span class="sxs-lookup"><span data-stu-id="af050-212">3\.</span></span> <span data-ttu-id="af050-213">Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="af050-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="af050-214">Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="af050-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="af050-215">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="af050-216">4 \.</span><span class="sxs-lookup"><span data-stu-id="af050-216">4\.</span></span> <span data-ttu-id="af050-217">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="af050-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="af050-218">5 \.</span><span class="sxs-lookup"><span data-stu-id="af050-218">5\.</span></span> <span data-ttu-id="af050-219">Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="af050-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="af050-220">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="af050-220">Select **Yes**.</span></span>

   <span data-ttu-id="af050-221">6 \.</span><span class="sxs-lookup"><span data-stu-id="af050-221">6\.</span></span> <span data-ttu-id="af050-222">Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="af050-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="af050-223">Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="af050-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="af050-224">7 \.</span><span class="sxs-lookup"><span data-stu-id="af050-224">7\.</span></span> <span data-ttu-id="af050-225">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="af050-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="af050-226">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="af050-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="af050-227">1 \.</span><span class="sxs-lookup"><span data-stu-id="af050-227">1\.</span></span> <span data-ttu-id="af050-228">Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="af050-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="af050-229">Basculez le [canal de mise à jour vers](/visualstudio/mac/install-preview)la version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="af050-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="af050-230">2 \.</span><span class="sxs-lookup"><span data-stu-id="af050-230">2\.</span></span> <span data-ttu-id="af050-231">Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="af050-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="af050-232">3 \.</span><span class="sxs-lookup"><span data-stu-id="af050-232">3\.</span></span> <span data-ttu-id="af050-233">Dans la barre latérale, sélectionnez **.net Core** > **application**.</span><span class="sxs-lookup"><span data-stu-id="af050-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="af050-234">4 \.</span><span class="sxs-lookup"><span data-stu-id="af050-234">4\.</span></span> <span data-ttu-id="af050-235">Sélectionnez le modèle **application serveurBlazor** .</span><span class="sxs-lookup"><span data-stu-id="af050-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="af050-236">Seul le modèle Blazor Server est disponible dans Visual Studio pour Mac pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="af050-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="af050-237">Pour une expérience Blazor webassembly, suivez les instructions de l’onglet **CLI .net Core** . Après avoir sélectionné le modèle Blazor Server, sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="af050-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="af050-238">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="af050-239">5 \.</span><span class="sxs-lookup"><span data-stu-id="af050-239">5\.</span></span> <span data-ttu-id="af050-240">Définissez la version **cible** de **.NET Framework sur .net Core 3,0** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="af050-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="af050-241">6 \.</span><span class="sxs-lookup"><span data-stu-id="af050-241">6\.</span></span> <span data-ttu-id="af050-242">Dans le champ **nom du projet** , nommez l’application `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="af050-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="af050-243">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="af050-243">Select **Create**.</span></span>

   <span data-ttu-id="af050-244">7 \.</span><span class="sxs-lookup"><span data-stu-id="af050-244">7\.</span></span> <span data-ttu-id="af050-245">Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="af050-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="af050-246">Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="af050-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="af050-247">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="af050-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="af050-248">Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="af050-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="af050-249">Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="af050-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="af050-250">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="af050-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="af050-251">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="af050-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="af050-252">Plusieurs pages sont disponibles à partir des onglets de la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="af050-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="af050-253">Dossier de base</span><span class="sxs-lookup"><span data-stu-id="af050-253">Home</span></span>
* <span data-ttu-id="af050-254">Counter</span><span class="sxs-lookup"><span data-stu-id="af050-254">Counter</span></span>
* <span data-ttu-id="af050-255">Extraire les données</span><span class="sxs-lookup"><span data-stu-id="af050-255">Fetch data</span></span>

<span data-ttu-id="af050-256">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="af050-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="af050-257">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais C#avec Blazor vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="af050-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="af050-258">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="af050-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="af050-259">Une demande d' `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="af050-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="af050-260">Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="af050-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="af050-261">Chaque fois que le bouton **Click Me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="af050-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="af050-262">L’événement `onclick` est déclenché.</span><span class="sxs-lookup"><span data-stu-id="af050-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="af050-263">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="af050-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="af050-264">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="af050-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="af050-265">Le composant est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="af050-265">The component is rendered again.</span></span>

<span data-ttu-id="af050-266">Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="af050-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="af050-267">Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="af050-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="af050-268">Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="af050-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="af050-269">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="af050-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="af050-270">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="af050-270">Run the app.</span></span> <span data-ttu-id="af050-271">La page d’accueil possède son propre compteur fourni par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="af050-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="af050-272">Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="af050-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="af050-273">Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="af050-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="af050-274">Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="af050-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="af050-275">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="af050-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="af050-276">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="af050-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="af050-277">Spécifiez le `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="af050-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="af050-278">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="af050-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="af050-279">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="af050-279">Run the app.</span></span> <span data-ttu-id="af050-280">Le composant `Index` possède son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="af050-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="af050-281">Le composant `Counter` (*Counter. Razor*) sur `/counter` continue d’être incrémenté d’une unité.</span><span class="sxs-lookup"><span data-stu-id="af050-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af050-282">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="af050-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="af050-283">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="af050-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
