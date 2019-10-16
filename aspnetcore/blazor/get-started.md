---
title: Prise en main d’ASP.NET Core éblouissante
author: guardrex
description: Commencez avec éblouissant en créant une application éblouissant avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/get-started
ms.openlocfilehash: fc368be5eb2e5d8f7c80071dc86a02ae986a685f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391055"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="91f29-103">Prise en main d’ASP.NET Core éblouissante</span><span class="sxs-lookup"><span data-stu-id="91f29-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="91f29-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="91f29-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="91f29-105">Prise en main de éblouissant :</span><span class="sxs-lookup"><span data-stu-id="91f29-105">Get started with Blazor:</span></span>

::: moniker range=">= aspnetcore-3.1"

1. <span data-ttu-id="91f29-106">Installez le [Kit de développement logiciel (SDK) .net Core 3,1 preview](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="91f29-106">Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="91f29-107">Installez le modèle de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) en exécutant la commande suivante dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="91f29-107">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell.</span></span> <span data-ttu-id="91f29-108">Le package [Microsoft. AspNetCore. éblouissant. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) a une préversion alors que l’assembly éblouissant est en version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="91f29-108">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="91f29-109">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="91f29-109">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91f29-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91f29-110">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="91f29-111">1 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-111">1\.</span></span> <span data-ttu-id="91f29-112">Installez [Visual Studio 16,4 Preview 2 ou version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="91f29-112">Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="91f29-113">2 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-113">2\.</span></span> <span data-ttu-id="91f29-114">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="91f29-114">Create a new project.</span></span>

   <span data-ttu-id="91f29-115">3 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-115">3\.</span></span> <span data-ttu-id="91f29-116">Sélectionnez l' **application éblouissant**.</span><span class="sxs-lookup"><span data-stu-id="91f29-116">Select **Blazor App**.</span></span> <span data-ttu-id="91f29-117">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="91f29-117">Select **Next**.</span></span>

   <span data-ttu-id="91f29-118">4 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-118">4\.</span></span> <span data-ttu-id="91f29-119">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="91f29-119">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="91f29-120">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="91f29-120">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="91f29-121">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="91f29-121">Select **Create**.</span></span>

   <span data-ttu-id="91f29-122">5 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-122">5\.</span></span> <span data-ttu-id="91f29-123">Pour une expérience de webassembly éblouissant, choisissez le modèle **application éblouissant Webassembly** .</span><span class="sxs-lookup"><span data-stu-id="91f29-123">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="91f29-124">Pour une expérience de serveur éblouissant, choisissez le modèle **application de serveur éblouissant** .</span><span class="sxs-lookup"><span data-stu-id="91f29-124">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="91f29-125">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="91f29-125">Select **Create**.</span></span> <span data-ttu-id="91f29-126">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="91f29-126">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="91f29-127">6 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-127">6\.</span></span> <span data-ttu-id="91f29-128">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="91f29-128">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="91f29-129">Si vous avez installé l’extension Visual Studio éblouissant pour une version préliminaire antérieure de ASP.NET Core éblouissant (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="91f29-129">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="91f29-130">L’installation des modèles éblouissants dans un interpréteur de commandes est désormais suffisante pour faire apparaître les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="91f29-130">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="91f29-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91f29-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="91f29-132">1 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-132">1\.</span></span> <span data-ttu-id="91f29-133">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="91f29-133">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="91f29-134">2 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-134">2\.</span></span> <span data-ttu-id="91f29-135">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="91f29-135">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="91f29-136">3 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-136">3\.</span></span> <span data-ttu-id="91f29-137">Pour une expérience de webassembly éblouissant, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-137">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="91f29-138">Pour une expérience de serveur éblouissant, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-138">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="91f29-139">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="91f29-139">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="91f29-140">4 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-140">4\.</span></span> <span data-ttu-id="91f29-141">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="91f29-141">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="91f29-142">5 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-142">5\.</span></span> <span data-ttu-id="91f29-143">Pour un projet de serveur éblouissant, l’IDE demande que vous ajoutiez des ressources pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="91f29-143">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="91f29-144">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="91f29-144">Select **Yes**.</span></span>

   <span data-ttu-id="91f29-145">6 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-145">6\.</span></span> <span data-ttu-id="91f29-146">Si vous utilisez une application de serveur éblouissant, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="91f29-146">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="91f29-147">Si vous utilisez une application de webassembly éblouissante, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="91f29-147">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="91f29-148">7 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-148">7\.</span></span> <span data-ttu-id="91f29-149">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="91f29-149">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="91f29-150">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="91f29-150">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="91f29-151">Pour une expérience de webassembly éblouissant, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-151">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="91f29-152">Pour une expérience de serveur éblouissant, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-152">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="91f29-153">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="91f29-153">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="91f29-154">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="91f29-154">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. <span data-ttu-id="91f29-155">Installez la dernière version du [Kit de développement logiciel (SDK) .net Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .</span><span class="sxs-lookup"><span data-stu-id="91f29-155">Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.</span></span>

1. <span data-ttu-id="91f29-156">Installez éventuellement le modèle de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) en installant le [Kit de développement logiciel (SDK) .net Core 3,1 preview](https://dotnet.microsoft.com/download/dotnet-core/3.1) , puis en exécutant la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-156">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview1.19508.20
   ```

1. <span data-ttu-id="91f29-157">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="91f29-157">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="91f29-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91f29-158">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="91f29-159">1 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-159">1\.</span></span> <span data-ttu-id="91f29-160">Installez la dernière version de [Visual Studio](https://visualstudio.com/vs/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="91f29-160">Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="91f29-161">2 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-161">2\.</span></span> <span data-ttu-id="91f29-162">Si vous le souhaitez, vous pouvez installer [Visual Studio 16,4 Preview 2 ou version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** pour le développement d’applications éblouissantes webassembly.</span><span class="sxs-lookup"><span data-stu-id="91f29-162">Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.</span></span>

   <span data-ttu-id="91f29-163">3 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-163">3\.</span></span> <span data-ttu-id="91f29-164">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="91f29-164">Create a new project.</span></span>

   <span data-ttu-id="91f29-165">4 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-165">4\.</span></span> <span data-ttu-id="91f29-166">Sélectionnez l' **application éblouissant**.</span><span class="sxs-lookup"><span data-stu-id="91f29-166">Select **Blazor App**.</span></span> <span data-ttu-id="91f29-167">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="91f29-167">Select **Next**.</span></span>

   <span data-ttu-id="91f29-168">5 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-168">5\.</span></span> <span data-ttu-id="91f29-169">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="91f29-169">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="91f29-170">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="91f29-170">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="91f29-171">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="91f29-171">Select **Create**.</span></span>

   <span data-ttu-id="91f29-172">6 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-172">6\.</span></span> <span data-ttu-id="91f29-173">Pour une expérience de webassembly éblouissant, choisissez le modèle **application éblouissant Webassembly** .</span><span class="sxs-lookup"><span data-stu-id="91f29-173">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="91f29-174">Pour une expérience de serveur éblouissant, choisissez le modèle **application de serveur éblouissant** .</span><span class="sxs-lookup"><span data-stu-id="91f29-174">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="91f29-175">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="91f29-175">Select **Create**.</span></span> <span data-ttu-id="91f29-176">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="91f29-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="91f29-177">7 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-177">7\.</span></span> <span data-ttu-id="91f29-178">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="91f29-178">Press **F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="91f29-179">Si vous avez installé l’extension Visual Studio éblouissant pour une version préliminaire antérieure de ASP.NET Core éblouissant (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="91f29-179">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="91f29-180">L’installation des modèles éblouissants dans un interpréteur de commandes est désormais suffisante pour faire apparaître les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="91f29-180">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="91f29-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="91f29-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="91f29-182">1 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-182">1\.</span></span> <span data-ttu-id="91f29-183">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="91f29-183">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="91f29-184">2 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-184">2\.</span></span> <span data-ttu-id="91f29-185">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="91f29-185">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="91f29-186">3 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-186">3\.</span></span> <span data-ttu-id="91f29-187">Pour une expérience de webassembly éblouissant, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-187">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="91f29-188">Pour une expérience de serveur éblouissant, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-188">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="91f29-189">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="91f29-189">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="91f29-190">4 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-190">4\.</span></span> <span data-ttu-id="91f29-191">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="91f29-191">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="91f29-192">5 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-192">5\.</span></span> <span data-ttu-id="91f29-193">Pour un projet de serveur éblouissant, l’IDE demande que vous ajoutiez des ressources pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="91f29-193">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="91f29-194">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="91f29-194">Select **Yes**.</span></span>

   <span data-ttu-id="91f29-195">6 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-195">6\.</span></span> <span data-ttu-id="91f29-196">Si vous utilisez une application de serveur éblouissant, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="91f29-196">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="91f29-197">Si vous utilisez une application de webassembly éblouissante, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="91f29-197">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="91f29-198">7 \.</span><span class="sxs-lookup"><span data-stu-id="91f29-198">7\.</span></span> <span data-ttu-id="91f29-199">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="91f29-199">In a browser, navigate to `https://localhost:5001`.</span></span>

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

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="91f29-200">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="91f29-200">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="91f29-201">Pour une expérience de webassembly éblouissant, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-201">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="91f29-202">Pour une expérience de serveur éblouissant, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="91f29-202">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="91f29-203">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="91f29-203">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="91f29-204">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="91f29-204">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

::: moniker-end

<span data-ttu-id="91f29-205">Plusieurs pages sont disponibles à partir des onglets de la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="91f29-205">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="91f29-206">Début</span><span class="sxs-lookup"><span data-stu-id="91f29-206">Home</span></span>
* <span data-ttu-id="91f29-207">Counter</span><span class="sxs-lookup"><span data-stu-id="91f29-207">Counter</span></span>
* <span data-ttu-id="91f29-208">Extraire les données</span><span class="sxs-lookup"><span data-stu-id="91f29-208">Fetch data</span></span>

<span data-ttu-id="91f29-209">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="91f29-209">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="91f29-210">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais avec C#un éblouissant que vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="91f29-210">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="91f29-211">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="91f29-211">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="91f29-212">Une demande de `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="91f29-212">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="91f29-213">Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="91f29-213">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="91f29-214">Chaque fois que le bouton **Click Me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="91f29-214">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="91f29-215">L’événement `onclick` est déclenché.</span><span class="sxs-lookup"><span data-stu-id="91f29-215">The `onclick` event is fired.</span></span>
* <span data-ttu-id="91f29-216">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="91f29-216">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="91f29-217">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="91f29-217">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="91f29-218">Le composant est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="91f29-218">The component is rendered again.</span></span>

<span data-ttu-id="91f29-219">Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="91f29-219">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="91f29-220">Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="91f29-220">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="91f29-221">Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="91f29-221">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="91f29-222">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="91f29-222">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="91f29-223">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="91f29-223">Run the app.</span></span> <span data-ttu-id="91f29-224">La page d’accueil possède son propre compteur fourni par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="91f29-224">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="91f29-225">Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="91f29-225">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="91f29-226">Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="91f29-226">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="91f29-227">Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="91f29-227">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="91f29-228">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="91f29-228">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="91f29-229">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="91f29-229">*Pages/Counter.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="91f29-230">Spécifiez le `IncrementAmount` dans l’élément `<Counter>` `Index` du composant à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="91f29-230">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="91f29-231">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="91f29-231">*Pages/Index.razor*:</span></span>

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="91f29-232">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="91f29-232">Run the app.</span></span> <span data-ttu-id="91f29-233">Le composant `Index` a son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="91f29-233">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="91f29-234">Le composant `Counter` (*Counter. Razor*) à `/counter` continue d’être incrémenté d’une unité.</span><span class="sxs-lookup"><span data-stu-id="91f29-234">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91f29-235">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="91f29-235">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="91f29-236">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="91f29-236">Additional resources</span></span>

* <xref:signalr/introduction>
