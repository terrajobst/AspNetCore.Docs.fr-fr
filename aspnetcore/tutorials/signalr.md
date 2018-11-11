---
title: Bien démarrer avec ASP.NET Core SignalR
author: tdykstra
description: Dans ce tutoriel, vous créez une application de conversation qui utilise ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 7d9f3a3f8aa7a5e47169da66e6fa2d6a28de3853
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021246"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="2c6f9-103">Tutoriel : Bien démarrer avec ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2c6f9-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="2c6f9-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="2c6f9-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c6f9-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-106">Create a web project.</span></span>
> * <span data-ttu-id="2c6f9-107">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="2c6f9-108">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="2c6f9-109">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="2c6f9-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="2c6f9-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-111">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="2c6f9-113">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2c6f9-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c6f9-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="2c6f9-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c6f9-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c6f9-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c6f9-116">[Visual Studio 2017 version 15.8 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="2c6f9-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="2c6f9-117">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="2c6f9-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c6f9-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c6f9-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="2c6f9-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c6f9-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="2c6f9-120">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="2c6f9-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="2c6f9-121">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c6f9-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c6f9-122">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2c6f9-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="2c6f9-123">Visual Studio pour Mac version 7.5.4 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="2c6f9-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="2c6f9-124">[.NET Core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all) (inclus dans l’installation de Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="2c6f9-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="2c6f9-125">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="2c6f9-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c6f9-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c6f9-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2c6f9-127">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="2c6f9-128">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2c6f9-129">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-129">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="2c6f9-131">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="2c6f9-132">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.1**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c6f9-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c6f9-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="2c6f9-135">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal) dans le dossier dans lequel le nouveau dossier de projet va être créé.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-135">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="2c6f9-136">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-136">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c6f9-137">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2c6f9-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2c6f9-138">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="2c6f9-139">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="2c6f9-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="2c6f9-140">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-140">Select **Next**.</span></span>

* <span data-ttu-id="2c6f9-141">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="2c6f9-142">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="2c6f9-142">Add the SignalR client library</span></span>

<span data-ttu-id="2c6f9-143">La bibliothèque de serveur SignalR est incluse dans le métapackage `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="2c6f9-144">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="2c6f9-145">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="2c6f9-146">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c6f9-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c6f9-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="2c6f9-148">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="2c6f9-149">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="2c6f9-150">Pour **Bibliothèque**, entrez `@aspnet/signalr@1`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/libman1.png)

* <span data-ttu-id="2c6f9-152">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="2c6f9-153">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/*, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/libman2.png)

  <span data-ttu-id="2c6f9-155">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c6f9-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c6f9-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="2c6f9-157">Dans le terminal intégré, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-157">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="2c6f9-158">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="2c6f9-159">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="2c6f9-160">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="2c6f9-161">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="2c6f9-162">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="2c6f9-163">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-163">Copy only the specified files.</span></span>

  <span data-ttu-id="2c6f9-164">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c6f9-165">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2c6f9-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2c6f9-166">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="2c6f9-167">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="2c6f9-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="2c6f9-168">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="2c6f9-169">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="2c6f9-170">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="2c6f9-171">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="2c6f9-172">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-172">Copy only the specified files.</span></span>

  <span data-ttu-id="2c6f9-173">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="2c6f9-174">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="2c6f9-174">Create a SignalR hub</span></span>

<span data-ttu-id="2c6f9-175">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-175">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="2c6f9-176">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="2c6f9-177">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="2c6f9-178">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-178">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="2c6f9-179">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="2c6f9-180">La méthode `SendMessage` peut être appelée par n’importe quel client connecté.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="2c6f9-181">Elle envoie le message reçu à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-181">It sends the received message to all clients.</span></span> <span data-ttu-id="2c6f9-182">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="2c6f9-183">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="2c6f9-183">Configure SignalR</span></span>

<span data-ttu-id="2c6f9-184">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="2c6f9-185">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="2c6f9-186">Ces modifications ajoutent SignalR au système d’injection de dépendances ASP.NET Core et au pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-186">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="2c6f9-187">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="2c6f9-187">Add SignalR client code</span></span>

* <span data-ttu-id="2c6f9-188">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="2c6f9-189">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-189">The preceding code:</span></span>

  * <span data-ttu-id="2c6f9-190">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="2c6f9-191">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="2c6f9-192">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="2c6f9-193">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="2c6f9-194">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-194">The preceding code:</span></span>

  * <span data-ttu-id="2c6f9-195">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="2c6f9-196">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="2c6f9-197">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2c6f9-198">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="2c6f9-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c6f9-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c6f9-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c6f9-200">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c6f9-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c6f9-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2c6f9-202">Dans le terminal intégré, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-202">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c6f9-203">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="2c6f9-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2c6f9-204">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="2c6f9-205">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="2c6f9-206">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="2c6f9-207">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-207">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="2c6f9-209">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="2c6f9-210">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="2c6f9-211">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="2c6f9-212">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="2c6f9-213">![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="2c6f9-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c6f9-214">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="2c6f9-214">Next steps</span></span>

<span data-ttu-id="2c6f9-215">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-215">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c6f9-216">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-216">Create a web app project.</span></span>
> * <span data-ttu-id="2c6f9-217">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-217">Add the SignalR client library.</span></span>
> * <span data-ttu-id="2c6f9-218">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-218">Create a SignalR hub.</span></span>
> * <span data-ttu-id="2c6f9-219">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-219">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="2c6f9-220">Ajouter le code qui utilise le hub pour envoyer des messages de n’importe quel client à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="2c6f9-220">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="2c6f9-221">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="2c6f9-221">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2c6f9-222">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2c6f9-222">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
