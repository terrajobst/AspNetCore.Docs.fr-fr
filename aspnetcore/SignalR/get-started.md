---
title: Prise en main SignalR sur ASP.NET Core
author: rachelappel
description: Dans ce didacticiel, vous créez une application à l’aide de SignalR pour ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/09/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 11c8cf079a0922e925060ad3d439ba5e095ae22e
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="9ed66-103">Prise en main SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ed66-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="9ed66-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="9ed66-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="9ed66-105">Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ed66-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="9ed66-107">Ce didacticiel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="9ed66-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ed66-108">Créez un SignalR sur l’application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ed66-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="9ed66-109">Créer un concentrateur SignalR pour transmettre le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="9ed66-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="9ed66-110">Modifier la `Startup` classe et de configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="9ed66-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="9ed66-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9ed66-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="9ed66-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="9ed66-112">Prerequisites</span></span>

<span data-ttu-id="9ed66-113">Installer les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="9ed66-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ed66-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ed66-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9ed66-115">[Kit de développement .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="9ed66-115">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* <span data-ttu-id="9ed66-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,7 ou version ultérieure avec le **développement web ASP.NET et** la charge de travail</span><span class="sxs-lookup"><span data-stu-id="9ed66-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="9ed66-117">npm</span><span class="sxs-lookup"><span data-stu-id="9ed66-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ed66-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ed66-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9ed66-119">[Kit de développement .NET core 2.1.0 RC 1](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="9ed66-119">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* [<span data-ttu-id="9ed66-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ed66-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="9ed66-121">C# pour le Code de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ed66-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="9ed66-122">npm</span><span class="sxs-lookup"><span data-stu-id="9ed66-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="9ed66-123">Créez un projet ASP.NET Core qui héberge le serveur et client SignalR</span><span class="sxs-lookup"><span data-stu-id="9ed66-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ed66-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ed66-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="9ed66-125">Utilisez le **fichier** > **nouveau projet** menu option et choisissez **Application ASP.NET Core Web**.</span><span class="sxs-lookup"><span data-stu-id="9ed66-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="9ed66-126">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="9ed66-126">Name the project *SignalRChat*.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="9ed66-128">Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="9ed66-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="9ed66-129">Puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ed66-129">Then select **OK**.</span></span> <span data-ttu-id="9ed66-130">Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, si SignalR s’exécute sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="9ed66-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="9ed66-132">Visual Studio inclut la `Microsoft.AspNetCore.SignalR` package contenant ses bibliothèques du serveur dans le cadre de son **Application ASP.NET Core Web** modèle.</span><span class="sxs-lookup"><span data-stu-id="9ed66-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="9ed66-133">Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="9ed66-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="9ed66-134">Exécutez les commandes suivantes le **Package Manager Console** fenêtre, à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="9ed66-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="9ed66-135">Copie le *signalr.js* à partir du fichier *node_modules\\ @aspnet\signalr\dist\browser*  à la *lib* dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="9ed66-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ed66-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ed66-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="9ed66-137">À partir de la **Terminal Server intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9ed66-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="9ed66-138">Installer la bibliothèque cliente JavaScript à l’aide *npm*.</span><span class="sxs-lookup"><span data-stu-id="9ed66-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

3. <span data-ttu-id="9ed66-139">Copie le *signalr.js* à partir du fichier *node_modules\\ @aspnet\signalr\dist\browser*  à la *lib* dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="9ed66-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="9ed66-140">Création du Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="9ed66-140">Create the SignalR Hub</span></span>

<span data-ttu-id="9ed66-141">Un concentrateur est une classe qui sert d’un pipeline de haut niveau qui permet au client et le serveur appeler des méthodes sur eux.</span><span class="sxs-lookup"><span data-stu-id="9ed66-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ed66-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ed66-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="9ed66-143">Ajoutez une classe au projet en choisissant **fichier** > **nouveau** > **fichier** et en sélectionnant **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="9ed66-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="9ed66-144">Hériter de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="9ed66-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="9ed66-145">La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que des données d’envoi et de réception.</span><span class="sxs-lookup"><span data-stu-id="9ed66-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="9ed66-146">Créer le `SendMessage` méthode qui envoie un message à tous les clients de conversation connecté.</span><span class="sxs-lookup"><span data-stu-id="9ed66-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="9ed66-147">Notez qu’il renvoie un [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="9ed66-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="9ed66-148">Code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="9ed66-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ed66-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ed66-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="9ed66-150">Ouvrez le *SignalRChat* dossier dans le Code de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9ed66-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="9ed66-151">Ajoutez une classe au projet en sélectionnant **fichier** > **nouveau fichier** à partir du menu.</span><span class="sxs-lookup"><span data-stu-id="9ed66-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="9ed66-152">Hériter de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="9ed66-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="9ed66-153">La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et les groupes, ainsi que d’envoyer et recevoir des données vers les clients.</span><span class="sxs-lookup"><span data-stu-id="9ed66-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="9ed66-154">Ajoutez une méthode `SendMessage` à la classe.</span><span class="sxs-lookup"><span data-stu-id="9ed66-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="9ed66-155">Le `SendMessage` méthode envoie un message à tous les clients de conversation connecté.</span><span class="sxs-lookup"><span data-stu-id="9ed66-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="9ed66-156">Notez qu’il renvoie un [tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="9ed66-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="9ed66-157">Code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="9ed66-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="9ed66-158">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="9ed66-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="9ed66-159">Le serveur de SignalR doit être configuré de sorte qu’il sache pour transmettre les demandes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="9ed66-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="9ed66-160">Pour configurer un projet SignalR, modifiez du projet `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="9ed66-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="9ed66-161">`services.AddSignalR` Ajoute SignalR dans le cadre de la [intergiciel (middleware)](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="9ed66-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="9ed66-162">Configurer des itinéraires à vos concentrateurs à l’aide de `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="9ed66-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="9ed66-163">Créer le code de client SignalR</span><span class="sxs-lookup"><span data-stu-id="9ed66-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="9ed66-164">Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="9ed66-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="9ed66-165">Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="9ed66-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="9ed66-166">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="9ed66-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="9ed66-167">Ajouter un fichier JavaScript nommé *chat.js*, à la *wwwroot\js* dossier.</span><span class="sxs-lookup"><span data-stu-id="9ed66-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="9ed66-168">Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="9ed66-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="9ed66-169">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="9ed66-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9ed66-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9ed66-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="9ed66-171">Sélectionnez **déboguer** > **démarrer sans débogage** pour lancer un navigateur et de charger le site Web localement.</span><span class="sxs-lookup"><span data-stu-id="9ed66-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="9ed66-172">Copiez l’URL de la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="9ed66-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="9ed66-173">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="9ed66-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="9ed66-174">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton.</span><span class="sxs-lookup"><span data-stu-id="9ed66-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="9ed66-175">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="9ed66-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9ed66-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9ed66-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="9ed66-177">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="9ed66-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="9ed66-178">Le programme en cours d’exécution, une fenêtre de navigateur s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="9ed66-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="9ed66-179">Ouvrir une autre fenêtre de navigateur et de charger le site Web localement dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="9ed66-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="9ed66-180">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton.</span><span class="sxs-lookup"><span data-stu-id="9ed66-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="9ed66-181">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="9ed66-181">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Solution](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="9ed66-183">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="9ed66-183">Related resources</span></span>

[<span data-ttu-id="9ed66-184">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="9ed66-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)