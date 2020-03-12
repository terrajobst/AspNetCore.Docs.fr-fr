---
title: Utiliser ASP.NET Core SignalR avec Blazor webassembly
author: guardrex
description: Créez une application de conversation qui utilise ASP.NET Core SignalR avec Blazor webassembly.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/31/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: 605cf8ebd3e85586f3e479c815f0b9902ce5a91a
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083379"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="2663a-103">Utiliser ASP.NET Core Signalr avec le webassembly éblouissant</span><span class="sxs-lookup"><span data-stu-id="2663a-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="2663a-104">Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2663a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2663a-105">Ce didacticiel enseigne les bases de la création d’une application en temps réel à l’aide de Signalr avec le webassembly éblouissant.</span><span class="sxs-lookup"><span data-stu-id="2663a-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="2663a-106">Vous allez apprendre à effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="2663a-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2663a-107">Créer un projet d’application hébergée webassembly éblouissant</span><span class="sxs-lookup"><span data-stu-id="2663a-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="2663a-108">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="2663a-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="2663a-109">Ajouter un concentrateur Signalr</span><span class="sxs-lookup"><span data-stu-id="2663a-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="2663a-110">Ajouter des services Signalr et un point de terminaison pour le concentrateur Signalr</span><span class="sxs-lookup"><span data-stu-id="2663a-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="2663a-111">Ajouter du code de composant Razor pour la conversation</span><span class="sxs-lookup"><span data-stu-id="2663a-111">Add Razor component code for chat</span></span>

