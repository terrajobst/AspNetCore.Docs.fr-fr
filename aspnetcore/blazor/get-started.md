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
ms.openlocfilehash: 2135c2a090d60ec7a46fa4f899f0f14987b6b4e0
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951720"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="63881-103">Prise en main de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="63881-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="63881-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="63881-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="63881-105">Prise en main de Blazor:</span><span class="sxs-lookup"><span data-stu-id="63881-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="63881-106">Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="63881-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="63881-107">Installez éventuellement le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="63881-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="63881-108">Installez le [Kit de développement logiciel (SDK) .net Core 3,1 ou ultérieur (version préliminaire)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="63881-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="63881-109">Exécutez la commande suivante dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="63881-109">Run the following command in a command shell.</span></span> <span data-ttu-id="63881-110">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Le package de modèles a une version préliminaire alors que Blazor Webassembly est en préversion.</span><span class="sxs-lookup"><span data-stu-id="63881-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="63881-111">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="63881-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63881-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63881-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="63881-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="63881-113">1\.</span></span> <span data-ttu-id="63881-114">Installez [Visual Studio 16,4 ou une version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="63881-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="63881-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="63881-115">2\.</span></span> <span data-ttu-id="63881-116">Créez un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="63881-116">Create a new project.</span></span>

   <span data-ttu-id="63881-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="63881-117">3\.</span></span> <span data-ttu-id="63881-118">Sélectionnez **Blazor application**.</span><span class="sxs-lookup"><span data-stu-id="63881-118">Select **Blazor App**.</span></span> <span data-ttu-id="63881-119">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="63881-119">Select **Next**.</span></span>

   <span data-ttu-id="63881-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="63881-120">4\.</span></span> <span data-ttu-id="63881-121">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="63881-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="63881-122">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="63881-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="63881-123">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="63881-123">Select **Create**.</span></span>

   <span data-ttu-id="63881-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="63881-124">5\.</span></span> <span data-ttu-id="63881-125">Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="63881-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="63881-126">Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="63881-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="63881-127">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="63881-127">Select **Create**.</span></span> <span data-ttu-id="63881-128">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="63881-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="63881-129">6\.</span></span> <span data-ttu-id="63881-130">Appuyez sur **Ctrl**+**F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="63881-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="63881-131">Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="63881-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="63881-132">L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="63881-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63881-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63881-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="63881-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="63881-134">1\.</span></span> <span data-ttu-id="63881-135">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="63881-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="63881-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="63881-136">2\.</span></span> <span data-ttu-id="63881-137">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="63881-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="63881-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="63881-138">3\.</span></span> <span data-ttu-id="63881-139">Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="63881-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="63881-140">Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="63881-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="63881-141">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="63881-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="63881-142">4\.</span></span> <span data-ttu-id="63881-143">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="63881-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="63881-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="63881-144">5\.</span></span> <span data-ttu-id="63881-145">Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="63881-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="63881-146">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="63881-146">Select **Yes**.</span></span>

   <span data-ttu-id="63881-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="63881-147">6\.</span></span> <span data-ttu-id="63881-148">Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="63881-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="63881-149">Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="63881-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="63881-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="63881-150">7\.</span></span> <span data-ttu-id="63881-151">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="63881-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63881-152">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="63881-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="63881-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="63881-153">1\.</span></span> <span data-ttu-id="63881-154">Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="63881-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="63881-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="63881-155">2\.</span></span> <span data-ttu-id="63881-156">Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="63881-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="63881-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="63881-157">3\.</span></span> <span data-ttu-id="63881-158">Dans la barre latérale, sélectionnez **.net Core** > **application**.</span><span class="sxs-lookup"><span data-stu-id="63881-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="63881-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="63881-159">4\.</span></span> <span data-ttu-id="63881-160">Sélectionnez le modèle **application serveurBlazor** .</span><span class="sxs-lookup"><span data-stu-id="63881-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="63881-161">Seul le modèle Blazor Server est disponible dans Visual Studio pour Mac pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="63881-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="63881-162">Pour une expérience Blazor webassembly, suivez les instructions de l’onglet **CLI .net Core** . Après avoir sélectionné le modèle Blazor Server, sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="63881-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="63881-163">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="63881-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="63881-164">5\.</span></span> <span data-ttu-id="63881-165">Définissez la version **cible** de **.NET Framework sur .net Core 3,1** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="63881-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="63881-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="63881-166">6\.</span></span> <span data-ttu-id="63881-167">Dans le champ **nom du projet** , nommez l’application `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="63881-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="63881-168">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="63881-168">Select **Create**.</span></span>

   <span data-ttu-id="63881-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="63881-169">7\.</span></span> <span data-ttu-id="63881-170">Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="63881-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="63881-171">Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="63881-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="63881-172">Si une invite s’affiche pour faire confiance au certificat de développement, approuvez le certificat et continuez.</span><span class="sxs-lookup"><span data-stu-id="63881-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="63881-173">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="63881-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="63881-174">Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="63881-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="63881-175">Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="63881-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="63881-176">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="63881-177">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="63881-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="63881-178">Installez le dernier [Kit de développement logiciel (SDK) .net Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span><span class="sxs-lookup"><span data-stu-id="63881-178">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0).</span></span>

1. <span data-ttu-id="63881-179">Installez éventuellement le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="63881-179">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="63881-180">Installez le [Kit de développement logiciel (SDK) .net Core 3,1 ou ultérieur (version préliminaire)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="63881-180">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="63881-181">Exécutez la commande suivante dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="63881-181">Run the following command in a command shell.</span></span> <span data-ttu-id="63881-182">[Microsoft.AspNetCore.Blazor.](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Le package de modèles a une version préliminaire alors que Blazor Webassembly est en préversion.</span><span class="sxs-lookup"><span data-stu-id="63881-182">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="63881-183">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="63881-183">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63881-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63881-184">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="63881-185">1 \.</span><span class="sxs-lookup"><span data-stu-id="63881-185">1\.</span></span> <span data-ttu-id="63881-186">Installez la dernière version de [Visual Studio](https://visualstudio.com/vs/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="63881-186">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="63881-187">2 \.</span><span class="sxs-lookup"><span data-stu-id="63881-187">2\.</span></span> <span data-ttu-id="63881-188">Si vous le souhaitez, vous pouvez installer [Visual Studio 16,4 Preview 2 ou version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** pour Blazor développement d’applications webassembly.</span><span class="sxs-lookup"><span data-stu-id="63881-188">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="63881-189">3 \.</span><span class="sxs-lookup"><span data-stu-id="63881-189">3\.</span></span> <span data-ttu-id="63881-190">Créez un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="63881-190">Create a new project.</span></span>

   <span data-ttu-id="63881-191">4 \.</span><span class="sxs-lookup"><span data-stu-id="63881-191">4\.</span></span> <span data-ttu-id="63881-192">Sélectionnez **Blazor application**.</span><span class="sxs-lookup"><span data-stu-id="63881-192">Select **Blazor App**.</span></span> <span data-ttu-id="63881-193">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="63881-193">Select **Next**.</span></span>

   <span data-ttu-id="63881-194">5 \.</span><span class="sxs-lookup"><span data-stu-id="63881-194">5\.</span></span> <span data-ttu-id="63881-195">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="63881-195">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="63881-196">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="63881-196">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="63881-197">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="63881-197">Select **Create**.</span></span>

   <span data-ttu-id="63881-198">6 \.</span><span class="sxs-lookup"><span data-stu-id="63881-198">6\.</span></span> <span data-ttu-id="63881-199">Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="63881-199">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="63881-200">Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="63881-200">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="63881-201">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="63881-201">Select **Create**.</span></span> <span data-ttu-id="63881-202">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-202">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="63881-203">7 \.</span><span class="sxs-lookup"><span data-stu-id="63881-203">7\.</span></span> <span data-ttu-id="63881-204">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="63881-204">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="63881-205">Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="63881-205">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="63881-206">L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="63881-206">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63881-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63881-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="63881-208">1 \.</span><span class="sxs-lookup"><span data-stu-id="63881-208">1\.</span></span> <span data-ttu-id="63881-209">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="63881-209">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="63881-210">2 \.</span><span class="sxs-lookup"><span data-stu-id="63881-210">2\.</span></span> <span data-ttu-id="63881-211">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="63881-211">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="63881-212">3 \.</span><span class="sxs-lookup"><span data-stu-id="63881-212">3\.</span></span> <span data-ttu-id="63881-213">Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="63881-213">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="63881-214">Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="63881-214">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="63881-215">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-215">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="63881-216">4 \.</span><span class="sxs-lookup"><span data-stu-id="63881-216">4\.</span></span> <span data-ttu-id="63881-217">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="63881-217">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="63881-218">5 \.</span><span class="sxs-lookup"><span data-stu-id="63881-218">5\.</span></span> <span data-ttu-id="63881-219">Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="63881-219">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="63881-220">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="63881-220">Select **Yes**.</span></span>

   <span data-ttu-id="63881-221">6 \.</span><span class="sxs-lookup"><span data-stu-id="63881-221">6\.</span></span> <span data-ttu-id="63881-222">Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="63881-222">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="63881-223">Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="63881-223">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="63881-224">7 \.</span><span class="sxs-lookup"><span data-stu-id="63881-224">7\.</span></span> <span data-ttu-id="63881-225">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="63881-225">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63881-226">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="63881-226">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="63881-227">1 \.</span><span class="sxs-lookup"><span data-stu-id="63881-227">1\.</span></span> <span data-ttu-id="63881-228">Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="63881-228">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span> <span data-ttu-id="63881-229">Basculez le [canal de mise à jour vers](/visualstudio/mac/install-preview)la version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="63881-229">Switch the [Update channel to Preview](/visualstudio/mac/install-preview).</span></span>

   <span data-ttu-id="63881-230">2 \.</span><span class="sxs-lookup"><span data-stu-id="63881-230">2\.</span></span> <span data-ttu-id="63881-231">Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="63881-231">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="63881-232">3 \.</span><span class="sxs-lookup"><span data-stu-id="63881-232">3\.</span></span> <span data-ttu-id="63881-233">Dans la barre latérale, sélectionnez **.net Core** > **application**.</span><span class="sxs-lookup"><span data-stu-id="63881-233">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="63881-234">4 \.</span><span class="sxs-lookup"><span data-stu-id="63881-234">4\.</span></span> <span data-ttu-id="63881-235">Sélectionnez le modèle **application serveurBlazor** .</span><span class="sxs-lookup"><span data-stu-id="63881-235">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="63881-236">Seul le modèle Blazor Server est disponible dans Visual Studio pour Mac pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="63881-236">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="63881-237">Pour une expérience Blazor webassembly, suivez les instructions de l’onglet **CLI .net Core** . Après avoir sélectionné le modèle Blazor Server, sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="63881-237">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="63881-238">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-238">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="63881-239">5 \.</span><span class="sxs-lookup"><span data-stu-id="63881-239">5\.</span></span> <span data-ttu-id="63881-240">Définissez la version **cible** de **.NET Framework sur .net Core 3,0** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="63881-240">Set the **Target Framework** to **.NET Core 3.0** and select **Next**.</span></span>

   <span data-ttu-id="63881-241">6 \.</span><span class="sxs-lookup"><span data-stu-id="63881-241">6\.</span></span> <span data-ttu-id="63881-242">Dans le champ **nom du projet** , nommez l’application `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="63881-242">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="63881-243">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="63881-243">Select **Create**.</span></span>

   <span data-ttu-id="63881-244">7 \.</span><span class="sxs-lookup"><span data-stu-id="63881-244">7\.</span></span> <span data-ttu-id="63881-245">Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="63881-245">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="63881-246">Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="63881-246">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="63881-247">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="63881-247">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="63881-248">Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="63881-248">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="63881-249">Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="63881-249">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="63881-250">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="63881-250">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="63881-251">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="63881-251">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="63881-252">Plusieurs pages sont disponibles à partir des onglets de la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="63881-252">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="63881-253">Accueil</span><span class="sxs-lookup"><span data-stu-id="63881-253">Home</span></span>
