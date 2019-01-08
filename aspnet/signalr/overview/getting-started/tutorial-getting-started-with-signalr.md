---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutoriel : Conversation en temps réel avec SignalR 2 | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous ajoutez SignalR à une application de web ASP.NET vide.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: aa015abc47bb2450e04e167c0404aaa1d119ba2c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098622"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="993e7-104">Tutoriel : Conversation en temps réel avec SignalR 2</span><span class="sxs-lookup"><span data-stu-id="993e7-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="993e7-105">Ce didacticiel vous montre comment utiliser SignalR pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="993e7-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="993e7-106">Vous ajoutez des SignalR à une application de web ASP.NET vide et que vous créez une page HTML pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="993e7-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="993e7-107">Dans ce didacticiel, vous :</span><span class="sxs-lookup"><span data-stu-id="993e7-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="993e7-108">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="993e7-108">Set up the project</span></span>
> * <span data-ttu-id="993e7-109">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="993e7-109">Run the sample</span></span>
> * <span data-ttu-id="993e7-110">Examinez le code</span><span class="sxs-lookup"><span data-stu-id="993e7-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="993e7-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="993e7-111">Prerequisites</span></span>

* <span data-ttu-id="993e7-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.</span><span class="sxs-lookup"><span data-stu-id="993e7-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="993e7-113">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="993e7-113">Set up the Project</span></span>

<span data-ttu-id="993e7-114">Cette section montre comment utiliser Visual Studio 2017 et SignalR 2 pour créer une application web ASP.NET vide, ajouter SignalR et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="993e7-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="993e7-115">Dans Visual Studio, créez une Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="993e7-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer le web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="993e7-117">Dans le **nouveau projet ASP.NET - SignalRChat** fenêtre, laissez le champ **vide** sélectionné et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="993e7-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="993e7-118">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="993e7-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="993e7-119">Dans **ajouter un nouvel élément - SignalRChat**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="993e7-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="993e7-120">Nommez la classe *ChatHub* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="993e7-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="993e7-121">Cette étape crée la *ChatHub.cs* fichier de classe et ajoute un ensemble de fichiers de script et les références d’assembly qui prennent en charge SignalR au projet.</span><span class="sxs-lookup"><span data-stu-id="993e7-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="993e7-122">Remplacez le code dans le nouveau *ChatHub.cs* fichier de classe avec ce code :</span><span class="sxs-lookup"><span data-stu-id="993e7-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="993e7-123">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="993e7-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="993e7-124">Dans **ajouter un nouvel élément - SignalRChat** sélectionnez **installé** > **Visual C#**   >  **Web** , puis Sélectionnez **classe de démarrage OWIN**.</span><span class="sxs-lookup"><span data-stu-id="993e7-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="993e7-125">Nommez la classe *démarrage* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="993e7-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="993e7-126">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **HTML Page**.</span><span class="sxs-lookup"><span data-stu-id="993e7-126">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="993e7-127">Nommez la nouvelle page *index* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="993e7-127">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="993e7-128">Dans **l’Explorateur de solutions**, avec le bouton droit de la page HTML que vous avez créé et sélectionnez **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="993e7-128">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="993e7-129">Remplacez le code par défaut dans la page HTML avec ce code :</span><span class="sxs-lookup"><span data-stu-id="993e7-129">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="993e7-130">Dans **l’Explorateur de solutions**, développez **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="993e7-130">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="993e7-131">Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="993e7-131">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="993e7-132">Le Gestionnaire de package peut avoir installé une version ultérieure des scripts SignalR.</span><span class="sxs-lookup"><span data-stu-id="993e7-132">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="993e7-133">Vérifier que les références de script dans le bloc de code correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="993e7-133">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="993e7-134">Références de script à partir du bloc de code d’origine :</span><span class="sxs-lookup"><span data-stu-id="993e7-134">Script references from the original code block:</span></span>
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="993e7-135">Si elles ne correspondent pas, mettre à jour le *.html* fichier.</span><span class="sxs-lookup"><span data-stu-id="993e7-135">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="993e7-136">Dans la barre de menus, sélectionnez **fichier** > **Enregistrer tout**.</span><span class="sxs-lookup"><span data-stu-id="993e7-136">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="993e7-137">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="993e7-137">Run the Sample</span></span>

