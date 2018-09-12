---
title: 'Tutoriel : Bien démarrer avec SignalR sur ASP.NET Core'
author: tdykstra
description: Dans ce tutoriel, vous allez créer une application de conversation qui utilise SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6d96331a4630f766ca11edb056fd3e13b52b6ae4
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893163"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="09edb-103">Tutoriel : Bien démarrer avec SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09edb-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="09edb-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="09edb-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="09edb-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="09edb-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="09edb-106">Créer une application web qui utilise SignalR sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09edb-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="09edb-107">Créer un hub SignalR sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="09edb-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="09edb-108">Établir une connexion au hub SignalR à partir de clients JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09edb-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="09edb-109">Utiliser le hub pour envoyer des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="09edb-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="09edb-110">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="09edb-110">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="09edb-112">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="09edb-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09edb-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="09edb-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09edb-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09edb-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="09edb-115">[Visual Studio 2017 version 15.8 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="09edb-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="09edb-116">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="09edb-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="09edb-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09edb-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="09edb-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09edb-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="09edb-119">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="09edb-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="09edb-120">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09edb-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="09edb-121">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="09edb-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="09edb-122">Visual Studio pour Mac version 7.5.4 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="09edb-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="09edb-123">[.NET Core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all) (inclus dans l’installation de Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="09edb-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="09edb-124">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="09edb-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09edb-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09edb-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="09edb-126">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="09edb-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="09edb-127">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="09edb-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="09edb-128">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="09edb-128">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="09edb-130">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="09edb-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="09edb-131">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.1**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="09edb-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="09edb-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09edb-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="09edb-134">Ouvrez un dossier que vous pouvez utiliser pour un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="09edb-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="09edb-135">Dans le **Terminal intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="09edb-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="09edb-136">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="09edb-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="09edb-137">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="09edb-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="09edb-138">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="09edb-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="09edb-139">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="09edb-139">Select **Next**.</span></span>

* <span data-ttu-id="09edb-140">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="09edb-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="09edb-141">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="09edb-141">Add the SignalR client library</span></span>

<span data-ttu-id="09edb-142">La bibliothèque de serveur SignalR est comprise dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="09edb-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="09edb-143">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="09edb-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="09edb-144">Pour ce tutoriel, vous utilisez le [Gestionnaire de bibliothèque (LibMan)](xref:client-side/libman/index) pour obtenir la bibliothèque cliente à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="09edb-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="09edb-145">[unpkg](https://unpkg.com/#/) est un [réseau de distribution de contenu](https://wikipedia.org/wiki/Content_delivery_network) qui peut fournir tout ce qui est trouvé dans [npm, le Gestionnaire de package Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="09edb-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09edb-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09edb-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="09edb-147">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="09edb-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="09edb-148">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="09edb-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="09edb-149">Pour **Bibliothèque**, entrez _@aspnet/signalr@1_, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="09edb-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/libman1.png)

* <span data-ttu-id="09edb-151">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="09edb-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="09edb-152">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/*, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="09edb-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/libman2.png)

  <span data-ttu-id="09edb-154">[LibMan](xref:client-side/libman/index) crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="09edb-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="09edb-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09edb-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="09edb-156">Dans le **Terminal intégré**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="09edb-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="09edb-157">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="09edb-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="09edb-158">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="09edb-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="09edb-159">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="09edb-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="09edb-160">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="09edb-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="09edb-161">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="09edb-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="09edb-162">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="09edb-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="09edb-163">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="09edb-163">Copy only the specified files.</span></span>

  <span data-ttu-id="09edb-164">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="09edb-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="09edb-165">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="09edb-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="09edb-166">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="09edb-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="09edb-167">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="09edb-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="09edb-168">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="09edb-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="09edb-169">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="09edb-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="09edb-170">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="09edb-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="09edb-171">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="09edb-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="09edb-172">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="09edb-172">Copy only the specified files.</span></span>

  <span data-ttu-id="09edb-173">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="09edb-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="09edb-174">Créer le hub SignalR</span><span class="sxs-lookup"><span data-stu-id="09edb-174">Create the SignalR hub</span></span>

<span data-ttu-id="09edb-175">Un [hub](xref:signalr/hubs) est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="09edb-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="09edb-176">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="09edb-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="09edb-177">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="09edb-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="09edb-178">La classe `ChatHub` hérite de la classe SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub).</span><span class="sxs-lookup"><span data-stu-id="09edb-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="09edb-179">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="09edb-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="09edb-180">La méthode `SendMessage` peut être appelée par n’importe quel client connecté.</span><span class="sxs-lookup"><span data-stu-id="09edb-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="09edb-181">Elle envoie le message reçu à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="09edb-181">It sends the received message to all clients.</span></span> <span data-ttu-id="09edb-182">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="09edb-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="09edb-183">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="09edb-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="09edb-184">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="09edb-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="09edb-185">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="09edb-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="09edb-186">Ces modifications ajoutent SignalR au système d’[injection de dépendances](xref:fundamentals/dependency-injection) au pipeline de [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="09edb-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="09edb-187">Créer le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="09edb-187">Create the SignalR client code</span></span>

* <span data-ttu-id="09edb-188">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="09edb-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="09edb-189">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="09edb-189">The preceding code:</span></span>

  * <span data-ttu-id="09edb-190">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="09edb-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="09edb-191">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="09edb-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="09edb-192">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="09edb-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="09edb-193">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="09edb-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="09edb-194">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="09edb-194">The preceding code:</span></span>

  * <span data-ttu-id="09edb-195">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="09edb-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="09edb-196">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="09edb-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="09edb-197">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="09edb-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="09edb-198">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="09edb-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09edb-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09edb-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="09edb-200">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="09edb-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="09edb-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09edb-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="09edb-202">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="09edb-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="09edb-203">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="09edb-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="09edb-204">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="09edb-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="09edb-205">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="09edb-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="09edb-206">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="09edb-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="09edb-207">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="09edb-207">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="09edb-209">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="09edb-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="09edb-210">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09edb-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="09edb-211">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="09edb-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="09edb-212">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="09edb-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="09edb-213">![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="09edb-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="09edb-214">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="09edb-214">Next steps</span></span>

<span data-ttu-id="09edb-215">Si vous souhaitez que les clients se connectent à une application SignalR à partir de différents domaines, vous devez activer le Partage des ressources cross-origin (CORS).</span><span class="sxs-lookup"><span data-stu-id="09edb-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="09edb-216">Pour plus d’informations, consultez [Partage des ressources cross-origin](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="09edb-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="09edb-217">Pour en savoir plus sur SignalR, les hubs et les clients JavaScript, consultez ces ressources :</span><span class="sxs-lookup"><span data-stu-id="09edb-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="09edb-218">Introduction à SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09edb-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="09edb-219">Utilisation des hubs dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09edb-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="09edb-220">Client JavaScript SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09edb-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
