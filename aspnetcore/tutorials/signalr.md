---
title: Bien démarrer avec ASP.NET Core SignalR
author: tdykstra
description: Dans ce tutoriel, vous créez une application de conversation qui utilise ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: c059ace7ebe0e65ecb3ac068677d65ae148322a0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207678"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="15505-103">Tutoriel : Bien démarrer avec ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="15505-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="15505-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="15505-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="15505-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="15505-106">Créez un projet web.</span><span class="sxs-lookup"><span data-stu-id="15505-106">Create a web project.</span></span>
> * <span data-ttu-id="15505-107">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="15505-108">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="15505-109">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="15505-110">Ajouter du code qui envoie des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="15505-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="15505-111">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="15505-111">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="15505-113">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="15505-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15505-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="15505-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15505-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15505-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="15505-116">[Visual Studio 2017 version 15.8 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="15505-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="15505-117">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="15505-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15505-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15505-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="15505-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15505-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="15505-120">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="15505-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="15505-121">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15505-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15505-122">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="15505-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="15505-123">Visual Studio pour Mac version 7.5.4 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="15505-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="15505-124">[.NET Core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all) (inclus dans l’installation de Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="15505-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="15505-125">Créer un projet web</span><span class="sxs-lookup"><span data-stu-id="15505-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15505-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15505-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="15505-127">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="15505-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="15505-128">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="15505-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="15505-129">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="15505-129">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="15505-131">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="15505-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="15505-132">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.1**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="15505-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15505-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15505-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="15505-135">Ouvrez un dossier que vous pouvez utiliser pour un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="15505-135">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="15505-136">Dans le **Terminal intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="15505-136">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15505-137">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="15505-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="15505-138">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="15505-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="15505-139">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="15505-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="15505-140">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="15505-140">Select **Next**.</span></span>

* <span data-ttu-id="15505-141">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="15505-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="15505-142">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="15505-142">Add the SignalR client library</span></span>

<span data-ttu-id="15505-143">La bibliothèque de serveur SignalR est incluse dans le métapackage `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="15505-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="15505-144">La bibliothèque cliente JavaScript n’est pas incluse automatiquement dans le projet.</span><span class="sxs-lookup"><span data-stu-id="15505-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="15505-145">Pour ce tutoriel, vous utilisez le gestionnaire de bibliothèque (LibMan) pour obtenir la bibliothèque de client à partir de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="15505-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="15505-146">unpkg est un réseau de distribution de contenu (CDN) qui peut fournir tout ce qui est trouvé dans npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="15505-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15505-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15505-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="15505-148">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter** > **Bibliothèque côté client**.</span><span class="sxs-lookup"><span data-stu-id="15505-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="15505-149">Dans la boîte de dialogue **Ajouter une bibliothèque côté Client**, pour **Fournisseur** sélectionnez **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="15505-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="15505-150">Pour **Bibliothèque**, entrez `@aspnet/signalr@1`, puis sélectionnez la version la plus récente qui n’est pas en préversion.</span><span class="sxs-lookup"><span data-stu-id="15505-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner la bibliothèque](signalr/_static/libman1.png)

* <span data-ttu-id="15505-152">Sélectionnez **Choisir des fichiers spécifiques**, développez le dossier *dist/browser*, puis sélectionnez *signalr.js* et *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="15505-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="15505-153">Définissez **Emplacement cible** sur *wwwroot/lib/signalr/*, puis sélectionnez **Installer**.</span><span class="sxs-lookup"><span data-stu-id="15505-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Boîte de dialogue Ajouter une bibliothèque côté client - sélectionner les fichiers et la destination](signalr/_static/libman2.png)

  <span data-ttu-id="15505-155">LibMan crée un dossier *wwwroot/lib/signalr* et y copie les fichiers sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="15505-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15505-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15505-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="15505-157">Dans le **Terminal intégré**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="15505-157">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="15505-158">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="15505-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="15505-159">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="15505-159">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="15505-160">Vous devrez peut-être attendre quelques secondes avant que la sortie ne s’affiche.</span><span class="sxs-lookup"><span data-stu-id="15505-160">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="15505-161">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="15505-161">The parameters specify the following options:</span></span>
  * <span data-ttu-id="15505-162">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="15505-162">Use the unpkg provider.</span></span>
  * <span data-ttu-id="15505-163">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="15505-163">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="15505-164">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="15505-164">Copy only the specified files.</span></span>

  <span data-ttu-id="15505-165">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="15505-165">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15505-166">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="15505-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="15505-167">Dans le **Terminal**, exécutez la commande suivante pour installer LibMan.</span><span class="sxs-lookup"><span data-stu-id="15505-167">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="15505-168">Accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="15505-168">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="15505-169">Exécutez la commande suivante pour obtenir la bibliothèque cliente SignalR à l’aide de LibMan.</span><span class="sxs-lookup"><span data-stu-id="15505-169">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="15505-170">Les paramètres spécifient les options suivantes :</span><span class="sxs-lookup"><span data-stu-id="15505-170">The parameters specify the following options:</span></span>
  * <span data-ttu-id="15505-171">Utilisez le fournisseur unpkg.</span><span class="sxs-lookup"><span data-stu-id="15505-171">Use the unpkg provider.</span></span>
  * <span data-ttu-id="15505-172">Copiez les fichiers vers la destination *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="15505-172">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="15505-173">Copiez uniquement les fichiers spécifiés.</span><span class="sxs-lookup"><span data-stu-id="15505-173">Copy only the specified files.</span></span>

  <span data-ttu-id="15505-174">La sortie se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="15505-174">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="15505-175">Créer un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="15505-175">Create a SignalR hub</span></span>

<span data-ttu-id="15505-176">Un *hub* est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="15505-176">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="15505-177">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="15505-177">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="15505-178">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="15505-178">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="15505-179">La classe `ChatHub` hérite la classe SignalR `Hub`.</span><span class="sxs-lookup"><span data-stu-id="15505-179">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="15505-180">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="15505-180">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="15505-181">La méthode `SendMessage` peut être appelée par n’importe quel client connecté.</span><span class="sxs-lookup"><span data-stu-id="15505-181">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="15505-182">Elle envoie le message reçu à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="15505-182">It sends the received message to all clients.</span></span> <span data-ttu-id="15505-183">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="15505-183">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="15505-184">Configurer SignalR</span><span class="sxs-lookup"><span data-stu-id="15505-184">Configure SignalR</span></span>

<span data-ttu-id="15505-185">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-185">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="15505-186">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="15505-186">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="15505-187">Ces modifications ajoutent SignalR au système d’injection de dépendances ASP.NET Core et au pipeline de middleware.</span><span class="sxs-lookup"><span data-stu-id="15505-187">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="15505-188">Ajouter le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="15505-188">Add SignalR client code</span></span>

* <span data-ttu-id="15505-189">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="15505-189">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="15505-190">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="15505-190">The preceding code:</span></span>

  * <span data-ttu-id="15505-191">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="15505-191">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="15505-192">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-192">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="15505-193">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="15505-193">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="15505-194">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="15505-194">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="15505-195">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="15505-195">The preceding code:</span></span>

  * <span data-ttu-id="15505-196">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="15505-196">Creates and starts a connection.</span></span>
  * <span data-ttu-id="15505-197">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="15505-197">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="15505-198">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="15505-198">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="15505-199">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="15505-199">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15505-200">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15505-200">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="15505-201">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="15505-201">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15505-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15505-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="15505-203">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="15505-203">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15505-204">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="15505-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="15505-205">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="15505-205">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="15505-206">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="15505-206">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="15505-207">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="15505-207">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="15505-208">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="15505-208">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="15505-210">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="15505-210">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="15505-211">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="15505-211">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="15505-212">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="15505-212">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="15505-213">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="15505-213">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="15505-214">![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="15505-214">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="15505-215">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="15505-215">Next steps</span></span>

<span data-ttu-id="15505-216">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="15505-216">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="15505-217">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="15505-217">Create a web app project.</span></span>
> * <span data-ttu-id="15505-218">Ajouter la bibliothèque de client SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-218">Add the SignalR client library.</span></span>
> * <span data-ttu-id="15505-219">Créer un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-219">Create a SignalR hub.</span></span>
> * <span data-ttu-id="15505-220">Configurer le projet pour utiliser SignalR.</span><span class="sxs-lookup"><span data-stu-id="15505-220">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="15505-221">Ajouter le code qui utilise le hub pour envoyer des messages de n’importe quel client à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="15505-221">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="15505-222">Pour en savoir plus sur SignalR, consultez :</span><span class="sxs-lookup"><span data-stu-id="15505-222">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="15505-223">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="15505-223">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
