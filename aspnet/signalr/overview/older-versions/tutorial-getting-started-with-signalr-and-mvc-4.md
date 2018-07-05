---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Didacticiel : Bien démarrer avec SignalR 1.x et MVC 4 | Microsoft Docs'
author: pfletcher
description: Utiliser ASP.NET SignalR et ASP.NET MVC 4 pour créer une application de conversation en temps réel.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: cced40f65700eff073aa4cff4560a137a50bbc5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399955"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="f7400-103">Didacticiel : Bien démarrer avec SignalR 1.x et MVC 4</span><span class="sxs-lookup"><span data-stu-id="f7400-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="f7400-104">par [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="f7400-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="f7400-105">Ce didacticiel montre comment utiliser ASP.NET SignalR pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="f7400-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="f7400-106">Vous ajouter SignalR à une application MVC 4 et créer une vue de conversation pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="f7400-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="f7400-107">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f7400-107">Overview</span></span>

<span data-ttu-id="f7400-108">Ce didacticiel vous présente au développement d’applications web en temps réel avec SignalR ASP.NET et ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f7400-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="f7400-109">Ce didacticiel utilise le même code d’application de conversation que la [didacticiel SignalR mise en route](tutorial-getting-started-with-signalr.md), mais montre comment ajouter à une application MVC 4 basée sur le modèle d’Internet.</span><span class="sxs-lookup"><span data-stu-id="f7400-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="f7400-110">Dans cette rubrique, vous allez apprendre les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="f7400-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="f7400-111">Ajout de la bibliothèque de SignalR à une application MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f7400-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="f7400-112">Création d’une classe de hub pour envoyer le contenu vers les clients.</span><span class="sxs-lookup"><span data-stu-id="f7400-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="f7400-113">À l’aide de la bibliothèque jQuery SignalR dans une page web pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="f7400-114">La capture d’écran suivante montre l’application de conversation terminée en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instances de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="f7400-116">Sections :</span><span class="sxs-lookup"><span data-stu-id="f7400-116">Sections:</span></span>

- [<span data-ttu-id="f7400-117">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="f7400-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="f7400-118">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="f7400-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="f7400-119">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="f7400-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="f7400-120">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="f7400-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="f7400-121">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="f7400-121">Set up the Project</span></span>

<span data-ttu-id="f7400-122">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="f7400-122">Prerequisites:</span></span>

- <span data-ttu-id="f7400-123">Visual Studio 2010 SP1, Visual Studio 2012 ou Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="f7400-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="f7400-124">Si vous n’avez pas Visual Studio, consultez [téléchargements ASP.NET](https://www.asp.net/downloads) pour obtenir le Visual Studio 2012 Express outil de développement gratuit.</span><span class="sxs-lookup"><span data-stu-id="f7400-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="f7400-125">Pour Visual Studio 2010, installez [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="f7400-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="f7400-126">Cette section montre comment créer une application ASP.NET MVC 4, ajoutez la bibliothèque SignalR et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="f7400-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="f7400-127">Dans Visual Studio créer une application ASP.NET MVC 4, nommez-le SignalRChat et cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="f7400-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="f7400-128">Dans Visual Studio 2010, sélectionnez **.NET Framework 4** dans le contrôle de liste déroulante de version de Framework.</span><span class="sxs-lookup"><span data-stu-id="f7400-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="f7400-129">Code de SignalR s’exécute sur les versions du .NET Framework 4 et 4.5.</span><span class="sxs-lookup"><span data-stu-id="f7400-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Créer web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="f7400-131">Sélectionnez le modèle d’Application Internet, désactivez l’option **créer un projet de test unitaire**, puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="f7400-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Créer un site internet de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="f7400-133">Ouvrez le **outils | Gestionnaire de Package de bibliothèque | Console du Gestionnaire de package** et exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="f7400-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="f7400-134">Cette étape ajoute au projet un ensemble de fichiers de script et les références d’assembly qui activent les fonctionnalités de SignalR.</span><span class="sxs-lookup"><span data-stu-id="f7400-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="f7400-135">Dans **l’Explorateur de solutions** développez le dossier Scripts.</span><span class="sxs-lookup"><span data-stu-id="f7400-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="f7400-136">Notez que les bibliothèques de scripts pour SignalR ont été ajoutés au projet.</span><span class="sxs-lookup"><span data-stu-id="f7400-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Références de bibliothèque](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="f7400-138">Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter | Nouveau dossier**, et ajoutez un nouveau dossier nommé **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="f7400-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="f7400-139">Cliquez sur le **Hubs** dossier, cliquez sur **ajouter | Classe**et créez une nouvelle classe c# nommée **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="f7400-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="f7400-140">Vous utiliserez cette classe comme un concentrateur de serveur SignalR qui envoie des messages à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="f7400-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="f7400-141">Si vous utilisez Visual Studio 2012 et que vous avez installé le [mise à jour ASP.NET et Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), vous pouvez utiliser le nouveau modèle d’élément de SignalR pour créer la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="f7400-142">Pour ce faire, cliquez sur le **Hubs** dossier, cliquez sur **ajouter | Un nouvel élément**, sélectionnez **classe de concentrateur SignalR (v1)** et nommez la classe **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="f7400-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="f7400-143">Remplacez le code dans le **ChatHub** classe par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="f7400-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="f7400-144">Ouvrez le **Global.asax** fichier pour le projet, puis ajoutez un appel à la méthode `RouteTable.Routes.MapHubs();` en tant que la première ligne de code dans le `Application_Start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="f7400-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="f7400-145">Ce code enregistre l’itinéraire par défaut pour les concentrateurs SignalR et doit être appelé avant d’inscrire tous les autres itinéraires.</span><span class="sxs-lookup"><span data-stu-id="f7400-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="f7400-146">Fin du `Application_Start` méthode ressemble à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="f7400-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="f7400-147">Modifier le `HomeController` classe trouvée dans **Controllers/HomeController.cs** et ajoutez la méthode suivante à la classe.</span><span class="sxs-lookup"><span data-stu-id="f7400-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="f7400-148">Cette méthode retourne le **Chat** vue que vous allez créer dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="f7400-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="f7400-149">Avec le bouton droit dans le `Chat` méthode que vous venez de créer et cliquez sur **ajouter une vue** pour créer un nouveau fichier d’affichage.</span><span class="sxs-lookup"><span data-stu-id="f7400-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="f7400-150">Dans le **ajouter une vue** boîte de dialogue, vérifiez que la case à cocher est sélectionnée pour **utiliser une disposition ou la page maître** (désactivez les autres cases à cocher), puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="f7400-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Ajouter une vue](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="f7400-152">Modifiez le nouveau fichier d’affichage nommé **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="f7400-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="f7400-153">Après le &lt;h2&gt; balise, collez le code suivant &lt;div&gt; section et `@section scripts` bloc de code dans la page.</span><span class="sxs-lookup"><span data-stu-id="f7400-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="f7400-154">Ce script permet à la page envoyer des messages de conversation et afficher des messages à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="f7400-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="f7400-155">Le code complet pour la vue de conversation s’affiche dans le bloc de code suivant.</span><span class="sxs-lookup"><span data-stu-id="f7400-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f7400-156">Lorsque vous ajoutez SignalR et autres bibliothèques de scripts à votre projet Visual Studio, le Gestionnaire de Package peut installer des versions des scripts qui sont plus récents que les versions indiquées dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="f7400-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="f7400-157">Assurez-vous que les références de script dans votre code correspondent aux versions des bibliothèques de script installés dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="f7400-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="f7400-158">**Enregistrer tous les** pour le projet.</span><span class="sxs-lookup"><span data-stu-id="f7400-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="f7400-159">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="f7400-159">Run the Sample</span></span>

1. <span data-ttu-id="f7400-160">Appuyez sur F5 pour exécuter le projet en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="f7400-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="f7400-161">Dans la ligne d’adresse de navigateur, ajoutez **chat/home/** à l’URL de la page par défaut pour le projet.</span><span class="sxs-lookup"><span data-stu-id="f7400-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="f7400-162">La page de la conversation se charge dans une instance du navigateur et des invites pour un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="f7400-164">Entrez un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-164">Enter a user name.</span></span>
4. <span data-ttu-id="f7400-165">Copiez l’URL de la ligne d’adresse du navigateur et l’utiliser pour ouvrir les deux autres instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="f7400-166">Dans chaque instance du navigateur, entrez un nom d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="f7400-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="f7400-167">Dans chaque instance du navigateur, ajoutez un commentaire et cliquez sur **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="f7400-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="f7400-168">Les commentaires doivent s’afficher dans toutes les instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f7400-169">Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f7400-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="f7400-170">Le hub diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="f7400-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="f7400-171">Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.</span><span class="sxs-lookup"><span data-stu-id="f7400-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="f7400-172">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="f7400-174">Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="f7400-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="f7400-175">Ce nœud est visible en mode débogage si vous utilisez Internet Explorer comme navigateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="f7400-176">Il existe un fichier de script nommé **hubs** que la bibliothèque SignalR génère dynamiquement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f7400-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="f7400-177">Ce fichier gère la communication entre le script de jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="f7400-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="f7400-178">Si vous utilisez un navigateur autre que Internet Explorer, vous pouvez également accéder à la dynamique **hubs** fichier en y accédant directement, par exemple http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="f7400-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Script de hub généré](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="f7400-180">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="f7400-180">Examine the Code</span></span>

<span data-ttu-id="f7400-181">L’application de conversation SignalR montre deux tâches de développement SignalR base : création d’un hub en tant que l’objet principal de coordination sur le serveur et à l’aide de la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="f7400-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="f7400-182">Concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="f7400-182">SignalR Hubs</span></span>

<span data-ttu-id="f7400-183">Dans l’exemple de code la **ChatHub** classe dérive de la **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="f7400-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="f7400-184">Dérivation à partir de la **Hub** classe est un moyen utile pour créer une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="f7400-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="f7400-185">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts jQuery dans une page web.</span><span class="sxs-lookup"><span data-stu-id="f7400-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="f7400-186">Dans le code de la conversation, les clients appellent le **ChatHub.Send** méthode pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="f7400-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="f7400-187">Le concentrateur à son tour envoie le message à tous les clients en appelant **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="f7400-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="f7400-188">Le **envoyer** méthode illustre plusieurs concepts de hub :</span><span class="sxs-lookup"><span data-stu-id="f7400-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="f7400-189">Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.</span><span class="sxs-lookup"><span data-stu-id="f7400-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="f7400-190">Utilisez le **Microsoft.AspNet.SignalR.Hub.Clients** propriété pour accéder à tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="f7400-191">Appeler une fonction de jQuery sur le client (tel que le `addNewMessageToPage` (fonction)) pour mettre à jour des clients.</span><span class="sxs-lookup"><span data-stu-id="f7400-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="f7400-192">SignalR et jQuery</span><span class="sxs-lookup"><span data-stu-id="f7400-192">SignalR and jQuery</span></span>

<span data-ttu-id="f7400-193">Le **Chat.cshtml** afficher le fichier dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="f7400-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="f7400-194">Les tâches essentielles dans le code sont création d’une référence pour le proxy généré automatiquement pour le hub, déclarant une fonction que le serveur peut appeler pour transmettre le contenu aux clients et le démarrage d’une connexion pour envoyer des messages au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="f7400-195">Le code suivant déclare un proxy pour un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="f7400-196">Dans jQuery, la référence à la classe de serveur et de ses membres est en casse mixte.</span><span class="sxs-lookup"><span data-stu-id="f7400-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="f7400-197">L’exemple de code fait référence à celle de C# **ChatHub** classe dans jQuery comme **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="f7400-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="f7400-198">Si vous souhaitez faire référence à la `ChatHub` classe dans jQuery avec Pascal conventionnel mise en majuscules comme vous le feriez en c#, modifiez le fichier de classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="f7400-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="f7400-199">Ajouter un `using` instruction pour référencer le `Microsoft.AspNet.SignalR.Hubs` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="f7400-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="f7400-200">Ajoutez ensuite le `HubName` attribut le `ChatHub` de classe, par exemple `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="f7400-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="f7400-201">Enfin, mettez à jour de votre référence de jQuery à la `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="f7400-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="f7400-202">Le code suivant montre comment créer une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="f7400-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="f7400-203">La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="f7400-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="f7400-204">L’appel facultatif à la `htmlEncode` affiche la fonction moyen HTML encode le contenu du message avant de les afficher dans la page, comme un moyen d’empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="f7400-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="f7400-205">Le code suivant montre comment ouvrir une connexion avec le hub.</span><span class="sxs-lookup"><span data-stu-id="f7400-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="f7400-206">Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page de la conversation.</span><span class="sxs-lookup"><span data-stu-id="f7400-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="f7400-207">Cette approche garantit que la connexion est établie avant que le Gestionnaire d’événements s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f7400-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="f7400-208">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="f7400-208">Next Steps</span></span>

<span data-ttu-id="f7400-209">Vous avez appris que SignalR est une infrastructure pour générer des applications web en temps réel.</span><span class="sxs-lookup"><span data-stu-id="f7400-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="f7400-210">Vous avez également appris à plusieurs tâches de développement de SignalR : comment ajouter SignalR à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f7400-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="f7400-211">Pour en savoir plus les concepts de développements SignalR plus avancés, consultez les sites suivants pour le code source de SignalR et de ressources :</span><span class="sxs-lookup"><span data-stu-id="f7400-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="f7400-212">Projet de SignalR</span><span class="sxs-lookup"><span data-stu-id="f7400-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="f7400-213">SignalR Github et exemples</span><span class="sxs-lookup"><span data-stu-id="f7400-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="f7400-214">Wiki de SignalR</span><span class="sxs-lookup"><span data-stu-id="f7400-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
