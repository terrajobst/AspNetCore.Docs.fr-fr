---
title: Prise en main de SignalR sur ASP.NET Core
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
ms.openlocfilehash: c71d98f86c15a4c6fbbe400f912123419b4ad076
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252202"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="8b554-103">Prise en main de SignalR sur ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b554-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="8b554-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8b554-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="8b554-105">Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b554-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="8b554-107">Ce didacticiel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="8b554-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8b554-108">Créer un SignalR sur une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b554-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="8b554-109">Créer un hub SignalR pour envoyer du contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="8b554-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="8b554-110">Modifier la classe `Startup` et configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="8b554-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="8b554-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8b554-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="8b554-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="8b554-112">Prerequisites</span></span>

<span data-ttu-id="8b554-113">Installez les logiciels suivants :
</span><span class="sxs-lookup"><span data-stu-id="8b554-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b554-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b554-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="8b554-115">.NET core SDK 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="8b554-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8b554-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 ou version ultérieure avec la charge de travail **Développement ASP.NET et web**</span><span class="sxs-lookup"><span data-stu-id="8b554-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8b554-117">npm</span><span class="sxs-lookup"><span data-stu-id="8b554-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b554-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b554-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="8b554-119">.NET core SDK 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="8b554-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="8b554-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b554-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="8b554-121">C# pour le Code de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b554-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="8b554-122">npm</span><span class="sxs-lookup"><span data-stu-id="8b554-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="8b554-123">Créer un projet ASP.NET Core qui héberge le serveur et le client SignalR</span><span class="sxs-lookup"><span data-stu-id="8b554-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b554-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b554-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="8b554-125">Utilisez l'option de menu **Fichier** > **Nouveau projet** et choisissez **Application ASP.NET Core Web**.</span><span class="sxs-lookup"><span data-stu-id="8b554-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8b554-126">Nommez le projet *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="8b554-126">Name the project *SignalRChat*.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="8b554-128">Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="8b554-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="8b554-129">Puis sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="8b554-129">Then select **OK**.</span></span> <span data-ttu-id="8b554-130">Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, même si SignalR peut s’exécuter sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="8b554-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="8b554-132">Visual Studio inclut le package `Microsoft.AspNetCore.SignalR` contenant ses bibliothèques du serveur dans son modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8b554-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="8b554-133">Toutefois, la bibliothèque cliente JavaScript pour SignalR doit être installée à l’aide de *npm*.</span><span class="sxs-lookup"><span data-stu-id="8b554-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="8b554-134">Exécutez les commandes suivantes dans la fenêtre **Console du gestionnaire de package**, à partir de la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="8b554-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="8b554-135">Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="8b554-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="8b554-136">Copiez le fichier *signalr.js* à partir de *node_modules\\ @aspnet\signalr\dist\browser*  dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="8b554-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b554-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b554-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="8b554-138">À partir de la **Terminal Server intégré**, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8b554-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="8b554-139">Installez la bibliothèque cliente JavaScript en utilisant *npm*.</span><span class="sxs-lookup"><span data-stu-id="8b554-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="8b554-140">Créez un dossier nommé « signalr » à l’intérieur du dossier *lib* dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="8b554-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="8b554-141">Copiez le fichier *signalr.js* à partir de *node_modules\\ @aspnet\signalr\dist\browser*  dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="8b554-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="8b554-142">Création du Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="8b554-142">Create the SignalR Hub</span></span>

<span data-ttu-id="8b554-143">Un hub est une classe servant de pipeline de haut niveau qui permet au client et au serveur d'appeler des méthodes l’un sur l’autre.</span><span class="sxs-lookup"><span data-stu-id="8b554-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b554-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b554-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="8b554-145">Ajoutez une classe au projet en choisissant **Fichier** > **Nouveau** > **Fichier** et en sélectionnant **Classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="8b554-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="8b554-146">Nommez le fichier *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="8b554-146">Name the file *ChatHub*.</span></span> 

2. <span data-ttu-id="8b554-147">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="8b554-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="8b554-148">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que pour l'envoi et la réception des données.</span><span class="sxs-lookup"><span data-stu-id="8b554-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="8b554-149">Créez la méthode `SendMessage` qui envoie un message à tous les clients connectés à la conversation.</span><span class="sxs-lookup"><span data-stu-id="8b554-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="8b554-150">Notez qu’elle renvoie une [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="8b554-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="8b554-151">Le code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="8b554-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b554-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b554-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="8b554-153">Ouvrez le dossier *SignalRChat* dans Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8b554-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="8b554-154">Ajoutez une classe au projet en sélectionnant **Fichier** > **Nouveau fichier** à partir du menu.</span><span class="sxs-lookup"><span data-stu-id="8b554-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="8b554-155">Héritez de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="8b554-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="8b554-156">La classe `Hub` contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que pour l'envoi et la réception des données à des clients.</span><span class="sxs-lookup"><span data-stu-id="8b554-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="8b554-157">Ajoutez une méthode `SendMessage` à la classe.</span><span class="sxs-lookup"><span data-stu-id="8b554-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="8b554-158">La méthode `SendMessage` envoie un message à tous les clients connectés à la conversation.</span><span class="sxs-lookup"><span data-stu-id="8b554-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="8b554-159">Notez qu’elle renvoie une [tâche](/dotnet/api/system.threading.tasks.task), car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="8b554-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="8b554-160">Le code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="8b554-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="8b554-161">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="8b554-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="8b554-162">Le serveur de SignalR doit être configuré de sorte qu’il sache transmettre les demandes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b554-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="8b554-163">Pour configurer un projet SignalR, modifiez la méthode `Startup.ConfigureServices` du projet.</span><span class="sxs-lookup"><span data-stu-id="8b554-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="8b554-164">`services.AddSignalR` SignalR dans le cadre du pipeline de l'[intergiciel (middleware)](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="8b554-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="8b554-165">Configurez des routes pour vos hubs en utilisant `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="8b554-165">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="8b554-166">Créer le code de client SignalR</span><span class="sxs-lookup"><span data-stu-id="8b554-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="8b554-167">Ajoutez un fichier JavaScript nommé *chat.js* au dossier *wwwroot\js*. Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b554-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="8b554-168">Ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b554-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="8b554-169">Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="8b554-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="8b554-170">Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="8b554-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="8b554-171">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="8b554-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>


## <a name="run-the-app"></a><span data-ttu-id="8b554-172">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="8b554-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8b554-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8b554-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="8b554-174">Sélectionnez **Débogage** > **Démarrer sans débogage** pour lancer un navigateur et charger le site web localement.</span><span class="sxs-lookup"><span data-stu-id="8b554-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="8b554-175">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="8b554-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="8b554-176">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="8b554-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="8b554-177">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="8b554-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="8b554-178">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="8b554-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8b554-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8b554-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="8b554-180">Appuyez sur **Débogage** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="8b554-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="8b554-181">L’exécution du programme ouvre une fenêtre de navigateur.</span><span class="sxs-lookup"><span data-stu-id="8b554-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="8b554-182">Ouvrir une autre fenêtre de navigateur et de charger le site Web localement dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="8b554-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="8b554-183">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="8b554-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="8b554-184">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="8b554-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Solution](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="8b554-186">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="8b554-186">Related resources</span></span>

[<span data-ttu-id="8b554-187">Introduction à ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8b554-187">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
