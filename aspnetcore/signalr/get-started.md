---
title: Prise en main SignalR sur ASP.NET Core
author: rachelappel
description: Dans ce didacticiel, vous créez une application à l’aide de SignalR pour ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: ba1db640e5608fd9f5e7fa024283a651bf7772c2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819056"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="a6d69-103">Prise en main SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6d69-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="a6d69-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="a6d69-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="a6d69-105">Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6d69-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="a6d69-107">Ce didacticiel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="a6d69-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6d69-108">Créez un SignalR sur l’application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6d69-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="a6d69-109">Créer un concentrateur SignalR pour transmettre le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="a6d69-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="a6d69-110">Modifier la `Startup` classe et de configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="a6d69-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="a6d69-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a6d69-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="a6d69-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="a6d69-112">Prerequisites</span></span>

<span data-ttu-id="a6d69-113">Installer les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="a6d69-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6d69-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6d69-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="a6d69-115">.NET core SDK 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="a6d69-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a6d69-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,7 ou version ultérieure avec le **développement web ASP.NET et** la charge de travail</span><span class="sxs-lookup"><span data-stu-id="a6d69-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a6d69-117">npm</span><span class="sxs-lookup"><span data-stu-id="a6d69-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6d69-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6d69-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a6d69-119">.NET core SDK 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="a6d69-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a6d69-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6d69-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a6d69-121">C# pour le Code de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6d69-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="a6d69-122">npm</span><span class="sxs-lookup"><span data-stu-id="a6d69-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="a6d69-123">Créez un projet ASP.NET Core qui héberge le serveur et client SignalR</span><span class="sxs-lookup"><span data-stu-id="a6d69-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6d69-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6d69-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a6d69-125">Utilisez le **fichier** > **nouveau projet** menu option et choisissez **Application ASP.NET Core Web**.</span><span class="sxs-lookup"><span data-stu-id="a6d69-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a6d69-126">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="a6d69-126">Name the project *SignalRChat*.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="a6d69-128">Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="a6d69-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="a6d69-129">Puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6d69-129">Then select **OK**.</span></span> <span data-ttu-id="a6d69-130">Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, si SignalR s’exécute sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="a6d69-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="a6d69-132">Visual Studio inclut la `Microsoft.AspNetCore.SignalR` package contenant ses bibliothèques du serveur dans le cadre de son **Application ASP.NET Core Web** modèle.</span><span class="sxs-lookup"><span data-stu-id="a6d69-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="a6d69-133">Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="a6d69-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="a6d69-134">Exécutez les commandes suivantes le **Package Manager Console** fenêtre, à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="a6d69-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="a6d69-135">Créez un dossier nommé « signalr » à l’intérieur de la *lib* dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="a6d69-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="a6d69-136">Copie le *signalr.js* à partir du fichier *node_modules\\ @aspnet\signalr\dist\browser*  dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="a6d69-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6d69-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6d69-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a6d69-138">À partir de la **Terminal Server intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="a6d69-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

2. <span data-ttu-id="a6d69-139">Installer la bibliothèque cliente JavaScript à l’aide *npm*.</span><span class="sxs-lookup"><span data-stu-id="a6d69-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="a6d69-140">Créez un dossier nommé « signalr » à l’intérieur de la *lib* dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="a6d69-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="a6d69-141">Copie le *signalr.js* à partir du fichier *node_modules\\ @aspnet\signalr\dist\browser*  dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="a6d69-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="a6d69-142">Création du Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="a6d69-142">Create the SignalR Hub</span></span>

<span data-ttu-id="a6d69-143">Un concentrateur est une classe qui sert d’un pipeline de haut niveau qui permet au client et le serveur appeler des méthodes sur eux.</span><span class="sxs-lookup"><span data-stu-id="a6d69-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6d69-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6d69-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="a6d69-145">Ajoutez une classe au projet en choisissant **fichier** > **nouveau** > **fichier** et en sélectionnant **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="a6d69-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="a6d69-146">Nommez le fichier *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="a6d69-146">Name the file *ChatHub*.</span></span> 

2. <span data-ttu-id="a6d69-147">Hériter de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="a6d69-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a6d69-148">La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que des données d’envoi et de réception.</span><span class="sxs-lookup"><span data-stu-id="a6d69-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="a6d69-149">Créer le `SendMessage` méthode qui envoie un message à tous les clients de conversation connecté.</span><span class="sxs-lookup"><span data-stu-id="a6d69-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="a6d69-150">Notez qu’il renvoie un [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a6d69-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="a6d69-151">Code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="a6d69-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6d69-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6d69-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="a6d69-153">Ouvrez le *SignalRChat* dossier dans le Code de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6d69-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="a6d69-154">Ajoutez une classe au projet en sélectionnant **fichier** > **nouveau fichier** à partir du menu.</span><span class="sxs-lookup"><span data-stu-id="a6d69-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="a6d69-155">Hériter de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="a6d69-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="a6d69-156">La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et les groupes, ainsi que d’envoyer et recevoir des données vers les clients.</span><span class="sxs-lookup"><span data-stu-id="a6d69-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="a6d69-157">Ajoutez une méthode `SendMessage` à la classe.</span><span class="sxs-lookup"><span data-stu-id="a6d69-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="a6d69-158">Le `SendMessage` méthode envoie un message à tous les clients de conversation connecté.</span><span class="sxs-lookup"><span data-stu-id="a6d69-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="a6d69-159">Notez qu’il renvoie un [tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="a6d69-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="a6d69-160">Code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="a6d69-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="a6d69-161">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="a6d69-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="a6d69-162">Le serveur de SignalR doit être configuré de sorte qu’il sache pour transmettre les demandes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="a6d69-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="a6d69-163">Pour configurer un projet SignalR, modifiez du projet `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="a6d69-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="a6d69-164">`services.AddSignalR` Ajoute SignalR dans le cadre de la [intergiciel (middleware)](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="a6d69-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="a6d69-165">Configurer des itinéraires à vos concentrateurs à l’aide de `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="a6d69-165">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="a6d69-166">Créer le code de client SignalR</span><span class="sxs-lookup"><span data-stu-id="a6d69-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="a6d69-167">Ajouter un fichier JavaScript nommé *chat.js*, à la *wwwroot\js* dossier.</span><span class="sxs-lookup"><span data-stu-id="a6d69-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="a6d69-168">Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a6d69-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="a6d69-169">Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="a6d69-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="a6d69-170">Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="a6d69-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="a6d69-171">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="a6d69-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>


## <a name="run-the-app"></a><span data-ttu-id="a6d69-172">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="a6d69-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6d69-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6d69-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a6d69-174">Sélectionnez **déboguer** > **démarrer sans débogage** pour lancer un navigateur et de charger le site Web localement.</span><span class="sxs-lookup"><span data-stu-id="a6d69-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="a6d69-175">Copiez l’URL de la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="a6d69-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="a6d69-176">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="a6d69-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="a6d69-177">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton.</span><span class="sxs-lookup"><span data-stu-id="a6d69-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a6d69-178">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="a6d69-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6d69-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6d69-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a6d69-180">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="a6d69-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="a6d69-181">Le programme en cours d’exécution, une fenêtre de navigateur s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="a6d69-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="a6d69-182">Ouvrir une autre fenêtre de navigateur et de charger le site Web localement dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="a6d69-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="a6d69-183">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton.</span><span class="sxs-lookup"><span data-stu-id="a6d69-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="a6d69-184">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="a6d69-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Solution](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="a6d69-186">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="a6d69-186">Related resources</span></span>

[<span data-ttu-id="a6d69-187">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a6d69-187">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
