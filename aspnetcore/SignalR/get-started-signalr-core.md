---
uid: signalr/get-started-signalr-core
title: Prise en main SignalR sur ASP.NET Core
author: rachelappel
ms.author: rachelap
description: "Dans ce didacticiel, vous créez une application à l’aide de SignalR pour ASP.NET Core."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
ms.openlocfilehash: 5e16569aa492e3639aa97abd241610b361fb0c56
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="671c6-103">Didacticiel : Prise en main SignalR pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="671c6-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="671c6-104">Par [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="671c6-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="671c6-105">Ce didacticiel explique les principes fondamentaux de la création d’une application en temps réel à l’aide de SignalR pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="671c6-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Solution](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="671c6-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="671c6-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="671c6-108">Ce didacticiel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="671c6-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="671c6-109">Créer une application de web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="671c6-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="671c6-110">Créer un concentrateur SignalR pour transmettre le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="671c6-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="671c6-111">Utilisez la bibliothèque SignalR JavaScript pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="671c6-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="671c6-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="671c6-112">Prerequisites</span></span>

<span data-ttu-id="671c6-113">Installer les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="671c6-113">Install the following software:</span></span>

* <span data-ttu-id="671c6-114">[Afficher un aperçu de .NET core 2.1.0 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="671c6-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="671c6-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,6 ou version ultérieure avec la charge de travail de développement ASP.NET et web</span><span class="sxs-lookup"><span data-stu-id="671c6-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="671c6-116">npm</span><span class="sxs-lookup"><span data-stu-id="671c6-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="671c6-117">Créez un projet ASP.NET Core qui héberge le serveur et client SignalR</span><span class="sxs-lookup"><span data-stu-id="671c6-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="671c6-118">Utilisez le **fichier** > **nouveau projet** menu option et choisissez **Application ASP.NET Core Web**.</span><span class="sxs-lookup"><span data-stu-id="671c6-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="671c6-119">Attribuez un nom au projet `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="671c6-119">Name the project `SignalRChat`.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="671c6-121">Sélectionnez **Web Application** pour créer un projet à l’aide des Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="671c6-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="671c6-122">Puis sélectionnez **Ok**.</span><span class="sxs-lookup"><span data-stu-id="671c6-122">Then select **Ok**.</span></span> <span data-ttu-id="671c6-123">Assurez-vous que **ASP.NET Core 2.1** est sélectionné dans le sélecteur de framework, si SignalR s’exécute sur des versions antérieures de .NET.</span><span class="sxs-lookup"><span data-stu-id="671c6-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Boîte de dialogue Nouveau projet dans Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="671c6-125">Les bibliothèques qui hébergent le code de SignalR côté serveur sont inclus dans le modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="671c6-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="671c6-126">Installer le JavaScript côté client séparément avec [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="671c6-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="671c6-127">Copie le *signalr.js* de *node_modules\\ @aspnet\signalr\dist\browser*  à la *wwwroot\lib* dossier dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="671c6-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="671c6-128">Création du Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="671c6-128">Create the SignalR Hub</span></span>

<span data-ttu-id="671c6-129">Un concentrateur est une classe qui sert d’un pipeline de haut niveau qui permet au client et le serveur appeler des méthodes sur eux.</span><span class="sxs-lookup"><span data-stu-id="671c6-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="671c6-130">Ajoutez une classe au projet en choisissant **fichier** > **nouveau** > **fichier** et en sélectionnant **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="671c6-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="671c6-131">Hériter de `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="671c6-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="671c6-132">La `Hub` classe contient des propriétés et des événements pour la gestion des connexions et des groupes, ainsi que des données d’envoi et de réception.</span><span class="sxs-lookup"><span data-stu-id="671c6-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="671c6-133">Créer le `Send` méthode qui envoie un message à tous les clients de conversation connecté.</span><span class="sxs-lookup"><span data-stu-id="671c6-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="671c6-134">Notez qu’il renvoie un `Task`, car SignalR est asynchrone.</span><span class="sxs-lookup"><span data-stu-id="671c6-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="671c6-135">Code asynchrone évolue mieux.</span><span class="sxs-lookup"><span data-stu-id="671c6-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="671c6-136">Configurer le projet pour utiliser SignalR</span><span class="sxs-lookup"><span data-stu-id="671c6-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="671c6-137">Le serveur de SignalR doit être configuré de sorte qu’il sache pour transmettre les demandes à SignalR.</span><span class="sxs-lookup"><span data-stu-id="671c6-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="671c6-138">Pour configurer un projet SignalR, modifiez le `ConfigureServices` (méthode) de l’application `Startup` classe en insérant un appel à `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="671c6-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="671c6-139">`services.AddSignalR` Ajoute SignalR dans le cadre de la [intergiciel (middleware) ASP.NET Core](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="671c6-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="671c6-140">Configurer des itinéraires à vos concentrateurs à l’aide de `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="671c6-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="671c6-141">Créer le code de client SignalR</span><span class="sxs-lookup"><span data-stu-id="671c6-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="671c6-142">Remplacez le contenu de *Pages\Index.cshtml* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="671c6-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="671c6-143">Le code HTML précédent affiche le nom et les champs de message et un bouton d’envoi.</span><span class="sxs-lookup"><span data-stu-id="671c6-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="671c6-144">Notez les références de script en bas : une référence à SignalR et *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="671c6-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="671c6-145">Ajouter un fichier JavaScript à la *wwwroot\js* dossier nommé *chat.js* et ajoutez-y le code suivant :</span><span class="sxs-lookup"><span data-stu-id="671c6-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="671c6-146">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="671c6-146">Run the app</span></span>

1. <span data-ttu-id="671c6-147">Sélectionnez **déboguer** > **démarrer sans débogage** pour lancer un navigateur et de charger le site Web localement.</span><span class="sxs-lookup"><span data-stu-id="671c6-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="671c6-148">Copiez l’URL de la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="671c6-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="671c6-149">Ouvrez une autre instance du navigateur (n’importe quel navigateur) et collez l’URL dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="671c6-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="671c6-150">Choisissez un navigateur, entrez un nom et un message, puis cliquez sur le **envoyer** bouton.</span><span class="sxs-lookup"><span data-stu-id="671c6-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="671c6-151">Le nom et le message sont affichés sur les deux pages instantanément.</span><span class="sxs-lookup"><span data-stu-id="671c6-151">The name and message are displayed on both pages instantly.</span></span>

  ![Solution](get-started-signalr-core/_static/signalr-get-started-finished.png)