1. <span data-ttu-id="993e7-138">Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’exemple en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="993e7-138">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="993e7-140">Lorsque le navigateur s’ouvre, entrez un nom pour votre identité de conversation.</span><span class="sxs-lookup"><span data-stu-id="993e7-140">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="993e7-141">Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="993e7-141">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="993e7-142">Dans chaque navigateur, entrez un nom unique.</span><span class="sxs-lookup"><span data-stu-id="993e7-142">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="993e7-143">Maintenant, ajoutez un commentaire, puis sélectionnez **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="993e7-143">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="993e7-144">Répétez que dans les autres navigateurs.</span><span class="sxs-lookup"><span data-stu-id="993e7-144">Repeat that in the other browsers.</span></span> <span data-ttu-id="993e7-145">Les commentaires s’affichent en temps réel.</span><span class="sxs-lookup"><span data-stu-id="993e7-145">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="993e7-146">Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="993e7-146">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="993e7-147">Le hub diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="993e7-147">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="993e7-148">Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.</span><span class="sxs-lookup"><span data-stu-id="993e7-148">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="993e7-149">Voyez comment l’application de conversation s’exécute dans trois différents navigateurs.</span><span class="sxs-lookup"><span data-stu-id="993e7-149">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="993e7-150">Lorsque Tom, Anand et Susan envoient des messages, tous les navigateurs mettre à jour en temps réel :</span><span class="sxs-lookup"><span data-stu-id="993e7-150">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Tous les trois navigateurs affichent le même historique de conversation](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="993e7-152">Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="993e7-152">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="993e7-153">Il existe un fichier de script nommé *hubs* qui génère de la bibliothèque SignalR lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="993e7-153">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="993e7-154">Ce fichier gère la communication entre le script de jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="993e7-154">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de hubs généré automatiquement dans le nœud Documents de Script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="993e7-156">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="993e7-156">Examine the Code</span></span>

<span data-ttu-id="993e7-157">L’application SignalRChat montre deux tâches de développement SignalR base.</span><span class="sxs-lookup"><span data-stu-id="993e7-157">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="993e7-158">Il vous montre comment créer un hub.</span><span class="sxs-lookup"><span data-stu-id="993e7-158">It shows you how to create a hub.</span></span> <span data-ttu-id="993e7-159">Le serveur utilise ce hub en tant que l’objet principal de coordination.</span><span class="sxs-lookup"><span data-stu-id="993e7-159">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="993e7-160">Le hub utilise la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="993e7-160">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="993e7-161">Concentrateurs SignalR dans les ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="993e7-161">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="993e7-162">Dans l’exemple de code ci-dessus, le `ChatHub` classe dérive de la `Microsoft.AspNet.SignalR.Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="993e7-162">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="993e7-163">Dérivation à partir de la `Hub` classe est un moyen utile pour créer une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="993e7-163">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="993e7-164">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite utiliser ces méthodes en les appelant à partir de scripts dans une page web.</span><span class="sxs-lookup"><span data-stu-id="993e7-164">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="993e7-165">Dans le code de la conversation, les clients appellent le `ChatHub.Send` méthode pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="993e7-165">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="993e7-166">Le concentrateur envoie ensuite le message à tous les clients en appelant `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="993e7-166">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="993e7-167">Le `Send` méthode illustre plusieurs concepts de hub :</span><span class="sxs-lookup"><span data-stu-id="993e7-167">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="993e7-168">Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.</span><span class="sxs-lookup"><span data-stu-id="993e7-168">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="993e7-169">Utilisez le `Microsoft.AspNet.SignalR.Hub.Clients` propriété dynamique à communiquer avec tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="993e7-169">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="993e7-170">Appeler une fonction sur le client (comme le `broadcastMessage` (fonction)) pour mettre à jour des clients.</span><span class="sxs-lookup"><span data-stu-id="993e7-170">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="993e7-171">SignalR et jQuery dans le fichier index.html</span><span class="sxs-lookup"><span data-stu-id="993e7-171">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="993e7-172">Le *index.html* page dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="993e7-172">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="993e7-173">Le code effectue de nombreuses tâches importantes.</span><span class="sxs-lookup"><span data-stu-id="993e7-173">The code carries out many important tasks.</span></span> <span data-ttu-id="993e7-174">Elle déclare un proxy vers le hub de référence, déclare une fonction que le serveur peut appeler pour transmettre le contenu aux clients, et il démarre une connexion pour envoyer des messages au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="993e7-174">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="993e7-175">Dans JavaScript la référence à la classe server et ses membres doit être une casse mixte.</span><span class="sxs-lookup"><span data-stu-id="993e7-175">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="993e7-176">Les références d’exemple de code la C# *ChatHub* classe dans JavaScript en tant que `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="993e7-176">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="993e7-177">Dans ce bloc de code, vous créez une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="993e7-177">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="993e7-178">La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="993e7-178">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="993e7-179">Les deux lignes que HTML à encoder le contenu avant de les afficher sont facultatifs et afficher un bon moyen d’empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="993e7-179">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="993e7-180">Ce code ouvre une connexion avec le hub.</span><span class="sxs-lookup"><span data-stu-id="993e7-180">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="993e7-181">Cette approche garantit que le code établit une connexion avant que le Gestionnaire d’événements s’exécute.</span><span class="sxs-lookup"><span data-stu-id="993e7-181">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="993e7-182">Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page HTML.</span><span class="sxs-lookup"><span data-stu-id="993e7-182">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="993e7-183">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="993e7-183">Additional resources</span></span>

<span data-ttu-id="993e7-184">Pour en savoir plus sur SignalR, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="993e7-184">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="993e7-185">Projet de SignalR</span><span class="sxs-lookup"><span data-stu-id="993e7-185">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="993e7-186">SignalR Github et exemples</span><span class="sxs-lookup"><span data-stu-id="993e7-186">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="993e7-187">Wiki de SignalR</span><span class="sxs-lookup"><span data-stu-id="993e7-187">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="993e7-188">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="993e7-188">Next steps</span></span>

<span data-ttu-id="993e7-189">Dans ce didacticiel vous :</span><span class="sxs-lookup"><span data-stu-id="993e7-189">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="993e7-190">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="993e7-190">Set up the project</span></span>
> * <span data-ttu-id="993e7-191">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="993e7-191">Ran the sample</span></span>
> * <span data-ttu-id="993e7-192">Examiner le code</span><span class="sxs-lookup"><span data-stu-id="993e7-192">Examined the code</span></span>

<span data-ttu-id="993e7-193">Passez à l’article suivant pour apprendre à utiliser SignalR et MVC 5.</span><span class="sxs-lookup"><span data-stu-id="993e7-193">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="993e7-194">SignalR 2 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="993e7-194">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)