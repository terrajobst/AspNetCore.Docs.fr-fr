---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutoriel : Conversation en temps réel avec SignalR 2 et MVC 5. | Microsoft Docs'
author: bradygaster
description: Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel. Vous ajoutez SignalR à une application MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1b02aecc68a93dbd6373ca5304530e76c9d0b6b5
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836999"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="ce782-104">Tutoriel : Conversation en temps réel avec SignalR 2 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="ce782-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="ce782-105">Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="ce782-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="ce782-106">Vous ajoutez des SignalR à une application MVC 5 et que vous créez une vue de conversation pour envoyer et afficher les messages.</span><span class="sxs-lookup"><span data-stu-id="ce782-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="ce782-107">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ce782-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ce782-108">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="ce782-108">Set up the project</span></span>
> * <span data-ttu-id="ce782-109">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="ce782-109">Run the sample</span></span>
> * <span data-ttu-id="ce782-110">Examinez le code</span><span class="sxs-lookup"><span data-stu-id="ce782-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="ce782-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="ce782-111">Prerequisites</span></span>

* <span data-ttu-id="ce782-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.</span><span class="sxs-lookup"><span data-stu-id="ce782-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ce782-113">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="ce782-113">Set up the Project</span></span>

<span data-ttu-id="ce782-114">Cette section montre comment utiliser Visual Studio 2017 et SignalR 2 pour créer une application ASP.NET MVC 5 vide, ajoutez la bibliothèque SignalR et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="ce782-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="ce782-115">Dans Visual Studio, créez une application c# ASP.NET qui cible .NET Framework 4.5, nommez-le SignalRChat et cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="ce782-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Créer le web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="ce782-117">Dans **nouvelle Application Web ASP.NET - SignalRMvcChat**, sélectionnez **MVC** , puis sélectionnez **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="ce782-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="ce782-118">Dans **modifier l’authentification**, sélectionnez **aucune authentification** et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce782-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Sélectionnez Aucune authentification](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="ce782-120">Dans **nouvelle Application Web ASP.NET - SignalRMvcChat**, sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce782-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="ce782-121">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="ce782-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="ce782-122">Dans **ajouter un nouvel élément - SignalRChat**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="ce782-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="ce782-123">Nommez la classe *ChatHub* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="ce782-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="ce782-124">Cette étape crée la *ChatHub.cs* fichier de classe et ajoute un ensemble de fichiers de script et les références d’assembly qui prennent en charge SignalR au projet.</span><span class="sxs-lookup"><span data-stu-id="ce782-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="ce782-125">Remplacez le code dans le nouveau *ChatHub.cs* fichier de classe avec ce code :</span><span class="sxs-lookup"><span data-stu-id="ce782-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="ce782-126">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="ce782-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="ce782-127">Nommez la nouvelle classe *démarrage* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="ce782-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="ce782-128">Remplacez le code dans le *Startup.cs* fichier de classe avec ce code :</span><span class="sxs-lookup"><span data-stu-id="ce782-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="ce782-129">Dans **l’Explorateur de solutions**, sélectionnez **contrôleurs** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ce782-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="ce782-130">Ajoutez la méthode suivante à la *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="ce782-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="ce782-131">Cette méthode retourne le **Chat** vue que vous créez dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="ce782-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="ce782-132">Dans **l’Explorateur de solutions**, avec le bouton droit **vues** > **accueil**, puis sélectionnez **ajouter**  >    **Vue**.</span><span class="sxs-lookup"><span data-stu-id="ce782-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="ce782-133">Dans **ajouter une vue**, nommez la nouvelle vue **Chat** et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ce782-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="ce782-134">Remplacez le contenu de **Chat.cshtml** avec ce code :</span><span class="sxs-lookup"><span data-stu-id="ce782-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="ce782-135">Dans **l’Explorateur de solutions**, développez **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="ce782-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="ce782-136">Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="ce782-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ce782-137">Le Gestionnaire de package peut avoir installé une version ultérieure des scripts SignalR.</span><span class="sxs-lookup"><span data-stu-id="ce782-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="ce782-138">Vérifier que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="ce782-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="ce782-139">Références de script à partir du bloc de code d’origine :</span><span class="sxs-lookup"><span data-stu-id="ce782-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="ce782-140">Si elles ne correspondent pas, mettre à jour le *.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="ce782-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="ce782-141">Dans la barre de menus, sélectionnez **fichier** > **Enregistrer tout**.</span><span class="sxs-lookup"><span data-stu-id="ce782-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="ce782-142">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="ce782-142">Run the Sample</span></span>

1. <span data-ttu-id="ce782-143">Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’exemple en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="ce782-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="ce782-145">Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.</span><span class="sxs-lookup"><span data-stu-id="ce782-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="ce782-146">Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="ce782-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="ce782-147">Dans chaque navigateur, entrez un nom unique.</span><span class="sxs-lookup"><span data-stu-id="ce782-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="ce782-148">Maintenant, ajoutez un commentaire, puis sélectionnez **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="ce782-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="ce782-149">Répétez que dans les autres navigateurs.</span><span class="sxs-lookup"><span data-stu-id="ce782-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="ce782-150">Les commentaires s’affichent en temps réel.</span><span class="sxs-lookup"><span data-stu-id="ce782-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ce782-151">Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="ce782-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="ce782-152">Le hub diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="ce782-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="ce782-153">Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.</span><span class="sxs-lookup"><span data-stu-id="ce782-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="ce782-154">Voyez comment l’application de conversation s’exécute dans trois différents navigateurs.</span><span class="sxs-lookup"><span data-stu-id="ce782-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="ce782-155">Lorsque Tom, Anand et Susan envoient des messages, tous les navigateurs mettre à jour en temps réel :</span><span class="sxs-lookup"><span data-stu-id="ce782-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Tous les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="ce782-157">Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="ce782-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="ce782-158">Il existe un fichier de script nommé *hubs* qui génère de la bibliothèque SignalR lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="ce782-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="ce782-159">Ce fichier gère la communication entre le script de jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="ce782-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de hubs généré automatiquement dans le nœud Documents de Script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="ce782-161">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="ce782-161">Examine the Code</span></span>

