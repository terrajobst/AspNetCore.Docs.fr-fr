---
title: Bien démarrer avec SignalR sur ASP.NET Core
author: rachelappel
description: Dans ce tutoriel, vous allez créer une application à l’aide de SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 62cef2d6f032caa2f048cfdd49a225d975dad10d
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033340"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="2ef8e-103">Bien démarrer avec SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ef8e-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="2ef8e-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="2ef8e-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="2ef8e-105">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="2ef8e-107">Ce tutoriel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="2ef8e-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2ef8e-108">Créer une application web SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ef8e-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="2ef8e-109">Créer un hub SignalR pour transmettre le contenu aux clients</span><span class="sxs-lookup"><span data-stu-id="2ef8e-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="2ef8e-110">Modifier la classe `Startup` et configurer l’application</span><span class="sxs-lookup"><span data-stu-id="2ef8e-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="2ef8e-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2ef8e-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ef8e-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="2ef8e-112">Prerequisites</span></span>

<span data-ttu-id="2ef8e-113">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="2ef8e-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ef8e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ef8e-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="2ef8e-115">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="2ef8e-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="2ef8e-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 ou ultérieure avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="2ef8e-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="2ef8e-117">npm</span><span class="sxs-lookup"><span data-stu-id="2ef8e-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ef8e-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ef8e-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="2ef8e-119">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="2ef8e-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="2ef8e-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ef8e-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="2ef8e-121">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ef8e-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="2ef8e-122">npm</span><span class="sxs-lookup"><span data-stu-id="2ef8e-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="2ef8e-123">Créer un projet ASP.NET Core qui héberge le serveur et le client SignalR</span><span class="sxs-lookup"><span data-stu-id="2ef8e-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ef8e-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ef8e-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="2ef8e-125">Utilisez l’option de menu **Fichier** > **Nouveau projet** et choisissez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2ef8e-126">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-126">Name the project *SignalRChat*.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="2ef8e-128">Sélectionnez **Application web** pour créer un projet à l’aide de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="2ef8e-129">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-129">Then select **OK**.</span></span> <span data-ttu-id="2ef8e-130">Vérifiez que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, même si SignalR s’exécute sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="2ef8e-132">Visual Studio inclut le package `Microsoft.AspNetCore.SignalR` contenant ses bibliothèques de serveur dans le cadre de son modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="2ef8e-133">Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="2ef8e-134">Exécutez les commandes suivantes dans la fenêtre **Console du Gestionnaire de package** à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="2ef8e-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="2ef8e-135">Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="2ef8e-136">Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ef8e-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ef8e-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="2ef8e-138">À partir du **Terminal intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2ef8e-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="2ef8e-139">Installez la bibliothèque cliente JavaScript à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="2ef8e-140">Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="2ef8e-141">Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="2ef8e-142">Créer le hub SignalR</span><span class="sxs-lookup"><span data-stu-id="2ef8e-142">Create the SignalR Hub</span></span>

<span data-ttu-id="2ef8e-143">Un hub est une classe qui sert de pipeline global permettant au client et au serveur d’appeler des méthodes de l’un à l’autre.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ef8e-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ef8e-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="2ef8e-145">Ajoutez une classe au projet en choisissant **Fichier** > **Nouveau** > **Fichier** et en sélectionnant **Classe Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="2ef8e-146">Nommez la classe `ChatHub` et le fichier *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="2ef8e-147">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="2ef8e-148">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="2ef8e-149">Créez la méthode `SendMessage` qui envoie un message à tous les clients de conversation connectés.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="2ef8e-150">Notez qu’elle retourne une [Tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="2ef8e-151">Le code asynchrone est plus scalable.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ef8e-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ef8e-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="2ef8e-153">Ouvrez le dossier *SignalRChat* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="2ef8e-154">Ajoutez une classe au projet en sélectionnant **Fichier** > **Nouveau fichier** dans le menu.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="2ef8e-155">Nommez la classe `ChatHub` et le fichier *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="2ef8e-156">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="2ef8e-157">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données avec les clients.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="2ef8e-158">Ajoutez une méthode `SendMessage` à la classe.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="2ef8e-159">La méthode `SendMessage` envoie un message à tous les clients de conversation connectés.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="2ef8e-160">Notez qu’elle retourne une [Tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="2ef8e-161">Le code asynchrone est plus scalable.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="2ef8e-162">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="2ef8e-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="2ef8e-163">Vous devez configurer le serveur SignalR pour qu’il sache qu’il doit transmettre les requêtes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="2ef8e-164">Pour configurer un projet SignalR, modifiez la méthode `Startup.ConfigureServices` du projet.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="2ef8e-165">`services.AddSignalR` Ajoute SignalR dans le cadre du pipeline d’[intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="2ef8e-165">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="2ef8e-166">Configurez des routes vers vos hubs à l’aide de `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-166">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="2ef8e-167">Créer le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="2ef8e-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="2ef8e-168">Ajoutez un fichier JavaScript nommé *chat.js* au dossier *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="2ef8e-169">Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2ef8e-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="2ef8e-170">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2ef8e-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="2ef8e-171">Le code HTML précédent affiche des champs de nom et de message, ainsi qu’un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="2ef8e-172">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2ef8e-173">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="2ef8e-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ef8e-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ef8e-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2ef8e-175">Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer un navigateur et chargez le site web localement.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="2ef8e-176">Copiez l’URL à partir de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="2ef8e-177">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2ef8e-178">Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="2ef8e-179">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ef8e-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ef8e-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2ef8e-181">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="2ef8e-182">L’exécution du programme ouvre une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="2ef8e-183">Ouvrez une autre fenêtre de navigateur et chargez-y le site web localement.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="2ef8e-184">Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="2ef8e-185">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="2ef8e-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Solution](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="2ef8e-187">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="2ef8e-187">Related resources</span></span>

[<span data-ttu-id="2ef8e-188">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2ef8e-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
