---
title: Bien démarrer avec SignalR sur ASP.NET Core
author: rachelappel
description: Dans ce tutoriel, vous allez créer une application à l’aide de SignalR pour ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 8762a4be1032d58014dd32dfdd3707197e14c6f9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297198"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="0c176-103">Bien démarrer avec SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c176-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="0c176-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="0c176-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="0c176-105">Ce tutoriel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0c176-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="0c176-107">Ce tutoriel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="0c176-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0c176-108">Créer une application web SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c176-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="0c176-109">Créer un hub SignalR pour transmettre le contenu aux clients</span><span class="sxs-lookup"><span data-stu-id="0c176-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="0c176-110">Modifier la classe `Startup` et configurer l’application</span><span class="sxs-lookup"><span data-stu-id="0c176-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="0c176-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c176-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="0c176-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="0c176-112">Prerequisites</span></span>

<span data-ttu-id="0c176-113">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="0c176-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c176-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c176-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="0c176-115">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="0c176-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="0c176-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 ou ultérieure avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="0c176-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="0c176-117">npm</span><span class="sxs-lookup"><span data-stu-id="0c176-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0c176-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c176-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="0c176-119">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="0c176-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="0c176-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c176-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="0c176-121">C# pour Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c176-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="0c176-122">npm</span><span class="sxs-lookup"><span data-stu-id="0c176-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="0c176-123">Créer un projet ASP.NET Core qui héberge le serveur et le client SignalR</span><span class="sxs-lookup"><span data-stu-id="0c176-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c176-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c176-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="0c176-125">Utilisez l’option de menu **Fichier** > **Nouveau projet** et choisissez **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c176-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0c176-126">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="0c176-126">Name the project *SignalRChat*.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="0c176-128">Sélectionnez **Application web** pour créer un projet à l’aide de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0c176-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="0c176-129">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="0c176-129">Then select **OK**.</span></span> <span data-ttu-id="0c176-130">Vérifiez que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, même si SignalR s’exécute sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="0c176-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="0c176-132">Visual Studio inclut le package `Microsoft.AspNetCore.SignalR` contenant ses bibliothèques de serveur dans le cadre de son modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0c176-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0c176-133">Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="0c176-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="0c176-134">Exécutez les commandes suivantes dans la fenêtre **Console du Gestionnaire de package** à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="0c176-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="0c176-135">Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="0c176-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="0c176-136">Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.</span><span class="sxs-lookup"><span data-stu-id="0c176-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0c176-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c176-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="0c176-138">À partir du **Terminal intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="0c176-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="0c176-139">Installez la bibliothèque cliente JavaScript à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="0c176-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="0c176-140">Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="0c176-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="0c176-141">Copiez le fichier *signalr.js* de *node_modules\\@aspnet\signalr\dist\browser* vers ce dossier.</span><span class="sxs-lookup"><span data-stu-id="0c176-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="0c176-142">Créer le hub SignalR</span><span class="sxs-lookup"><span data-stu-id="0c176-142">Create the SignalR Hub</span></span>

<span data-ttu-id="0c176-143">Un hub est une classe qui sert de pipeline global permettant au client et au serveur d’appeler des méthodes de l’un à l’autre.</span><span class="sxs-lookup"><span data-stu-id="0c176-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c176-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c176-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="0c176-145">Ajoutez une classe au projet en choisissant **Fichier** > **Nouveau** > **Fichier** et en sélectionnant **Classe Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="0c176-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="0c176-146">Nommez le fichier *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="0c176-146">Name the file *ChatHub*.</span></span>

2. <span data-ttu-id="0c176-147">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="0c176-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0c176-148">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données.</span><span class="sxs-lookup"><span data-stu-id="0c176-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="0c176-149">Créez la méthode `SendMessage` qui envoie un message à tous les clients de conversation connectés.</span><span class="sxs-lookup"><span data-stu-id="0c176-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="0c176-150">Notez qu’elle retourne une [Tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="0c176-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="0c176-151">Le code asynchrone est plus scalable.</span><span class="sxs-lookup"><span data-stu-id="0c176-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0c176-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c176-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="0c176-153">Ouvrez le dossier *SignalRChat* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0c176-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="0c176-154">Ajoutez une classe au projet en sélectionnant **Fichier** > **Nouveau fichier** dans le menu.</span><span class="sxs-lookup"><span data-stu-id="0c176-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="0c176-155">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="0c176-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0c176-156">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que l’envoi et la réception de données avec les clients.</span><span class="sxs-lookup"><span data-stu-id="0c176-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="0c176-157">Ajoutez une méthode `SendMessage` à la classe.</span><span class="sxs-lookup"><span data-stu-id="0c176-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="0c176-158">La méthode `SendMessage` envoie un message à tous les clients de conversation connectés.</span><span class="sxs-lookup"><span data-stu-id="0c176-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="0c176-159">Notez qu’elle retourne une [Tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="0c176-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="0c176-160">Le code asynchrone est plus scalable.</span><span class="sxs-lookup"><span data-stu-id="0c176-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="0c176-161">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="0c176-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="0c176-162">Vous devez configurer le serveur SignalR pour qu’il sache qu’il doit transmettre les requêtes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="0c176-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="0c176-163">Pour configurer un projet SignalR, modifiez la méthode `Startup.ConfigureServices` du projet.</span><span class="sxs-lookup"><span data-stu-id="0c176-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="0c176-164">`services.AddSignalR` Ajoute SignalR dans le cadre du pipeline d’[intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="0c176-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="0c176-165">Configurez des routes vers vos hubs à l’aide de `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="0c176-165">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="0c176-166">Créer le code client SignalR</span><span class="sxs-lookup"><span data-stu-id="0c176-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="0c176-167">Ajoutez un fichier JavaScript nommé *chat.js* au dossier *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="0c176-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="0c176-168">Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0c176-168">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="0c176-169">Remplacez le contenu de *Pages\Index.cshtml* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0c176-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="0c176-170">Le code HTML précédent affiche des champs de nom et de message, ainsi qu’un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="0c176-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="0c176-171">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="0c176-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0c176-172">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="0c176-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c176-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c176-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0c176-174">Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer un navigateur et chargez le site web localement.</span><span class="sxs-lookup"><span data-stu-id="0c176-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="0c176-175">Copiez l’URL à partir de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="0c176-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="0c176-176">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="0c176-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="0c176-177">Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**.</span><span class="sxs-lookup"><span data-stu-id="0c176-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0c176-178">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="0c176-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0c176-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c176-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="0c176-180">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="0c176-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="0c176-181">L’exécution du programme ouvre une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="0c176-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="0c176-182">Ouvrez une autre fenêtre de navigateur et chargez-y le site web localement.</span><span class="sxs-lookup"><span data-stu-id="0c176-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="0c176-183">Choisissez l’un des navigateurs, entrez un nom et un message, puis cliquez sur le bouton **Send**.</span><span class="sxs-lookup"><span data-stu-id="0c176-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0c176-184">Le nom et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="0c176-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Solution](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="0c176-186">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="0c176-186">Related resources</span></span>

[<span data-ttu-id="0c176-187">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0c176-187">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
