---
title: 'Tutoriel : Bien démarrer avec SignalR sur ASP.NET Core'
author: tdykstra
description: Dans ce tutoriel, vous allez créer une application de conversation qui utilise SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: db7f31963f6a4280069f1f4f82a547e2879e64bb
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751697"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="9e3b7-103">Tutoriel : Bien démarrer avec SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e3b7-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="9e3b7-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="9e3b7-105">Vous apprenez à :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9e3b7-106">Créer une application web qui utilise SignalR sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="9e3b7-107">Créer un hub SignalR sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="9e3b7-108">Établir une connexion au hub SignalR à partir de clients JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="9e3b7-109">Utiliser le hub pour envoyer des messages de n’importe quel client vers tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="9e3b7-110">À la fin, vous disposerez d’une application de conversation opérationnelle :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-110">At the end, you'll have a working chat app:</span></span>

![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="9e3b7-112">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e3b7-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9e3b7-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e3b7-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e3b7-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9e3b7-115">[Visual Studio 2017 version 15.7.3 ou ultérieure](https://www.visualstudio.com/downloads/) avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="9e3b7-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="9e3b7-116">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="9e3b7-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="9e3b7-117">[npm](https://www.npmjs.com/get-npm) (Gestionnaire de package pour Node.js, utilisé pour la bibliothèque de client JavaScript SignalR.)</span><span class="sxs-lookup"><span data-stu-id="9e3b7-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e3b7-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e3b7-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="9e3b7-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e3b7-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="9e3b7-120">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="9e3b7-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="9e3b7-121">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e3b7-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="9e3b7-122">[npm](https://www.npmjs.com/get-npm) (Gestionnaire de package pour Node.js, utilisé pour la bibliothèque de client JavaScript SignalR.)</span><span class="sxs-lookup"><span data-stu-id="9e3b7-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e3b7-123">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9e3b7-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="9e3b7-124">Visual Studio pour Mac version 7.5.4 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="9e3b7-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="9e3b7-125">[.NET Core SDK 2.1 ou version ultérieure](https://www.microsoft.com/net/download/all) (inclus dans l’installation de Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="9e3b7-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="9e3b7-126">[npm](https://www.npmjs.com/get-npm) (Gestionnaire de package pour Node.js, utilisé pour la bibliothèque de client JavaScript SignalR.)</span><span class="sxs-lookup"><span data-stu-id="9e3b7-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="9e3b7-127">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="9e3b7-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e3b7-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e3b7-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9e3b7-129">Dans le menu, sélectionnez **Fichier > Nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="9e3b7-130">Dans la boîte de dialogue **Nouveau projet**, sélectionnez **Installé > Visual C# > Web > Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="9e3b7-131">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-131">Name the project *SignalRChat*.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="9e3b7-133">Sélectionnez **Application web** pour créer un projet qui utilise Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="9e3b7-134">Sélectionnez **.NET Core** comme framework cible, sélectionnez **ASP.NET Core 2.1**, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e3b7-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e3b7-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="9e3b7-137">Ouvrez un dossier que vous pouvez utiliser pour un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="9e3b7-138">Dans le **Terminal intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e3b7-139">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9e3b7-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9e3b7-140">Dans le menu, sélectionnez **Fichier > Nouvelle solution**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="9e3b7-141">Sélectionnez **.NET Core > Application > Application web ASP.NET Core** (ne sélectionnez pas **Application web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="9e3b7-142">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-142">Select **Next**.</span></span>

* <span data-ttu-id="9e3b7-143">Nommez le projet *SignalRChat*, puis sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="9e3b7-144">Ajouter la bibliothèque de client SignalR</span><span class="sxs-lookup"><span data-stu-id="9e3b7-144">Add the SignalR client library</span></span>

<span data-ttu-id="9e3b7-145">La bibliothèque de serveur SignalR est comprise dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="9e3b7-146">En revanche, vous devez obtenir la bibliothèque de client JavaScript à partir de npm, le gestionnaire de package Node.js.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-146">But you have to get the JavaScript client library from npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e3b7-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e3b7-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="9e3b7-148">Dans la **Console du Gestionnaire de package**, accédez au dossier de projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e3b7-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e3b7-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="9e3b7-150">Basculez vers le nouveau dossier de projet.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e3b7-151">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9e3b7-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9e3b7-152">Dans le **Terminal**, accédez au dossier du projet (celui qui contient le fichier *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="9e3b7-153">Exécutez l’initialiseur npm pour créer un fichier *package.json* :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="9e3b7-154">La commande crée une sortie similaire à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="9e3b7-155">Installez le package de bibliothèque de client :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="9e3b7-156">La commande crée une sortie similaire à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="9e3b7-157">La commande `npm install` a téléchargé la bibliothèque de client JavaScript dans un sous-dossier sous *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="9e3b7-158">Copiez-la depuis cet emplacement vers un dossier sous *wwwroot* que vous pouvez référencer à partir de la page web de l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="9e3b7-159">Créez un dossier *signalr* dans *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="9e3b7-160">Copiez le fichier *signalr.js* de *node_modules/@aspnet/signalr/dist/browser* vers le nouveau dossier *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="9e3b7-161">Créer le hub SignalR</span><span class="sxs-lookup"><span data-stu-id="9e3b7-161">Create the SignalR hub</span></span>

<span data-ttu-id="9e3b7-162">Un [hub](xref:signalr/hubs) est une classe servant de pipeline global qui gère les communications client-serveur.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="9e3b7-163">Dans le dossier de projet SignalRChat, créez un dossier *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="9e3b7-164">Dans le dossier *Hubs*, créez un fichier *ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="9e3b7-165">La classe `ChatHub` hérite de la classe SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="9e3b7-166">La classe `Hub` gère les connexions, les groupes et la messagerie.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="9e3b7-167">La méthode `SendMessage` peut être appelée par n’importe quel client connecté.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="9e3b7-168">Elle envoie le message reçu à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-168">It sends the received message to all clients.</span></span> <span data-ttu-id="9e3b7-169">Le code SignalR est asynchrone afin de fournir une scalabilité maximale.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="9e3b7-170">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="9e3b7-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="9e3b7-171">Vous devez configurer le serveur SignalR pour que celui-ci transmette les requêtes SignalR à SignalR.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="9e3b7-172">Ajoutez le code en surbrillance suivant au fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="9e3b7-173">Ces modifications ajoutent SignalR au système d’[injection de dépendances](xref:fundamentals/dependency-injection) au pipeline de [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="9e3b7-174">Créer le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="9e3b7-174">Create the SignalR client code</span></span>

* <span data-ttu-id="9e3b7-175">Remplacez le contenu de *Pages\Index.cshtml* par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="9e3b7-176">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-176">The preceding code:</span></span>

  * <span data-ttu-id="9e3b7-177">Crée des zones de texte pour le nom et le texte du message, ainsi qu’un bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="9e3b7-178">Crée une liste avec `id="messagesList"` pour afficher les messages reçus en provenance du hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="9e3b7-179">Inclut des références de script à SignalR et le code d’application *chat.js* que vous créez à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="9e3b7-180">Dans le dossier *wwwroot/js*, créez un fichier *chat.js* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="9e3b7-181">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-181">The preceding code:</span></span>

  * <span data-ttu-id="9e3b7-182">Crée et lance une connexion.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="9e3b7-183">Ajoute au bouton Envoyer un gestionnaire qui envoie des messages au hub.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="9e3b7-184">Ajoute à l’objet de connexion un gestionnaire qui reçoit des messages à partir du hub et les ajoute à la liste.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="9e3b7-185">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="9e3b7-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9e3b7-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e3b7-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9e3b7-187">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9e3b7-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9e3b7-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9e3b7-189">Appuyez sur **Ctrl+F5** pour exécuter l’application sans débogage.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9e3b7-190">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="9e3b7-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9e3b7-191">Dans le menu, sélectionnez **Exécuter > Démarrer sans débogage**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="9e3b7-192">Copiez l’URL à partir de la barre d’adresse, ouvrez un autre onglet ou instance du navigateur, puis collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="9e3b7-193">Choisissez l’un des navigateurs, entrez un nom et un message, puis sélectionnez le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="9e3b7-194">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-194">The name and message are displayed on both pages instantly.</span></span>

  ![Exemple d’application SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="9e3b7-196">Si l’application ne fonctionne pas, ouvrez vos outils de développement (F12) de navigateur et accédez à la console.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="9e3b7-197">Vous pouvez observer des erreurs liées à votre code HTML et JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="9e3b7-198">Par exemple, supposez que vous placez *signalr.js* dans un dossier autre que celui stipulé.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="9e3b7-199">Dans ce cas, la référence à ce fichier ne fonctionnera pas et vous verrez une erreur 404 dans la console.</span><span class="sxs-lookup"><span data-stu-id="9e3b7-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="9e3b7-200">![Erreur de fichier SignalR.js introuvable](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="9e3b7-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e3b7-201">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9e3b7-201">Next steps</span></span>

<span data-ttu-id="9e3b7-202">Si vous souhaitez que les clients se connectent à une application SignalR à partir de différents domaines, vous devez activer le Partage des ressources cross-origin (CORS).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="9e3b7-203">Pour plus d’informations, consultez [Partage des ressources cross-origin](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="9e3b7-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="9e3b7-204">Pour en savoir plus sur SignalR, les hubs et les clients JavaScript, consultez ces ressources :</span><span class="sxs-lookup"><span data-stu-id="9e3b7-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="9e3b7-205">Introduction à SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e3b7-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="9e3b7-206">Utilisation des hubs dans SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e3b7-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="9e3b7-207">Client JavaScript SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e3b7-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
