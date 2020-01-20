---
title: Prise en main de ASP.NET Core Blazor
author: guardrex
description: Commencez avec Blazor en créant une application Blazor avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 09400a076849bdec35beb284a488d01feb8a84c2
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160000"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a><span data-ttu-id="6bab0-103">Prise en main de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="6bab0-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="6bab0-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6bab0-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="6bab0-105">Prise en main de Blazor:</span><span class="sxs-lookup"><span data-stu-id="6bab0-105">Get started with Blazor:</span></span>

1. <span data-ttu-id="6bab0-106">Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="6bab0-106">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="6bab0-107">Installez éventuellement le modèle [Blazor Webassembly](xref:blazor/hosting-models#blazor-webassembly) :</span><span class="sxs-lookup"><span data-stu-id="6bab0-107">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template:</span></span>
   * <span data-ttu-id="6bab0-108">Installez le [Kit de développement logiciel (SDK) .net Core 3,1 ou ultérieur (version préliminaire)](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="6bab0-108">Install the [.NET Core 3.1 or later (Preview) SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>
   * <span data-ttu-id="6bab0-109">Exécutez la commande suivante dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="6bab0-109">Run the following command in a command shell.</span></span> <span data-ttu-id="6bab0-110">[Microsoft. AspNetCore.Blazor. ](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/)Le package de modèles a une version préliminaire alors que Blazor Webassembly est en préversion.</span><span class="sxs-lookup"><span data-stu-id="6bab0-110">The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. <span data-ttu-id="6bab0-111">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="6bab0-111">Follow the guidance for your choice of tooling:</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6bab0-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6bab0-112">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="6bab0-113">1 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-113">1\.</span></span> <span data-ttu-id="6bab0-114">Installez [Visual Studio 16,4 ou une version ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="6bab0-114">Install [Visual Studio 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="6bab0-115">2 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-115">2\.</span></span> <span data-ttu-id="6bab0-116">Créez un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="6bab0-116">Create a new project.</span></span>

   <span data-ttu-id="6bab0-117">3 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-117">3\.</span></span> <span data-ttu-id="6bab0-118">Sélectionnez **Blazor application**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-118">Select **Blazor App**.</span></span> <span data-ttu-id="6bab0-119">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-119">Select **Next**.</span></span>

   <span data-ttu-id="6bab0-120">4 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-120">4\.</span></span> <span data-ttu-id="6bab0-121">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="6bab0-121">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="6bab0-122">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="6bab0-122">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="6bab0-123">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-123">Select **Create**.</span></span>

   <span data-ttu-id="6bab0-124">5 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-124">5\.</span></span> <span data-ttu-id="6bab0-125">Pour une expérience Blazor webassembly, choisissez le modèle **application WebassemblyBlazor** .</span><span class="sxs-lookup"><span data-stu-id="6bab0-125">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="6bab0-126">Pour une expérience Blazor Server, choisissez le modèle d' **applicationBlazor Server** .</span><span class="sxs-lookup"><span data-stu-id="6bab0-126">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="6bab0-127">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-127">Select **Create**.</span></span> <span data-ttu-id="6bab0-128">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bab0-128">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bab0-129">6 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-129">6\.</span></span> <span data-ttu-id="6bab0-130">Appuyez sur **Ctrl**+**F5** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="6bab0-130">Press **Ctrl**+**F5** to run the app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6bab0-131">Si vous avez installé le Blazor extension Visual Studio pour une version préliminaire antérieure de ASP.NET Core Blazor (version préliminaire 6 ou antérieure), vous pouvez désinstaller l’extension.</span><span class="sxs-lookup"><span data-stu-id="6bab0-131">If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension.</span></span> <span data-ttu-id="6bab0-132">L’installation des modèles de Blazor dans un interpréteur de commandes est désormais suffisante pour exposer les modèles dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6bab0-132">Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.</span></span>

   # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6bab0-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6bab0-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

   <span data-ttu-id="6bab0-134">1 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-134">1\.</span></span> <span data-ttu-id="6bab0-135">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6bab0-135">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

   <span data-ttu-id="6bab0-136">2 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-136">2\.</span></span> <span data-ttu-id="6bab0-137">Installez le dernier [ C# Visual Studio code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span><span class="sxs-lookup"><span data-stu-id="6bab0-137">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).</span></span>

   <span data-ttu-id="6bab0-138">3 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-138">3\.</span></span> <span data-ttu-id="6bab0-139">Pour une expérience Blazor webassembly, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="6bab0-139">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      <span data-ttu-id="6bab0-140">Pour une expérience Blazor Server, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="6bab0-140">For a Blazor Server experience, execute the following command in a command shell:</span></span>

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      <span data-ttu-id="6bab0-141">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bab0-141">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bab0-142">4 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-142">4\.</span></span> <span data-ttu-id="6bab0-143">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="6bab0-143">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

   <span data-ttu-id="6bab0-144">5 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-144">5\.</span></span> <span data-ttu-id="6bab0-145">Pour un projet Blazor Server, l’IDE demande que vous ajoutiez des éléments multimédias pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="6bab0-145">For a Blazor Server project, the IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="6bab0-146">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-146">Select **Yes**.</span></span>

   <span data-ttu-id="6bab0-147">6 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-147">6\.</span></span> <span data-ttu-id="6bab0-148">Si vous utilisez une application Blazor Server, exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6bab0-148">If using a Blazor Server app, run the app using the Visual Studio Code debugger.</span></span> <span data-ttu-id="6bab0-149">Si vous utilisez une application Blazor webassembly, exécutez `dotnet run` à partir du dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="6bab0-149">If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.</span></span>

   <span data-ttu-id="6bab0-150">7 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-150">7\.</span></span> <span data-ttu-id="6bab0-151">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-151">In a browser, navigate to `https://localhost:5001`.</span></span>

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6bab0-152">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6bab0-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

   <span data-ttu-id="6bab0-153">1 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-153">1\.</span></span> <span data-ttu-id="6bab0-154">Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="6bab0-154">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

   <span data-ttu-id="6bab0-155">2 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-155">2\.</span></span> <span data-ttu-id="6bab0-156">Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-156">Select **File** > **New Solution** or create a **New Project**.</span></span>

   <span data-ttu-id="6bab0-157">3 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-157">3\.</span></span> <span data-ttu-id="6bab0-158">Dans la barre latérale, sélectionnez **.net Core** > **application**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-158">In the sidebar, select **.NET Core** > **App**.</span></span>

   <span data-ttu-id="6bab0-159">4 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-159">4\.</span></span> <span data-ttu-id="6bab0-160">Sélectionnez le modèle **application serveurBlazor** .</span><span class="sxs-lookup"><span data-stu-id="6bab0-160">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="6bab0-161">Seul le modèle Blazor Server est disponible dans Visual Studio pour Mac pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="6bab0-161">Only the Blazor Server template is available in Visual Studio for Mac at this time.</span></span> <span data-ttu-id="6bab0-162">Pour une expérience Blazor webassembly, suivez les instructions de l’onglet **CLI .net Core** . Après avoir sélectionné le modèle Blazor Server, sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-162">For a Blazor WebAssembly experience, follow the instructions on the **.NET Core CLI** tab. After selecting the Blazor Server template, select **Next**.</span></span> <span data-ttu-id="6bab0-163">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bab0-163">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   <span data-ttu-id="6bab0-164">5 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-164">5\.</span></span> <span data-ttu-id="6bab0-165">Définissez la version **cible** de **.NET Framework sur .net Core 3,1** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-165">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

   <span data-ttu-id="6bab0-166">6 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-166">6\.</span></span> <span data-ttu-id="6bab0-167">Dans le champ **nom du projet** , nommez l’application `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-167">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="6bab0-168">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6bab0-168">Select **Create**.</span></span>

   <span data-ttu-id="6bab0-169">7 \.</span><span class="sxs-lookup"><span data-stu-id="6bab0-169">7\.</span></span> <span data-ttu-id="6bab0-170">Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="6bab0-170">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="6bab0-171">Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="6bab0-171">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

   <span data-ttu-id="6bab0-172">Si une invite s’affiche pour faire confiance au certificat de développement, approuvez le certificat et continuez.</span><span class="sxs-lookup"><span data-stu-id="6bab0-172">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6bab0-173">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="6bab0-173">.NET Core CLI</span></span>](#tab/netcore-cli/)

   <span data-ttu-id="6bab0-174">Pour une expérience Blazor webassembly, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="6bab0-174">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6bab0-175">Pour une expérience Blazor Server, exécutez les commandes suivantes dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="6bab0-175">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="6bab0-176">Pour plus d’informations sur les deux modèles d’hébergement Blazor, *Blazor Server* et *Blazor webassembly*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="6bab0-176">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

   <span data-ttu-id="6bab0-177">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-177">In a browser, navigate to `https://localhost:5001`.</span></span>

   ---

<span data-ttu-id="6bab0-178">Plusieurs pages sont disponibles à partir des onglets de la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="6bab0-178">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="6bab0-179">Accueil</span><span class="sxs-lookup"><span data-stu-id="6bab0-179">Home</span></span>
* <span data-ttu-id="6bab0-180">Counter</span><span class="sxs-lookup"><span data-stu-id="6bab0-180">Counter</span></span>
* <span data-ttu-id="6bab0-181">Extraire les données</span><span class="sxs-lookup"><span data-stu-id="6bab0-181">Fetch data</span></span>

<span data-ttu-id="6bab0-182">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="6bab0-182">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="6bab0-183">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais C#avec Blazor vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="6bab0-183">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="6bab0-184">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="6bab0-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="6bab0-185">Une demande d' `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-185">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="6bab0-186">Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="6bab0-186">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="6bab0-187">Chaque fois que le bouton **Click Me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="6bab0-187">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="6bab0-188">L’événement `onclick` est déclenché.</span><span class="sxs-lookup"><span data-stu-id="6bab0-188">The `onclick` event is fired.</span></span>
* <span data-ttu-id="6bab0-189">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="6bab0-189">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="6bab0-190">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="6bab0-190">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="6bab0-191">Le composant est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="6bab0-191">The component is rendered again.</span></span>

<span data-ttu-id="6bab0-192">Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="6bab0-192">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="6bab0-193">Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="6bab0-193">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="6bab0-194">Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-194">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="6bab0-195">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="6bab0-195">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="6bab0-196">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="6bab0-196">Run the app.</span></span> <span data-ttu-id="6bab0-197">La page d’accueil possède son propre compteur fourni par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-197">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="6bab0-198">Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="6bab0-198">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="6bab0-199">Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="6bab0-199">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="6bab0-200">Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-200">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="6bab0-201">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="6bab0-201">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="6bab0-202">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="6bab0-202">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="6bab0-203">Spécifiez le `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="6bab0-203">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="6bab0-204">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="6bab0-204">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="6bab0-205">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="6bab0-205">Run the app.</span></span> <span data-ttu-id="6bab0-206">Le composant `Index` possède son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="6bab0-206">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="6bab0-207">Le composant `Counter` (*Counter. Razor*) sur `/counter` continue d’être incrémenté d’une unité.</span><span class="sxs-lookup"><span data-stu-id="6bab0-207">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bab0-208">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6bab0-208">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="6bab0-209">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6bab0-209">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