<span data-ttu-id="ce782-162">L’application de conversation SignalR montre deux tâches de développement SignalR base.</span><span class="sxs-lookup"><span data-stu-id="ce782-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="ce782-163">Il vous montre comment créer un hub.</span><span class="sxs-lookup"><span data-stu-id="ce782-163">It shows you how to create a hub.</span></span> <span data-ttu-id="ce782-164">Le serveur utilise ce hub en tant que l’objet principal de coordination.</span><span class="sxs-lookup"><span data-stu-id="ce782-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="ce782-165">Le hub utilise la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="ce782-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="ce782-166">Concentrateurs SignalR dans les ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="ce782-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="ce782-167">Dans l’exemple de code, le `ChatHub` classe dérive de la `Microsoft.AspNet.SignalR.Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="ce782-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="ce782-168">Dérivation à partir de la `Hub` classe est un moyen utile pour créer une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="ce782-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="ce782-169">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts dans une page web.</span><span class="sxs-lookup"><span data-stu-id="ce782-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="ce782-170">Dans le code de la conversation, les clients appellent le `ChatHub.Send` méthode pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="ce782-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="ce782-171">Le concentrateur à son tour envoie le message à tous les clients en appelant `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="ce782-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="ce782-172">Le `Send` méthode illustre plusieurs concepts de hub :</span><span class="sxs-lookup"><span data-stu-id="ce782-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="ce782-173">Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.</span><span class="sxs-lookup"><span data-stu-id="ce782-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="ce782-174">Utilisez le `Microsoft.AspNet.SignalR.Hub.Clients` propriété dynamique à communiquer avec tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ce782-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="ce782-175">Appeler une fonction sur le client (comme le `addNewMessageToPage` (fonction)) pour mettre à jour des clients.</span><span class="sxs-lookup"><span data-stu-id="ce782-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="ce782-176">SignalR et jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="ce782-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="ce782-177">Le *Chat.cshtml* afficher le fichier dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="ce782-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="ce782-178">Le code effectue de nombreuses tâches importantes.</span><span class="sxs-lookup"><span data-stu-id="ce782-178">The code carries out many important tasks.</span></span> <span data-ttu-id="ce782-179">Il crée une référence au proxy généré automatiquement pour le hub, déclare une fonction que le serveur peut appeler pour envoyer le contenu aux clients, et il démarre une connexion pour envoyer des messages au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="ce782-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="ce782-180">Dans JavaScript, la référence à la classe de serveur et de ses membres est dans une casse mixte.</span><span class="sxs-lookup"><span data-stu-id="ce782-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="ce782-181">Les références d’exemple de code la C# `ChatHub` classe dans JavaScript en tant que `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="ce782-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="ce782-182">Dans ce bloc de code, vous créez une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="ce782-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="ce782-183">La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="ce782-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="ce782-184">L’appel facultatif à la `htmlEncode` affiche la fonction moyen HTML encode le contenu du message avant de les afficher dans la page.</span><span class="sxs-lookup"><span data-stu-id="ce782-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="ce782-185">C’est un moyen pour empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="ce782-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="ce782-186">Ce code ouvre une connexion avec le hub.</span><span class="sxs-lookup"><span data-stu-id="ce782-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="ce782-187">Cette approche garantit que vous établissez une connexion avant que le Gestionnaire d’événements s’exécute.</span><span class="sxs-lookup"><span data-stu-id="ce782-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="ce782-188">Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page de la conversation.</span><span class="sxs-lookup"><span data-stu-id="ce782-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="ce782-189">Obtenir le code</span><span class="sxs-lookup"><span data-stu-id="ce782-189">Get the code</span></span>

[<span data-ttu-id="ce782-190">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="ce782-190">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="ce782-191">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ce782-191">Additional resources</span></span>

<span data-ttu-id="ce782-192">Pour en savoir plus sur SignalR, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="ce782-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="ce782-193">Projet de SignalR</span><span class="sxs-lookup"><span data-stu-id="ce782-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="ce782-194">SignalR GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="ce782-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="ce782-195">Wiki de SignalR</span><span class="sxs-lookup"><span data-stu-id="ce782-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="ce782-196">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="ce782-196">Next steps</span></span>

<span data-ttu-id="ce782-197">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="ce782-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ce782-198">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="ce782-198">Set up the project</span></span>
> * <span data-ttu-id="ce782-199">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="ce782-199">Ran the sample</span></span>
> * <span data-ttu-id="ce782-200">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="ce782-200">Examined the code</span></span>

<span data-ttu-id="ce782-201">Passez à l’article suivant pour apprendre à créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de messagerie à fréquence élevée.</span><span class="sxs-lookup"><span data-stu-id="ce782-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ce782-202">Application Web avec la messagerie à fréquence élevée</span><span class="sxs-lookup"><span data-stu-id="ce782-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)