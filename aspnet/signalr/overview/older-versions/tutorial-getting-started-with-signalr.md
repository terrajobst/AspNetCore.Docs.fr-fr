---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "Didacticiel : Mise en route avec SignalR 1.x | Documents Microsoft"
author: pfletcher
description: "Utilisez ASP.NET SignalR pour générer une application de conversation en temps réel dans une page HTML."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c61be6f7a64c000c8d9489f35eea520fd0bb32dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="fc7dd-103">Didacticiel : Mise en route avec SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="fc7dd-103">Tutorial: Getting Started with SignalR 1.x</span></span>
====================
<span data-ttu-id="fc7dd-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="fc7dd-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="fc7dd-105">Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="fc7dd-106">Vous ajoutez SignalR à l’application web ASP.NET vide et créer une page HTML pour envoyer et afficher les messages.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="fc7dd-107">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="fc7dd-107">Overview</span></span>

<span data-ttu-id="fc7dd-108">Ce didacticiel présente le développement de SignalR en montrant comment créer une application simple conversation basée sur navigateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="fc7dd-109">Vous ajoute la bibliothèque SignalR à l’application web ASP.NET vide, créez une classe de concentrateur pour envoyer des messages aux clients et créer une page HTML qui permet aux utilisateurs d’envoyer et recevoir des messages de conversation.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="fc7dd-110">Pour obtenir un didacticiel similaire qui montre comment créer une application de conversation dans MVC 4 à l’aide d’une vue MVC, consultez [prise en main SignalR et MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="fc7dd-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fc7dd-111">Ce didacticiel utilise la version (1.x) de SignalR.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="fc7dd-112">Pour plus d’informations sur les modifications entre SignalR 1.x et 2.0, consultez [la mise à niveau de SignalR 1.x projets](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="fc7dd-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="fc7dd-113">SignalR est une bibliothèque de .NET open source pour la création d’applications web qui requièrent une intervention de l’utilisateur en direct ou mises à jour des données en temps réel.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="fc7dd-114">Exemples applications sociales, jeux multi-utilisateur, météo de collaboration et de news, entreprise ou des applications de mise à jour financière.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="fc7dd-115">Il s’agit souvent d’applications en temps réel.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-115">These are often called real-time applications.</span></span>

<span data-ttu-id="fc7dd-116">SignalR simplifie le processus de création d’applications en temps réel.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="fc7dd-117">Elle inclut une bibliothèque de serveur ASP.NET et d’une bibliothèque cliente de JavaScript pour le rendre plus facile à gérer les connexions client-serveur et d’envoyer des mises à jour de contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="fc7dd-118">Vous pouvez ajouter la bibliothèque SignalR pour une application ASP.NET existante pour bénéficier de fonctionnalités en temps réel.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="fc7dd-119">Le didacticiel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="fc7dd-120">Ajout de la bibliothèque de SignalR pour une application web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="fc7dd-121">Création d’une classe de concentrateur pour transmettre le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="fc7dd-122">À l’aide de la bibliothèque jQuery SignalR dans une page web pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="fc7dd-123">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="fc7dd-124">Chaque nouvel utilisateur peut publier des commentaires et voir les commentaires ajoutés après que l’utilisateur joint à la conversation.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instances de conversation](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="fc7dd-126">Sections :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-126">Sections:</span></span>

- [<span data-ttu-id="fc7dd-127">Le projet</span><span class="sxs-lookup"><span data-stu-id="fc7dd-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="fc7dd-128">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="fc7dd-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="fc7dd-129">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="fc7dd-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="fc7dd-130">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fc7dd-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="fc7dd-131">Le projet</span><span class="sxs-lookup"><span data-stu-id="fc7dd-131">Set up the Project</span></span>

<span data-ttu-id="fc7dd-132">Cette section montre comment créer une application web ASP.NET vide, ajoutez SignalR et créer l’application de la conversation.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="fc7dd-133">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-133">Prerequisites:</span></span>

- <span data-ttu-id="fc7dd-134">Visual Studio 2010 SP1 ou 2012.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="fc7dd-135">Si vous n’avez pas Visual Studio, consultez [téléchargements ASP.NET](https://www.asp.net/downloads) pour obtenir le Visual Studio 2012 Express outil de développement gratuit.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="fc7dd-136">[Microsoft ASP.NET et Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="fc7dd-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="fc7dd-137">Pour Visual Studio 2012, ce programme d’installation ajoute de nouvelles fonctionnalités d’ASP.NET, y compris les modèles de SignalR pour Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="fc7dd-138">Pour Visual Studio 2010 SP1, un programme d’installation n’est pas disponible, mais vous pouvez terminer le didacticiel en installant le package NuGet de SignalR, comme décrit dans les étapes d’installation.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="fc7dd-139">Les étapes suivantes utilisent Visual Studio 2012 pour créer une Application de Web ASP.NET vide et ajouter la bibliothèque SignalR :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="fc7dd-140">Dans Visual Studio, créez une Application de Web ASP.NET vide.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Créer le site web vide](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="fc7dd-142">Ouvrez le **Package Manager Console** en sélectionnant **outils | Gestionnaire de Package de bibliothèque | Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-142">Open the **Package Manager Console** by selecting **Tools | Library Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="fc7dd-143">Entrez la commande suivante dans la fenêtre de console :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="fc7dd-144">Cette commande installe la version la plus récente de SignalR 1.x.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="fc7dd-145">Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter | Classe**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="fc7dd-146">Nommez la nouvelle classe **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="fc7dd-147">Dans **l’Explorateur de solutions** développez le nœud Scripts.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="fc7dd-148">Bibliothèques de scripts de jQuery et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Références de bibliothèque](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="fc7dd-150">Remplacez le code dans le **ChatHub** classe par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="fc7dd-151">Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="fc7dd-152">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **classe d’Application globale** et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Ajouter global](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="fc7dd-154">Ajoutez le code suivant `using` instructions après avoir fourni `using` instructions dans la classe Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="fc7dd-155">Ajoutez la ligne suivante de code dans la `Application_Start` méthode de la classe globale pour inscrire l’itinéraire par défaut pour les concentrateurs SignalR.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="fc7dd-156">Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="fc7dd-157">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez Html Page, cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="fc7dd-158">Dans **l’Explorateur de solutions**, avec le bouton droit de la page HTML que vous venez de créer, cliquez sur **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="fc7dd-159">Remplacez le code par défaut dans la page HTML par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="fc7dd-160">**Enregistrer tous les** pour le projet.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="fc7dd-161">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="fc7dd-161">Run the Sample</span></span>

1. <span data-ttu-id="fc7dd-162">Appuyez sur F5 pour exécuter le projet en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="fc7dd-163">Charge de la page HTML dans une instance du navigateur et les invite à entrer un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="fc7dd-165">Entrez un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-165">Enter a user name.</span></span>
3. <span data-ttu-id="fc7dd-166">Copiez l’URL de la ligne d’adresse du navigateur et l’utiliser pour ouvrir les deux instances du navigateur plus.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="fc7dd-167">Dans chaque instance du navigateur, entrez un nom d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="fc7dd-168">Dans chaque instance du navigateur, ajoutez un commentaire, puis cliquez sur **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="fc7dd-169">Les commentaires doivent s’afficher dans toutes les instances du navigateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fc7dd-170">Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="fc7dd-171">Le concentrateur diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="fc7dd-172">Les utilisateurs qui participer à la conversation ultérieurement voient des messages ajoutés à partir du moment qu'ils se connectent.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="fc7dd-173">La capture d’écran suivante montre l’application de la conversation en cours d’exécution dans les trois instances de navigateur, qui sont mis à jour lorsqu’une instance envoie un message :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="fc7dd-175">Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="fc7dd-176">Il existe un fichier de script nommé **concentrateurs** que la bibliothèque SignalR génère dynamiquement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="fc7dd-177">Ce fichier gère la communication entre le script jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script de concentrateur généré](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="fc7dd-179">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="fc7dd-179">Examine the Code</span></span>

<span data-ttu-id="fc7dd-180">L’application de la conversation SignalR illustre deux tâches de développement SignalR base : création d’un hub en tant que l’objet principal de coordination sur le serveur et à l’aide de la bibliothèque jQuery SignalR pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="fc7dd-181">Concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="fc7dd-181">SignalR Hubs</span></span>

<span data-ttu-id="fc7dd-182">Dans l’exemple de code la **ChatHub** classe dérive de la **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="fc7dd-183">Dérivation à partir de la **Hub** classe est une méthode utile pour générer une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="fc7dd-184">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et puis accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page web.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="fc7dd-185">Dans le code de la conversation, les clients appellent le **ChatHub.Send** méthode pour envoyer un message.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="fc7dd-186">Le concentrateur à son tour envoie le message à tous les clients en appelant **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="fc7dd-187">Le **envoyer** méthode illustre plusieurs concepts de hub :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="fc7dd-188">Déclarer des méthodes publiques sur un concentrateur, afin que les clients peuvent appeler les.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="fc7dd-189">Utilisez le **Microsoft.AspNet.SignalR.Hub.Clients** propriétés dynamiques pour accéder à tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="fc7dd-190">Appeler une fonction de jQuery sur le client (telles que le `broadcastMessage` (fonction)) pour mettre à jour des clients.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="fc7dd-191">SignalR et jQuery</span><span class="sxs-lookup"><span data-stu-id="fc7dd-191">SignalR and jQuery</span></span>

<span data-ttu-id="fc7dd-192">La page HTML dans l’exemple de code montre comment utiliser la bibliothèque jQuery SignalR pour communiquer avec un concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="fc7dd-193">Les tâches essentielles dans le code déclarez un proxy pour référencer le concentrateur, déclarez une fonction qui peut appeler le serveur pour envoyer aux clients et démarrage d’une connexion pour envoyer des messages au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="fc7dd-194">Le code suivant déclare un proxy pour un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="fc7dd-195">Dans jQuery la référence à la classe de serveur et de ses membres est en casse mixte.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="fc7dd-196">L’exemple de code fait référence à c# **ChatHub** jQuery en tant que classe **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>


<span data-ttu-id="fc7dd-197">Le code suivant est la façon dont vous créez une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="fc7dd-198">La classe de concentrateur sur le serveur appelle cette fonction pour transmettre les mises à jour à chaque client.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="fc7dd-199">Les deux lignes que HTML encoder le contenu avant de les afficher sont facultatifs et affichent un moyen simple pour empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="fc7dd-200">Le code suivant montre comment ouvrir une connexion avec le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="fc7dd-201">Le code démarre la connexion et qu’il passe ensuite une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page HTML.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="fc7dd-202">Cette approche garantit que la connexion est établie avant l’exécution du Gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-202">This approach insures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="fc7dd-203">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fc7dd-203">Next Steps</span></span>

<span data-ttu-id="fc7dd-204">Vous avez appris que SignalR est une infrastructure pour la génération d’applications web en temps réel.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="fc7dd-205">Vous avez également appris à plusieurs tâches de développement SignalR : comment ajouter SignalR pour une application ASP.NET, la création d’une classe de concentrateur et comment envoyer et recevoir des messages à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="fc7dd-206">Vous pouvez mettre l’exemple d’application dans ce didacticiel ou d’autres applications SignalR disposition sur Internet en les déployant sur un fournisseur d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="fc7dd-207">Microsoft propose d’hébergement web gratuit pour jusqu'à 10 sites web dans un [compte d’évaluation de Windows Azure](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="fc7dd-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="fc7dd-208">Pour une procédure pas à pas sur la façon de déployer l’exemple d’application SignalR, consultez [publier le SignalR exemple Getting Started comme un Site Web de Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc7dd-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="fc7dd-209">Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur un Site Web de Windows Azure, consultez [déploiement d’une Application ASP.NET à un Site Web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="fc7dd-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="fc7dd-210">(Remarque : le transport WebSocket est actuellement pas en charge pour les Sites Web Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="fc7dd-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="fc7dd-211">Transport WebSocket lorsque n’est pas disponible, SignalR utilise les autres transports disponibles comme décrit dans la section des Transports de la [Introduction à SignalR rubrique](index.md).)</span><span class="sxs-lookup"><span data-stu-id="fc7dd-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="fc7dd-212">Pour en savoir plus avancées concepts de développements SignalR, visitez les sites suivants pour le code source de SignalR et ressources :</span><span class="sxs-lookup"><span data-stu-id="fc7dd-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="fc7dd-213">Projet SignalR</span><span class="sxs-lookup"><span data-stu-id="fc7dd-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="fc7dd-214">SignalR Github et exemples</span><span class="sxs-lookup"><span data-stu-id="fc7dd-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="fc7dd-215">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="fc7dd-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