<span data-ttu-id="2663a-112">À la fin de ce didacticiel, vous disposerez d’une application de conversation de travail.</span><span class="sxs-lookup"><span data-stu-id="2663a-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="2663a-113">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2663a-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2663a-114">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="2663a-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2663a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2663a-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="2663a-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2663a-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2663a-117">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2663a-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="2663a-118">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2663a-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="2663a-119">Créer un projet d’application webassembly éblouissant hébergé</span><span class="sxs-lookup"><span data-stu-id="2663a-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="2663a-120">Installez le modèle de [Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly) .</span><span class="sxs-lookup"><span data-stu-id="2663a-120">Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="2663a-121">Le package [Microsoft. AspNetCore. Components. Webassembly. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) a une préversion alors que le composant webassembly éblouissant est en version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="2663a-121">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="2663a-122">Dans une interface de commande, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2663a-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
```

<span data-ttu-id="2663a-123">Suivez les instructions de votre choix d’outils :</span><span class="sxs-lookup"><span data-stu-id="2663a-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2663a-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2663a-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2663a-125">Créez un projet.</span><span class="sxs-lookup"><span data-stu-id="2663a-125">Create a new project.</span></span>

1. <span data-ttu-id="2663a-126">Sélectionnez l' **application éblouissant** , puis sélectionnez **suivant**.</span><span class="sxs-lookup"><span data-stu-id="2663a-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="2663a-127">Tapez « BlazorSignalRApp » dans le champ **nom du projet** .</span><span class="sxs-lookup"><span data-stu-id="2663a-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="2663a-128">Confirmez que l’entrée d' **emplacement** est correcte ou indiquez un emplacement pour le projet.</span><span class="sxs-lookup"><span data-stu-id="2663a-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2663a-129">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="2663a-129">Select **Create**.</span></span>

1. <span data-ttu-id="2663a-130">Choisissez le modèle **application éblouissant Webassembly** .</span><span class="sxs-lookup"><span data-stu-id="2663a-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="2663a-131">Sous **avancé**, activez la case à cocher **ASP.net Core hébergé** .</span><span class="sxs-lookup"><span data-stu-id="2663a-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="2663a-132">Sélectionnez **Create** (Créer).</span><span class="sxs-lookup"><span data-stu-id="2663a-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="2663a-133">Si vous avez mis à niveau ou installé une nouvelle version de Visual Studio et que le modèle de webassembly éblouissant n’apparaît pas dans l’interface utilisateur VS, réinstallez le modèle à l’aide de la commande `dotnet new` présentée précédemment.</span><span class="sxs-lookup"><span data-stu-id="2663a-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="2663a-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2663a-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2663a-135">Dans une interface de commande, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2663a-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="2663a-136">Dans Visual Studio Code, ouvrez le dossier du projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="2663a-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="2663a-137">Quand la boîte de dialogue semble ajouter des éléments multimédias pour générer et déboguer l’application, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="2663a-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="2663a-138">Visual Studio Code ajoute automatiquement le dossier *. vscode* avec les fichiers *Launch. JSON* et *Tasks. JSON* générés.</span><span class="sxs-lookup"><span data-stu-id="2663a-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2663a-139">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2663a-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2663a-140">Dans une interface de commande, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2663a-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="2663a-141">Dans Visual Studio pour Mac, ouvrez le projet en accédant au dossier du projet et en ouvrant le fichier solution du projet ( *. sln*).</span><span class="sxs-lookup"><span data-stu-id="2663a-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2663a-142">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2663a-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="2663a-143">Dans une interface de commande, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2663a-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="2663a-144">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="2663a-144">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2663a-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2663a-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="2663a-146">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **BlazorSignalRApp. client** , puis sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2663a-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="2663a-147">Dans la boîte de dialogue **gérer les packages NuGet** , vérifiez que la **source du package** est définie sur *NuGet.org*.</span><span class="sxs-lookup"><span data-stu-id="2663a-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="2663a-148">Avec l’option **Parcourir** sélectionnée, tapez « Microsoft. AspNetCore. signalr. client » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="2663a-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="2663a-149">Dans les résultats de la recherche, sélectionnez le package `Microsoft.AspNetCore.SignalR.Client`, puis sélectionnez **installer**.</span><span class="sxs-lookup"><span data-stu-id="2663a-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="2663a-150">Si la boîte de dialogue **aperçu des modifications** s’affiche, sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="2663a-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="2663a-151">Si la boîte de dialogue acceptation de la **licence** s’affiche, sélectionnez **J’accepte** si vous acceptez les termes du contrat de licence.</span><span class="sxs-lookup"><span data-stu-id="2663a-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="2663a-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2663a-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="2663a-153">Dans le **Terminal intégré** (**Afficher** > **Terminal** à partir de la barre d’outils), exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="2663a-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2663a-154">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2663a-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2663a-155">Dans la barre latérale **solution** , cliquez avec le bouton droit sur le projet **BlazorSignalRApp. client** , puis sélectionnez **gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2663a-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="2663a-156">Dans la boîte de dialogue **gérer les packages NuGet** , vérifiez que la liste déroulante source est définie sur *NuGet.org*.</span><span class="sxs-lookup"><span data-stu-id="2663a-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="2663a-157">Avec l’option **Parcourir** sélectionnée, tapez « Microsoft. AspNetCore. signalr. client » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="2663a-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="2663a-158">Dans les résultats de la recherche, activez la case à cocher située en regard du package `Microsoft.AspNetCore.SignalR.Client`, puis sélectionnez **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="2663a-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="2663a-159">Si la boîte de dialogue acceptation de la **licence** s’affiche, sélectionnez **accepter** si vous acceptez les termes du contrat de licence.</span><span class="sxs-lookup"><span data-stu-id="2663a-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2663a-160">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2663a-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="2663a-161">Dans une interface de commande, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="2663a-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="2663a-162">Ajouter un concentrateur Signalr</span><span class="sxs-lookup"><span data-stu-id="2663a-162">Add a SignalR hub</span></span>

<span data-ttu-id="2663a-163">Dans le projet **BlazorSignalRApp. Server** , créez un dossier *hubs* (plural) et ajoutez la classe de `ChatHub` suivante (*hubs/ChatHub. cs*) :</span><span class="sxs-lookup"><span data-stu-id="2663a-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="2663a-164">Ajouter des services Signalr et un point de terminaison pour le concentrateur Signalr</span><span class="sxs-lookup"><span data-stu-id="2663a-164">Add SignalR services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="2663a-165">Dans le projet **BlazorSignalRApp. Server** , ouvrez le fichier *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="2663a-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="2663a-166">Ajoutez l’espace de noms de la classe `ChatHub` au début du fichier :</span><span class="sxs-lookup"><span data-stu-id="2663a-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="2663a-167">Ajoutez les services Signalr à `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2663a-167">Add the SignalR services to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddSignalR();
   ```

1. <span data-ttu-id="2663a-168">Dans `Startup.Configure` entre les points de terminaison pour l’itinéraire du contrôleur par défaut et le secours côté client, ajoutez un point de terminaison pour le Hub :</span><span class="sxs-lookup"><span data-stu-id="2663a-168">In `Startup.Configure` between the endpoints for the default controller route and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="2663a-169">Ajouter du code de composant Razor pour la conversation</span><span class="sxs-lookup"><span data-stu-id="2663a-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="2663a-170">Dans le projet **BlazorSignalRApp. client** , ouvrez le fichier *pages/index. Razor* .</span><span class="sxs-lookup"><span data-stu-id="2663a-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="2663a-171">Remplacez le balisage par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2663a-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="2663a-172">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="2663a-172">Run the app</span></span>

1. <span data-ttu-id="2663a-173">Suivez les instructions pour vos outils :</span><span class="sxs-lookup"><span data-stu-id="2663a-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2663a-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2663a-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2663a-175">Dans **Explorateur de solutions**, sélectionnez le projet **BlazorSignalRApp. Server** .</span><span class="sxs-lookup"><span data-stu-id="2663a-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="2663a-176">Appuyez sur **CTRL + F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="2663a-176">Press **Ctrl+F5** to run the app without debugging.</span></span>

1. <span data-ttu-id="2663a-177">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="2663a-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2663a-178">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="2663a-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2663a-179">Le nom et le message s’affichent instantanément sur les deux pages :</span><span class="sxs-lookup"><span data-stu-id="2663a-179">The name and message are displayed on both pages instantly:</span></span>

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2663a-181">Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2663a-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="2663a-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2663a-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2663a-183">Sélectionnez **Déboguer** > **exécuter sans débogage** à partir de la barre d’outils.</span><span class="sxs-lookup"><span data-stu-id="2663a-183">Select **Debug** > **Run Without Debugging** from the toolbar.</span></span>

1. <span data-ttu-id="2663a-184">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="2663a-184">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2663a-185">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="2663a-185">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2663a-186">Le nom et le message s’affichent instantanément sur les deux pages :</span><span class="sxs-lookup"><span data-stu-id="2663a-186">The name and message are displayed on both pages instantly:</span></span>

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2663a-188">Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2663a-188">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="2663a-189">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2663a-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2663a-190">Dans la barre latérale **solution** , sélectionnez le projet **BlazorSignalRApp. Server** .</span><span class="sxs-lookup"><span data-stu-id="2663a-190">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="2663a-191">Dans le menu, sélectionnez **exécuter** > **Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="2663a-191">From the menu, select **Run** > **Start Without Debugging**.</span></span>

1. <span data-ttu-id="2663a-192">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="2663a-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2663a-193">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="2663a-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2663a-194">Le nom et le message s’affichent instantanément sur les deux pages :</span><span class="sxs-lookup"><span data-stu-id="2663a-194">The name and message are displayed on both pages instantly:</span></span>

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2663a-196">Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2663a-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2663a-197">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2663a-197">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="2663a-198">Dans une interface de commande, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="2663a-198">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="2663a-199">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="2663a-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2663a-200">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="2663a-200">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="2663a-201">Le nom et le message s’affichent instantanément sur les deux pages :</span><span class="sxs-lookup"><span data-stu-id="2663a-201">The name and message are displayed on both pages instantly:</span></span>

   ![L’exemple d’application signalr du webassembly éblouissant est ouvert dans deux fenêtres de navigateur qui affichent les messages échangés.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="2663a-203">Devis : *Star Trek VI : le pays indécouvert* [&copy;1991](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="2663a-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="2663a-204">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="2663a-204">Next steps</span></span>

<span data-ttu-id="2663a-205">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="2663a-205">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2663a-206">Créer un projet d’application hébergée Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="2663a-206">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="2663a-207">Ajouter la bibliothèque cliente SignalR</span><span class="sxs-lookup"><span data-stu-id="2663a-207">Add the SignalR client library</span></span>
> * <span data-ttu-id="2663a-208">Ajouter un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="2663a-208">Add a SignalR hub</span></span>
> * <span data-ttu-id="2663a-209">Ajouter des services de SignalR et un point de terminaison pour le Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="2663a-209">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="2663a-210">Ajouter du code de composant Razor pour la conversation</span><span class="sxs-lookup"><span data-stu-id="2663a-210">Add Razor component code for chat</span></span>

<span data-ttu-id="2663a-211">Pour en savoir plus sur la création d’applications Blazor, consultez la documentation Blazor :</span><span class="sxs-lookup"><span data-stu-id="2663a-211">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="2663a-212">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2663a-212">Additional resources</span></span>

* <xref:signalr/introduction>
