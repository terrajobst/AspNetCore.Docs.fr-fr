---
title: Prise en main de ASP.NET Core Blazor
author: guardrex
description: Commencez avec Blazor en créant une application Blazor avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 9b4aee0be30568f098c756e9ab4cb5298e9a049b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963010"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="2d6eb-103">Prise en main de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="2d6eb-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="2d6eb-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2d6eb-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2d6eb-105">Prise en main de Blazor:</span><span class="sxs-lookup"><span data-stu-id="2d6eb-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="2d6eb-106">Installez le [Kit de développement logiciel (SDK) .net Core 3,1 preview](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="2d6eb-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="2d6eb-107">Installez le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) en exécutant la commande suivante dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="2d6eb-108">[Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Le package de modèles a une version préliminaire alors que Blazor Webassembly est en préversion.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="2d6eb-109">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d6eb-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d6eb-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="2d6eb-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-111">1\.</span></span> <span data-ttu-id="2d6eb-112">Installez [Visual Studio 16,4 Preview 2 ou version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="2d6eb-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="2d6eb-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-113">2\.</span></span> <span data-ttu-id="2d6eb-114">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-114">Create a new project.</span></span>

   <span data-ttu-id="2d6eb-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-115">3\.</span></span> <span data-ttu-id="2d6eb-116">Sélectionnez **Blazor application**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-116">Select **Blazor App**.</span></span> <span data-ttu-id="2d6eb-117">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-117">Select **Next**.</span></span>

   <span data-ttu-id="2d6eb-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-118">4\.</span></span> <span data-ttu-id="2d6eb-119">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2d6eb-120">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2d6eb-121">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-121">Select **Create**.</span></span>

   <span data-ttu-id="2d6eb-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-122">5\.</span></span> <span data-ttu-id="2d6eb-123">Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="2d6eb-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="2d6eb-124">Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="2d6eb-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="2d6eb-125">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-125">Select **Create**.</span></span> <span data-ttu-id="2d6eb-126">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2d6eb-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-127">6\.</span></span> <span data-ttu-id="2d6eb-128">Appuyez sur **Ctrl**+**F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-128">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2d6eb-129">Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="2d6eb-130">L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d6eb-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d6eb-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="2d6eb-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-132">1\.</span></span> <span data-ttu-id="2d6eb-133">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="2d6eb-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="2d6eb-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-134">2\.</span></span> <span data-ttu-id="2d6eb-135">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="2d6eb-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="2d6eb-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-136">3\.</span></span> <span data-ttu-id="2d6eb-137">Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="2d6eb-138">Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="2d6eb-139">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2d6eb-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-140">4\.</span></span> <span data-ttu-id="2d6eb-141">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="2d6eb-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-142">5\.</span></span> <span data-ttu-id="2d6eb-143">Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="2d6eb-144">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-144">Select **Yes**.</span></span>

   <span data-ttu-id="2d6eb-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-145">6\.</span></span> <span data-ttu-id="2d6eb-146">Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="2d6eb-147">Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="2d6eb-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-148">7\.</span></span> <span data-ttu-id="2d6eb-149">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-149">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2d6eb-150">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2d6eb-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="2d6eb-151">Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2d6eb-152">Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2d6eb-153">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2d6eb-154">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="2d6eb-155">Installez la dernière version du [Kit de développement logiciel (SDK) .net Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="2d6eb-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="2d6eb-156">Installez éventuellement le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) en installant le [Kit de développement logiciel (SDK) .net Core 3,1 preview](https://dotnet.microsoft.com/download/dotnet-core/3.1) , puis en exécutant la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview2.19528.8
   ```

1. <span data-ttu-id="2d6eb-157">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2d6eb-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2d6eb-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="2d6eb-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-159">1\.</span></span> <span data-ttu-id="2d6eb-160">Installez la dernière version de [Visual Studio](https://visualstudio.com/vs/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="2d6eb-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="2d6eb-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-161">2\.</span></span> <span data-ttu-id="2d6eb-162">Si vous le souhaitez, vous pouvez installer [Visual Studio 16,4 Preview 2 ou version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** pour Blazor développement d’applications webassembly.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="2d6eb-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-163">3\.</span></span> <span data-ttu-id="2d6eb-164">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-164">Create a new project.</span></span>

   <span data-ttu-id="2d6eb-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-165">4\.</span></span> <span data-ttu-id="2d6eb-166">Sélectionnez **Blazor application**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-166">Select **Blazor App**.</span></span> <span data-ttu-id="2d6eb-167">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-167">Select **Next**.</span></span>

   <span data-ttu-id="2d6eb-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-168">5\.</span></span> <span data-ttu-id="2d6eb-169">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2d6eb-170">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2d6eb-171">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-171">Select **Create**.</span></span>

   <span data-ttu-id="2d6eb-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-172">6\.</span></span> <span data-ttu-id="2d6eb-173">Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="2d6eb-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="2d6eb-174">Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="2d6eb-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="2d6eb-175">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-175">Select **Create**.</span></span> <span data-ttu-id="2d6eb-176">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2d6eb-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-177">7\.</span></span> <span data-ttu-id="2d6eb-178">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2d6eb-179">Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="2d6eb-180">L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2d6eb-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d6eb-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="2d6eb-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-182">1\.</span></span> <span data-ttu-id="2d6eb-183">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="2d6eb-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="2d6eb-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-184">2\.</span></span> <span data-ttu-id="2d6eb-185">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="2d6eb-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="2d6eb-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-186">3\.</span></span> <span data-ttu-id="2d6eb-187">Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="2d6eb-188">Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="2d6eb-189">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2d6eb-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-190">4\.</span></span> <span data-ttu-id="2d6eb-191">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="2d6eb-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-192">5\.</span></span> <span data-ttu-id="2d6eb-193">Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="2d6eb-194">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-194">Select **Yes**.</span></span>

   <span data-ttu-id="2d6eb-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-195">6\.</span></span> <span data-ttu-id="2d6eb-196">Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="2d6eb-197">Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="2d6eb-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-198">7\.</span></span> <span data-ttu-id="2d6eb-199">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-199">In a browser, navigate to `https://localhost:5001`.</span></span>

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2d6eb-200">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2d6eb-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="2d6eb-201">Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2d6eb-202">Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="2d6eb-203">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="2d6eb-204">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="2d6eb-205">Plusieurs pages sont disponibles à partir des onglets de la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="2d6eb-206">Début</span><span class="sxs-lookup"><span data-stu-id="2d6eb-206">Home</span></span>
* <span data-ttu-id="2d6eb-207">Counter</span><span class="sxs-lookup"><span data-stu-id="2d6eb-207">Counter</span></span>
* <span data-ttu-id="2d6eb-208">Extraire les données</span><span class="sxs-lookup"><span data-stu-id="2d6eb-208">Fetch data</span></span>

<span data-ttu-id="2d6eb-209">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="2d6eb-210">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais C#avec Blazor vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="2d6eb-211">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="2d6eb-212">Une demande de `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="2d6eb-213">Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="2d6eb-214">Chaque fois que le bouton **Click Me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="2d6eb-215">L’événement `onclick` est déclenché.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="2d6eb-216">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="2d6eb-217">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="2d6eb-218">Le composant est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-218">The component is rendered again.</span></span>

<span data-ttu-id="2d6eb-219">Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="2d6eb-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="2d6eb-220">Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="2d6eb-221">Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="2d6eb-222">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="2d6eb-223">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-223">Run the app.</span></span> <span data-ttu-id="2d6eb-224">La page d’accueil possède son propre compteur fourni par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="2d6eb-225">Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="2d6eb-226">Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="2d6eb-227">Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="2d6eb-228">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="2d6eb-229">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="2d6eb-230">Spécifiez le `IncrementAmount` dans l’élément `<Counter>` `Index` du composant à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="2d6eb-231">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="2d6eb-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="2d6eb-232">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-232">Run the app.</span></span> <span data-ttu-id="2d6eb-233">Le composant `Index` a son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="2d6eb-234">Le composant `Counter` (*Counter. Razor*) à `/counter` continue d’être incrémenté d’une unité.</span><span class="sxs-lookup"><span data-stu-id="2d6eb-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d6eb-235">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="2d6eb-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="2d6eb-236">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2d6eb-236">Additional resources</span></span>

* <xref:signalr/introduction>
