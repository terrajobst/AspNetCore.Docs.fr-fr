---
title: Bien démarrer avec SignalR sur ASP.NET Core
author: tdykstra
description: Dans ce tutoriel, vous allez créer une application à l’aide de SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095489"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="ab471-103">Bien démarrer avec SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab471-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="ab471-104">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab471-104">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="ab471-106">Ce tutoriel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="ab471-106">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ab471-107">Créer une application web SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab471-107">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="ab471-108">Créer un hub SignalR pour transmettre le contenu aux clients</span><span class="sxs-lookup"><span data-stu-id="ab471-108">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="ab471-109">Modifier la classe `Startup` et configurer l’application</span><span class="sxs-lookup"><span data-stu-id="ab471-109">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="ab471-110">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab471-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab471-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ab471-111">Prerequisites</span></span>

<span data-ttu-id="ab471-112">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="ab471-112">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ab471-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab471-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="ab471-114">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ab471-114">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="ab471-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 ou ultérieure avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="ab471-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* <span data-ttu-id="ab471-116">[npm](https://www.npmjs.com/get-npm) (gestionnaire de package pour Node.js)</span><span class="sxs-lookup"><span data-stu-id="ab471-116">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ab471-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab471-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="ab471-118">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="ab471-118">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="ab471-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab471-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="ab471-120">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab471-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="ab471-121">[npm](https://www.npmjs.com/get-npm) (gestionnaire de package pour Node.js)</span><span class="sxs-lookup"><span data-stu-id="ab471-121">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="ab471-122">Créer un projet ASP.NET Core qui héberge le serveur et le client SignalR</span><span class="sxs-lookup"><span data-stu-id="ab471-122">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ab471-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab471-123">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="ab471-124">Utilisez l’option de menu **Fichier** > **Nouveau projet** et choisissez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ab471-124">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ab471-125">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="ab471-125">Name the project *SignalRChat*.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="ab471-127">Sélectionnez **Application web** pour créer un projet à l’aide de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ab471-127">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="ab471-128">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab471-128">Then select **OK**.</span></span> <span data-ttu-id="ab471-129">Vérifiez que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, même si SignalR s’exécute sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="ab471-129">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="ab471-131">Visual Studio inclut le package `Microsoft.AspNetCore.SignalR` contenant ses bibliothèques de serveur dans le cadre de son modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ab471-131">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ab471-132">Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="ab471-132">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="ab471-133">Exécutez les commandes suivantes dans la fenêtre **Console du Gestionnaire de package** à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="ab471-133">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="ab471-134">Créez un dossier nommé « signalr » à l’intérieur du dossier *wwwroot/lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="ab471-134">Create a new folder named "signalr" inside the  *wwwroot/lib* folder in your project.</span></span> <span data-ttu-id="ab471-135">Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.</span><span class="sxs-lookup"><span data-stu-id="ab471-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ab471-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab471-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="ab471-137">À partir du **Terminal intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ab471-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="ab471-138">Installez la bibliothèque cliente JavaScript à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="ab471-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="ab471-139">Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="ab471-139">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="ab471-140">Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.</span><span class="sxs-lookup"><span data-stu-id="ab471-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="ab471-141">Créer le hub SignalR</span><span class="sxs-lookup"><span data-stu-id="ab471-141">Create the SignalR Hub</span></span>

<span data-ttu-id="ab471-142">Un hub est une classe qui sert de pipeline global permettant au client et au serveur d’appeler des méthodes de l’un à l’autre.</span><span class="sxs-lookup"><span data-stu-id="ab471-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ab471-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab471-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="ab471-144">Ajoutez une classe au projet en choisissant **Fichier** > **Nouveau** > **Fichier** et en sélectionnant **Classe Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="ab471-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="ab471-145">Nommez la classe `ChatHub` et le fichier *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="ab471-145">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="ab471-146">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="ab471-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="ab471-147">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données.</span><span class="sxs-lookup"><span data-stu-id="ab471-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="ab471-148">Créez la méthode `SendMessage` qui envoie un message à tous les clients de conversation connectés.</span><span class="sxs-lookup"><span data-stu-id="ab471-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="ab471-149">Notez qu’elle retourne une [Tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ab471-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="ab471-150">Le code asynchrone est plus scalable.</span><span class="sxs-lookup"><span data-stu-id="ab471-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ab471-151">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab471-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="ab471-152">Ouvrez le dossier *SignalRChat* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ab471-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="ab471-153">Ajoutez une classe au projet en sélectionnant **Fichier** > **Nouveau fichier** dans le menu.</span><span class="sxs-lookup"><span data-stu-id="ab471-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="ab471-154">Nommez la classe `ChatHub` et le fichier *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="ab471-154">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="ab471-155">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="ab471-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="ab471-156">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données avec les clients.</span><span class="sxs-lookup"><span data-stu-id="ab471-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="ab471-157">Ajoutez une méthode `SendMessage` à la classe.</span><span class="sxs-lookup"><span data-stu-id="ab471-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="ab471-158">La méthode `SendMessage` envoie un message à tous les clients de conversation connectés.</span><span class="sxs-lookup"><span data-stu-id="ab471-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="ab471-159">Notez qu’elle retourne une [Tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="ab471-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="ab471-160">Le code asynchrone est plus scalable.</span><span class="sxs-lookup"><span data-stu-id="ab471-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="ab471-161">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="ab471-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="ab471-162">Vous devez configurer le serveur SignalR pour qu’il sache qu’il doit transmettre les requêtes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab471-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="ab471-163">Pour configurer un projet SignalR, modifiez la méthode `Startup.ConfigureServices` du projet.</span><span class="sxs-lookup"><span data-stu-id="ab471-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="ab471-164">`services.AddSignalR` rend les services SignalR accessibles au système d’[injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ab471-164">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="ab471-165">Configurez des itinéraires vers vos hubs avec `UseSignalR` dans la méthode `Configure`.</span><span class="sxs-lookup"><span data-stu-id="ab471-165">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="ab471-166">`app.UseSignalR` ajoute SignalR au pipeline d’[intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="ab471-166">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="ab471-167">Créer le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="ab471-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="ab471-168">Ajoutez un fichier JavaScript nommé *chat.js* au dossier *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="ab471-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="ab471-169">Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ab471-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="ab471-170">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="ab471-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="ab471-171">Le code HTML précédent affiche des champs de nom et de message, ainsi qu’un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="ab471-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="ab471-172">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="ab471-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="ab471-173">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="ab471-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ab471-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab471-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ab471-175">Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer un navigateur et chargez le site web localement.</span><span class="sxs-lookup"><span data-stu-id="ab471-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="ab471-176">Copiez l’URL à partir de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="ab471-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="ab471-177">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="ab471-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ab471-178">Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**.</span><span class="sxs-lookup"><span data-stu-id="ab471-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="ab471-179">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="ab471-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ab471-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab471-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ab471-181">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="ab471-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="ab471-182">L’exécution du programme ouvre une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="ab471-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="ab471-183">Ouvrez une autre fenêtre de navigateur et chargez-y le site web localement.</span><span class="sxs-lookup"><span data-stu-id="ab471-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="ab471-184">Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**.</span><span class="sxs-lookup"><span data-stu-id="ab471-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="ab471-185">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="ab471-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Solution](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="ab471-187">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="ab471-187">Related resources</span></span>

[<span data-ttu-id="ab471-188">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ab471-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