* <span data-ttu-id="63881-254">Counter</span><span class="sxs-lookup"><span data-stu-id="63881-254">Counter</span></span>
* <span data-ttu-id="63881-255">Extraire les données</span><span class="sxs-lookup"><span data-stu-id="63881-255">Fetch data</span></span>

<span data-ttu-id="63881-256">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="63881-256">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="63881-257">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais C#avec Blazor vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="63881-257">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="63881-258">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="63881-258">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="63881-259">Une demande d' `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="63881-259">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="63881-260">Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="63881-260">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="63881-261">Chaque fois que le bouton **Click Me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="63881-261">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="63881-262">L’événement `onclick` est déclenché.</span><span class="sxs-lookup"><span data-stu-id="63881-262">The `onclick` event is fired.</span></span>
* <span data-ttu-id="63881-263">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="63881-263">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="63881-264">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="63881-264">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="63881-265">Le composant est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="63881-265">The component is rendered again.</span></span>

<span data-ttu-id="63881-266">Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="63881-266">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="63881-267">Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="63881-267">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="63881-268">Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="63881-268">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="63881-269">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="63881-269">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="63881-270">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="63881-270">Run the app.</span></span> <span data-ttu-id="63881-271">La page d’accueil possède son propre compteur fourni par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="63881-271">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="63881-272">Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="63881-272">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="63881-273">Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="63881-273">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="63881-274">Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="63881-274">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="63881-275">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="63881-275">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="63881-276">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="63881-276">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="63881-277">Spécifiez le `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="63881-277">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="63881-278">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="63881-278">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="63881-279">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="63881-279">Run the app.</span></span> <span data-ttu-id="63881-280">Le composant `Index` possède son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="63881-280">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="63881-281">Le composant `Counter` (*Counter. Razor*) sur `/counter` continue d’être incrémenté d’une unité.</span><span class="sxs-lookup"><span data-stu-id="63881-281">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63881-282">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="63881-282">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="63881-283">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="63881-283">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
