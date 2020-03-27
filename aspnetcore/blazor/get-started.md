---
title: Prise en main de ASP.NET Core Blazor
author: guardrex
description: Commencez avec Blazor en créant une application Blazor avec les outils de votre choix.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 9ebeb57d2fad7e4c288d61a46911f2bf64cac2fb
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320939"
---
# <a name="get-started-with-aspnet-core-blazor"></a><span data-ttu-id="d4531-103">Prise en main d’ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="d4531-103">Get started with ASP.NET Core Blazor</span></span>

<span data-ttu-id="d4531-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d4531-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="d4531-105">Pour vous familiariser avec éblouissant, suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="d4531-105">To get started with Blazor, follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d4531-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4531-106">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="d4531-107">Pour créer des applications de serveur éblouissantes, installez [Visual Studio 2019 version 16,4 ou ultérieure](https://visualstudio.microsoft.com/vs/preview/) avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="d4531-107">To create Blazor Server apps, install [Visual Studio 2019 version 16.4 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d4531-108">Pour créer des applications de serveur éblouissant et de webassembly éblouissant, installez Visual Studio 2019 16,6 Preview 2 ou version ultérieure avec la charge de travail **développement Web et ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="d4531-108">To create Blazor Server and Blazor WebAssembly apps, install Visual Studio 2019 16.6 Preview 2 or later with the **ASP.NET and web development** workload.</span></span>

   <span data-ttu-id="d4531-109">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *Webassembly éblouissant* et le *serveur éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4531-109">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="d4531-110">Créez un projet.</span><span class="sxs-lookup"><span data-stu-id="d4531-110">Create a new project.</span></span>

1. <span data-ttu-id="d4531-111">Sélectionnez l' **application éblouissant**.</span><span class="sxs-lookup"><span data-stu-id="d4531-111">Select **Blazor App**.</span></span> <span data-ttu-id="d4531-112">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="d4531-112">Select **Next**.</span></span>

1. <span data-ttu-id="d4531-113">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="d4531-113">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="d4531-114">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="d4531-114">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d4531-115">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="d4531-115">Select **Create**.</span></span>

1. <span data-ttu-id="d4531-116">Pour une expérience webassembly éblouissant (Visual Studio 16,6 Preview 2 ou version ultérieure), choisissez le modèle **application éblouissant Webassembly** .</span><span class="sxs-lookup"><span data-stu-id="d4531-116">For a Blazor WebAssembly experience (Visual Studio 16.6 Preview 2 or later), choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="d4531-117">Pour une expérience de serveur éblouissant (Visual Studio 16,4 ou version ultérieure), choisissez le modèle **application de serveur éblouissant** .</span><span class="sxs-lookup"><span data-stu-id="d4531-117">For a Blazor Server experience (Visual Studio 16.4 or later), choose the **Blazor Server App** template.</span></span> <span data-ttu-id="d4531-118">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="d4531-118">Select **Create**.</span></span>

1. <span data-ttu-id="d4531-119">Appuyez sur <kbd>Ctrl</kbd>+<kbd>F5</kbd> pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="d4531-119">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="d4531-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4531-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="d4531-121">Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d4531-121">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="d4531-122">Si vous le souhaitez, vous pouvez installer le modèle d’aperçu de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d4531-122">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="d4531-123">La [version de kit SDK .net Core 3.1.201 ou ultérieure](https://dotnet.microsoft.com/download/dotnet-core/3.1) est **requise** pour utiliser le modèle webassembly 3,2 Preview 3 éblouissant.</span><span class="sxs-lookup"><span data-stu-id="d4531-123">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="d4531-124">Confirmez la version de kit SDK .NET Core installée en exécutant `dotnet --version` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="d4531-124">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="d4531-125">Installez [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="d4531-125">Install [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="d4531-126">Installez la dernière [ C# extension pour Visual Studio code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) et l’extension de [débogueur JavaScript (nocturne)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) avec `debug.javascript.usePreview` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="d4531-126">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) and the [JavaScript Debugger (Nightly)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) extension with `debug.javascript.usePreview` set to `true`.</span></span>

1. <span data-ttu-id="d4531-127">Pour une expérience de serveur Blazor, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="d4531-127">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="d4531-128">Pour une expérience de webassembly Blazor, exécutez la commande suivante dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="d4531-128">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="d4531-129">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4531-129">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="d4531-130">Ouvrez le dossier *WebApplication1* dans Visual Studio code.</span><span class="sxs-lookup"><span data-stu-id="d4531-130">Open the *WebApplication1* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="d4531-131">L’IDE demande que vous ajoutiez des ressources pour générer et déboguer le projet.</span><span class="sxs-lookup"><span data-stu-id="d4531-131">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="d4531-132">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="d4531-132">Select **Yes**.</span></span>

1. <span data-ttu-id="d4531-133">Exécutez l’application à l’aide du débogueur Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d4531-133">Run the app using the Visual Studio Code debugger.</span></span>

1. <span data-ttu-id="d4531-134">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4531-134">In a browser, navigate to `https://localhost:5001`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="d4531-135">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="d4531-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d4531-136">Le serveur éblouissant est pris en charge dans Visual Studio pour Mac.</span><span class="sxs-lookup"><span data-stu-id="d4531-136">Blazor Server is supported in Visual Studio for Mac.</span></span> <span data-ttu-id="d4531-137">Le webassembly éblouissant n’est pas pris en charge pour le moment.</span><span class="sxs-lookup"><span data-stu-id="d4531-137">Blazor WebAssembly isn't supported at this time.</span></span> <span data-ttu-id="d4531-138">Pour générer des applications webassembly éblouissantes sur macOS, suivez les instructions de l’onglet **CLI .net Core** .</span><span class="sxs-lookup"><span data-stu-id="d4531-138">To build Blazor WebAssembly apps on macOS, follow the guidance on the **.NET Core CLI** tab.</span></span>

1. <span data-ttu-id="d4531-139">Installez [Visual Studio pour Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="d4531-139">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="d4531-140">Sélectionnez **fichier** > **nouvelle solution** ou créer un **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="d4531-140">Select **File** > **New Solution** or create a **New Project**.</span></span>

1. <span data-ttu-id="d4531-141">Dans la barre latérale, sélectionnez **.net Core** > **application**.</span><span class="sxs-lookup"><span data-stu-id="d4531-141">In the sidebar, select **.NET Core** > **App**.</span></span>

1. <span data-ttu-id="d4531-142">Sélectionnez le modèle **application de serveur éblouissant** .</span><span class="sxs-lookup"><span data-stu-id="d4531-142">Select the **Blazor Server App** template.</span></span> <span data-ttu-id="d4531-143">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="d4531-143">Select **Create**.</span></span>

   <span data-ttu-id="d4531-144">Pour plus d’informations sur le modèle d’hébergement de serveur éblouissant, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4531-144">For information on the Blazor Server hosting model, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="d4531-145">Définissez la version **cible** de **.NET Framework sur .net Core 3,1** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="d4531-145">Set the **Target Framework** to **.NET Core 3.1** and select **Next**.</span></span>

1. <span data-ttu-id="d4531-146">Dans le champ **nom du projet** , nommez l’application `WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="d4531-146">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="d4531-147">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="d4531-147">Select **Create**.</span></span>

1. <span data-ttu-id="d4531-148">Sélectionnez **exécuter** > **exécuter sans débogage** pour exécuter l’application *sans le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="d4531-148">Select **Run** > **Run Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="d4531-149">Exécutez l’application avec **Démarrer le débogage** pour exécuter l’application *avec le débogueur*.</span><span class="sxs-lookup"><span data-stu-id="d4531-149">Run the app with **Start Debugging** to run the app *with the debugger*.</span></span>

<span data-ttu-id="d4531-150">Si une invite s’affiche pour faire confiance au certificat de développement, approuvez le certificat et continuez.</span><span class="sxs-lookup"><span data-stu-id="d4531-150">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="d4531-151">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d4531-151">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="d4531-152">Installez le [Kit de développement logiciel (SDK) .net Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="d4531-152">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span>

1. <span data-ttu-id="d4531-153">Si vous le souhaitez, vous pouvez installer le modèle d’aperçu de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d4531-153">Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) preview template by running the following command:</span></span>

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > <span data-ttu-id="d4531-154">La [version de kit SDK .net Core 3.1.201 ou ultérieure](https://dotnet.microsoft.com/download/dotnet-core/3.1) est **requise** pour utiliser le modèle webassembly 3,2 Preview 3 éblouissant.</span><span class="sxs-lookup"><span data-stu-id="d4531-154">The [.NET Core SDK version 3.1.201 or later](https://dotnet.microsoft.com/download/dotnet-core/3.1) is **required** to use the 3.2 Preview 3 Blazor WebAssembly template.</span></span> <span data-ttu-id="d4531-155">Confirmez la version de kit SDK .NET Core installée en exécutant `dotnet --version` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="d4531-155">Confirm the installed .NET Core SDK version by running `dotnet --version` in a command shell.</span></span>

1. <span data-ttu-id="d4531-156">Pour une expérience de serveur Blazor, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="d4531-156">For a Blazor Server experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d4531-157">Pour une expérience de webassembly Blazor, exécutez les commandes suivantes dans une interface de commande :</span><span class="sxs-lookup"><span data-stu-id="d4531-157">For a Blazor WebAssembly experience, execute the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   <span data-ttu-id="d4531-158">Pour plus d’informations sur les deux modèles d’hébergement éblouissants, le *serveur éblouissant* et le *webassembly éblouissant*, consultez <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="d4531-158">For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="d4531-159">Dans un navigateur, accédez à `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="d4531-159">In a browser, navigate to `https://localhost:5001`.</span></span>

---

<span data-ttu-id="d4531-160">Plusieurs pages sont disponibles à partir des onglets de la barre latérale :</span><span class="sxs-lookup"><span data-stu-id="d4531-160">Multiple pages are available from tabs in the sidebar:</span></span>

* <span data-ttu-id="d4531-161">Accueil</span><span class="sxs-lookup"><span data-stu-id="d4531-161">Home</span></span>
* <span data-ttu-id="d4531-162">Compteur</span><span class="sxs-lookup"><span data-stu-id="d4531-162">Counter</span></span>
* <span data-ttu-id="d4531-163">Extraire les données</span><span class="sxs-lookup"><span data-stu-id="d4531-163">Fetch data</span></span>

<span data-ttu-id="d4531-164">Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="d4531-164">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="d4531-165">L’incrémentation d’un compteur dans une page Web nécessite normalement l’écriture de JavaScript, mais C#avec Blazor vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="d4531-165">Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.</span></span>

<span data-ttu-id="d4531-166">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="d4531-166">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

<span data-ttu-id="d4531-167">Une demande d' `/counter` dans le navigateur, comme spécifié par la directive `@page` en haut, entraîne le rendu du contenu par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d4531-167">A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content.</span></span> <span data-ttu-id="d4531-168">Les composants sont rendus dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisée pour mettre à jour l’interface utilisateur de manière flexible et efficace.</span><span class="sxs-lookup"><span data-stu-id="d4531-168">Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.</span></span>

<span data-ttu-id="d4531-169">Chaque fois que le bouton **Click Me** est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="d4531-169">Each time the **Click me** button is selected:</span></span>

* <span data-ttu-id="d4531-170">L’événement `onclick` est déclenché.</span><span class="sxs-lookup"><span data-stu-id="d4531-170">The `onclick` event is fired.</span></span>
* <span data-ttu-id="d4531-171">La méthode `IncrementCount` est appelée.</span><span class="sxs-lookup"><span data-stu-id="d4531-171">The `IncrementCount` method is called.</span></span>
* <span data-ttu-id="d4531-172">Le `currentCount` est incrémenté.</span><span class="sxs-lookup"><span data-stu-id="d4531-172">The `currentCount` is incremented.</span></span>
* <span data-ttu-id="d4531-173">Le composant est de nouveau restitué.</span><span class="sxs-lookup"><span data-stu-id="d4531-173">The component is rendered again.</span></span>

<span data-ttu-id="d4531-174">Le runtime compare le nouveau contenu au contenu précédent et applique uniquement le contenu modifié à l’Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="d4531-174">The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).</span></span>

<span data-ttu-id="d4531-175">Ajoutez un composant à un autre composant à l’aide de la syntaxe HTML.</span><span class="sxs-lookup"><span data-stu-id="d4531-175">Add a component to another component using HTML syntax.</span></span> <span data-ttu-id="d4531-176">Par exemple, ajoutez le composant `Counter` à la page d’accueil de l’application en ajoutant un élément `<Counter />` au composant `Index`.</span><span class="sxs-lookup"><span data-stu-id="d4531-176">For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.</span></span>

<span data-ttu-id="d4531-177">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="d4531-177">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

<span data-ttu-id="d4531-178">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="d4531-178">Run the app.</span></span> <span data-ttu-id="d4531-179">La page d’accueil possède son propre compteur fourni par le composant `Counter`.</span><span class="sxs-lookup"><span data-stu-id="d4531-179">The homepage has its own counter provided by the `Counter` component.</span></span>

<span data-ttu-id="d4531-180">Les paramètres de composant sont spécifiés à l’aide d’attributs ou de [contenu enfant](xref:blazor/components#child-content), ce qui vous permet de définir des propriétés sur le composant enfant.</span><span class="sxs-lookup"><span data-stu-id="d4531-180">Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component.</span></span> <span data-ttu-id="d4531-181">Pour ajouter un paramètre au composant `Counter`, mettez à jour le bloc `@code` du composant :</span><span class="sxs-lookup"><span data-stu-id="d4531-181">To add a parameter to the `Counter` component, update the component's `@code` block:</span></span>

* <span data-ttu-id="d4531-182">Ajoutez une propriété publique pour `IncrementAmount` avec un attribut `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="d4531-182">Add a public property for `IncrementAmount` with a `[Parameter]` attribute.</span></span>
* <span data-ttu-id="d4531-183">Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="d4531-183">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

<span data-ttu-id="d4531-184">*Pages/Counter.razor* :</span><span class="sxs-lookup"><span data-stu-id="d4531-184">*Pages/Counter.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

<span data-ttu-id="d4531-185">Spécifiez le `IncrementAmount` dans l’élément `<Counter>` du composant `Index` à l’aide d’un attribut.</span><span class="sxs-lookup"><span data-stu-id="d4531-185">Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.</span></span>

<span data-ttu-id="d4531-186">*Pages/Index.razor* :</span><span class="sxs-lookup"><span data-stu-id="d4531-186">*Pages/Index.razor*:</span></span>

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

<span data-ttu-id="d4531-187">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="d4531-187">Run the app.</span></span> <span data-ttu-id="d4531-188">Le composant `Index` possède son propre compteur qui est incrémenté de dix chaque fois que le bouton **Click Me** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d4531-188">The `Index` component has its own counter that increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="d4531-189">Le composant `Counter` (*Counter. Razor*) sur `/counter` continue d’être incrémenté d’une unité.</span><span class="sxs-lookup"><span data-stu-id="d4531-189">The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4531-190">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d4531-190">Next steps</span></span>

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a><span data-ttu-id="d4531-191">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d4531-191">Additional resources</span></span>

* <xref:blazor/templates>
* <xref:signalr/introduction>
