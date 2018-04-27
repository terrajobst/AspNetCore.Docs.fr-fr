---
title: Prise en main SignalR sur ASP.NET Core
author: rachelappel
description: Dans ce didacticiel, vous créez une application à l’aide de SignalR pour ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: 03735359bb22cc3085ddc7b34372ecfc9501a940
ms.sourcegitcommit: 07903a1be39a99dcf538d57981161592d0e658b8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="cbad7-103">Prise en main SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cbad7-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="cbad7-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="cbad7-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="cbad7-105">Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cbad7-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="cbad7-107">Ce didacticiel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="cbad7-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cbad7-108">Créez un SignalR sur l’application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cbad7-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="cbad7-109">Créer un concentrateur SignalR pour transmettre le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="cbad7-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="cbad7-110">Modifier la `Startup` classe et de configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="cbad7-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="cbad7-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cbad7-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="cbad7-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="cbad7-112">Prerequisites</span></span>

<span data-ttu-id="cbad7-113">Installer les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="cbad7-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cbad7-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cbad7-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cbad7-115">[Afficher un aperçu de .NET core 2.1.0 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cbad7-115">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* <span data-ttu-id="cbad7-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,7 ou version ultérieure avec le **développement web ASP.NET et** la charge de travail</span><span class="sxs-lookup"><span data-stu-id="cbad7-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="cbad7-117">npm</span><span class="sxs-lookup"><span data-stu-id="cbad7-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cbad7-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cbad7-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="cbad7-119">[Afficher un aperçu de .NET core 2.1.0 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="cbad7-119">[.NET Core 2.1.0 Preview 2 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview2) or later</span></span>
* [<span data-ttu-id="cbad7-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cbad7-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="cbad7-121">C# pour le Code de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cbad7-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="cbad7-122">npm</span><span class="sxs-lookup"><span data-stu-id="cbad7-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="cbad7-123">Créez un projet ASP.NET Core qui héberge le serveur et client SignalR</span><span class="sxs-lookup"><span data-stu-id="cbad7-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cbad7-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cbad7-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="cbad7-125">Utilisez le **fichier** > **nouveau projet** menu option et choisissez **Application ASP.NET Core Web**.</span><span class="sxs-lookup"><span data-stu-id="cbad7-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="cbad7-126">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="cbad7-126">Name the project *SignalRChat*.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="cbad7-128">Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="cbad7-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="cbad7-129">Puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbad7-129">Then select **OK**.</span></span> <span data-ttu-id="cbad7-130">Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, si SignalR s’exécute sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="cbad7-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="cbad7-132">Visual Studio inclut la `Microsoft.AspNetCore.SignalR` package contenant ses bibliothèques du serveur dans le cadre de son **Application ASP.NET Core Web** modèle.</span><span class="sxs-lookup"><span data-stu-id="cbad7-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="cbad7-133">Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="cbad7-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="cbad7-134">Exécutez les commandes suivantes le **Package Manager Console** fenêtre, à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="cbad7-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm init -y
      npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="cbad7-135">Copie le *signalr.js* à partir du fichier *node_modules\\ @aspnet\signalr\dist\browser*  à la *lib* dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="cbad7-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cbad7-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cbad7-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="cbad7-137">À partir de la **Terminal Server intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="cbad7-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="cbad7-138">Installer la bibliothèque cliente JavaScript à l’aide *npm*.</span><span class="sxs-lookup"><span data-stu-id="cbad7-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="cbad7-139">Création du Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="cbad7-139">Create the SignalR Hub</span></span>

<span data-ttu-id="cbad7-140">Un concentrateur est une classe qui sert d’un pipeline de haut niveau qui permet au client et le serveur appeler des méthodes sur eux.</span><span class="sxs-lookup"><span data-stu-id="cbad7-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cbad7-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cbad7-141">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="cbad7-142">Ajoutez une classe au projet en choisissant **fichier** > **nouveau** > **fichier** et en sélectionnant **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="cbad7-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="cbad7-143">Hériter de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="cbad7-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="cbad7-144">La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que des données d’envoi et de réception.</span><span class="sxs-lookup"><span data-stu-id="cbad7-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="cbad7-145">Créer le `SendMessage` méthode qui envoie un message à tous les clients de conversation connecté.</span><span class="sxs-lookup"><span data-stu-id="cbad7-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="cbad7-146">Notez qu’il renvoie un [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cbad7-146">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="cbad7-147">Code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="cbad7-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cbad7-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cbad7-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="cbad7-149">Ouvrez le *SignalRChat* dossier dans le Code de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cbad7-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="cbad7-150">Ajoutez une classe au projet en sélectionnant **fichier** > **nouveau fichier** à partir du menu.</span><span class="sxs-lookup"><span data-stu-id="cbad7-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="cbad7-151">Hériter de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="cbad7-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="cbad7-152">La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et les groupes, ainsi que d’envoyer et recevoir des données vers les clients.</span><span class="sxs-lookup"><span data-stu-id="cbad7-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="cbad7-153">Ajoutez une méthode `SendMessage` à la classe.</span><span class="sxs-lookup"><span data-stu-id="cbad7-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="cbad7-154">Le `SendMessage` méthode envoie un message à tous les clients de conversation connecté.</span><span class="sxs-lookup"><span data-stu-id="cbad7-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="cbad7-155">Notez qu’il renvoie un [tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="cbad7-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="cbad7-156">Code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="cbad7-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="cbad7-157">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="cbad7-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="cbad7-158">Le serveur de SignalR doit être configuré de sorte qu’il sache pour transmettre les demandes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="cbad7-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="cbad7-159">Pour configurer un projet SignalR, modifiez du projet `Startup.ConfigureServices` (méthode).</span><span class="sxs-lookup"><span data-stu-id="cbad7-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="cbad7-160">`services.AddSignalR` Ajoute SignalR dans le cadre de la [intergiciel (middleware)](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="cbad7-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="cbad7-161">Configurer des itinéraires à vos concentrateurs à l’aide de `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="cbad7-161">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=36,56-59)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="cbad7-162">Créer le code de client SignalR</span><span class="sxs-lookup"><span data-stu-id="cbad7-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="cbad7-163">Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cbad7-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="cbad7-164">Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="cbad7-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="cbad7-165">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="cbad7-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="cbad7-166">Ajouter un fichier JavaScript nommé *chat.js*, à la *wwwroot\js* dossier.</span><span class="sxs-lookup"><span data-stu-id="cbad7-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="cbad7-167">Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="cbad7-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="cbad7-168">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="cbad7-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cbad7-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cbad7-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="cbad7-170">Sélectionnez **déboguer** > **démarrer sans débogage** pour lancer un navigateur et de charger le site Web localement.</span><span class="sxs-lookup"><span data-stu-id="cbad7-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="cbad7-171">Copiez l’URL de la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="cbad7-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="cbad7-172">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="cbad7-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="cbad7-173">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton.</span><span class="sxs-lookup"><span data-stu-id="cbad7-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="cbad7-174">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="cbad7-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="cbad7-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="cbad7-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="cbad7-176">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="cbad7-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="cbad7-177">Le programme en cours d’exécution, une fenêtre de navigateur s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="cbad7-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="cbad7-178">Ouvrir une autre fenêtre de navigateur et de charger le site Web localement dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="cbad7-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="cbad7-179">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton.</span><span class="sxs-lookup"><span data-stu-id="cbad7-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="cbad7-180">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="cbad7-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Solution](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="cbad7-182">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="cbad7-182">Related resources</span></span>

[<span data-ttu-id="cbad7-183">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="cbad7-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)