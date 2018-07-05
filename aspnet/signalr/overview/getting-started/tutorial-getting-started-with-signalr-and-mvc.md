---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Didacticiel : Bien démarrer avec SignalR 2 et MVC 5 | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel. Vous ajouter SignalR à une application MVC 5 et créer une vue de la conversation...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 903319040c9ac938cea5dce2e6579d88e0d80bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384049"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="12f60-104">Didacticiel : Bien démarrer avec SignalR 2 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="12f60-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="12f60-105">par [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="12f60-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="12f60-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="12f60-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="12f60-107">Ce didacticiel montre comment utiliser ASP.NET SignalR 2 pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="12f60-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="12f60-108">Vous ajouter SignalR à une application MVC 5 et créer une vue de conversation pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="12f60-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="12f60-109">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="12f60-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="12f60-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="12f60-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="12f60-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="12f60-111">.NET 4.5</span></span>
> - <span data-ttu-id="12f60-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="12f60-112">MVC 5</span></span>
> - <span data-ttu-id="12f60-113">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="12f60-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="12f60-114">À l’aide de Visual Studio 2012 avec ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="12f60-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="12f60-115">Pour utiliser Visual Studio 2012 avec ce didacticiel, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="12f60-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="12f60-116">Mise à jour votre [Gestionnaire de Package](http://docs.nuget.org/docs/start-here/installing-nuget) vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="12f60-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="12f60-117">Installer le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="12f60-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="12f60-118">Dans le programme Web Platform Installer, recherchez et installez **ASP.NET et Web Tools 2013.1 pour Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="12f60-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="12f60-119">Cela installera les modèles Visual Studio pour les classes de SignalR comme **Hub**.</span><span class="sxs-lookup"><span data-stu-id="12f60-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="12f60-120">Certains modèles (tels que **classe de démarrage OWIN**) ne sera pas disponible ; dans ce cas, utilisez un fichier de classe à la place.</span><span class="sxs-lookup"><span data-stu-id="12f60-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="12f60-121">Versions de didacticiels</span><span class="sxs-lookup"><span data-stu-id="12f60-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="12f60-122">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="12f60-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="12f60-123">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="12f60-123">Questions and comments</span></span>
> 
> <span data-ttu-id="12f60-124">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="12f60-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="12f60-125">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="12f60-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="12f60-126">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="12f60-126">Overview</span></span>

<span data-ttu-id="12f60-127">Ce didacticiel vous présente au développement d’applications web en temps réel avec ASP.NET SignalR 2 et ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="12f60-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="12f60-128">Ce didacticiel utilise le même code d’application de conversation que la [didacticiel SignalR mise en route](tutorial-getting-started-with-signalr.md), mais montre comment ajouter à une application MVC 5.</span><span class="sxs-lookup"><span data-stu-id="12f60-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="12f60-129">Dans cette rubrique, vous allez apprendre les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="12f60-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="12f60-130">Ajout de la bibliothèque de SignalR à une application MVC 5.</span><span class="sxs-lookup"><span data-stu-id="12f60-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="12f60-131">Création de hub et les classes de démarrage OWIN pour envoyer le contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="12f60-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="12f60-132">À l’aide de la bibliothèque jQuery SignalR dans une page web pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="12f60-133">La capture d’écran suivante montre l’application de conversation terminée en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instances de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="12f60-135">Sections :</span><span class="sxs-lookup"><span data-stu-id="12f60-135">Sections:</span></span>

- [<span data-ttu-id="12f60-136">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="12f60-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="12f60-137">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="12f60-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="12f60-138">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="12f60-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="12f60-139">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="12f60-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="12f60-140">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="12f60-140">Set up the Project</span></span>

<span data-ttu-id="12f60-141">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="12f60-141">Prerequisites:</span></span>

- <span data-ttu-id="12f60-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="12f60-142">Visual Studio 2013.</span></span> <span data-ttu-id="12f60-143">Si vous n’avez pas Visual Studio, consultez [téléchargements ASP.NET](https://www.asp.net/downloads) pour obtenir le Visual Studio 2013 Express outil de développement gratuit.</span><span class="sxs-lookup"><span data-stu-id="12f60-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="12f60-144">Cette section montre comment créer une application ASP.NET MVC 5, ajoutez la bibliothèque SignalR et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="12f60-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="12f60-145">Dans Visual Studio, créez une application c# ASP.NET qui cible .NET Framework 4.5, nommez-le SignalRChat et cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="12f60-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Créer le web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="12f60-147">Dans le `New ASP.NET Project` boîte de dialogue, puis sélectionnez **MVC**, puis cliquez sur **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="12f60-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Créer le web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="12f60-149">Sélectionnez **aucune authentification** dans le **modifier l’authentification** boîte de dialogue, puis cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="12f60-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Sélectionnez Aucune authentification](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="12f60-151">Si vous sélectionnez un fournisseur d’authentification pour votre application, un `Startup.cs` classe sera créée pour vous, vous ne devez pas créer votre propre `Startup.cs` classe à l’étape 10 ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="12f60-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="12f60-152">Cliquez sur **OK** dans le **nouveau projet ASP.NET** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="12f60-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="12f60-153">Ouvrez le **outils | Gestionnaire de Package de bibliothèque | Console du Gestionnaire de package** et exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="12f60-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="12f60-154">Cette étape ajoute au projet un ensemble de fichiers de script et les références d’assembly qui activent les fonctionnalités de SignalR.</span><span class="sxs-lookup"><span data-stu-id="12f60-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="12f60-155">Dans **l’Explorateur de solutions**, développez le dossier Scripts.</span><span class="sxs-lookup"><span data-stu-id="12f60-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="12f60-156">Notez que les bibliothèques de scripts pour SignalR ont été ajoutés au projet.</span><span class="sxs-lookup"><span data-stu-id="12f60-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Dossier de scripts](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="12f60-158">Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter | Nouveau dossier**, et ajoutez un nouveau dossier nommé **Hubs**.</span><span class="sxs-lookup"><span data-stu-id="12f60-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="12f60-159">Cliquez sur le **Hubs** dossier, cliquez sur **ajouter | Un nouvel élément**, sélectionnez le **Visual C# | Web | SignalR** nœud dans le **installé** volet, sélectionnez **classe de concentrateur SignalR (v2)** dans le volet central et créez un nouveau hub nommé **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="12f60-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="12f60-160">Vous utiliserez cette classe comme un concentrateur de serveur SignalR qui envoie des messages à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="12f60-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Créer nouveau concentrateur](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="12f60-162">Remplacez le code dans le **ChatHub** classe par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="12f60-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="12f60-163">Créer une nouvelle classe appelée Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="12f60-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="12f60-164">Modifier le contenu du fichier à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="12f60-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="12f60-165">Modifier le `HomeController` classe trouvée dans **Controllers/HomeController.cs** et ajoutez la méthode suivante à la classe.</span><span class="sxs-lookup"><span data-stu-id="12f60-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="12f60-166">Cette méthode retourne le **Chat** vue que vous allez créer dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="12f60-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="12f60-167">Cliquez sur le **vues/accueil** dossier, puis sélectionnez **ajouter... | Vue**.</span><span class="sxs-lookup"><span data-stu-id="12f60-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="12f60-168">Dans le **ajouter une vue** boîte de dialogue, nom de la nouvelle vue **Chat**.</span><span class="sxs-lookup"><span data-stu-id="12f60-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Ajouter une vue](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="12f60-170">Remplacez le contenu de **Chat.cshtml** avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="12f60-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="12f60-171">Lorsque vous ajoutez SignalR et autres bibliothèques de scripts à votre projet Visual Studio, le Gestionnaire de Package peut installer une version du fichier de script de SignalR est plus récente que la version indiquée dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="12f60-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="12f60-172">Assurez-vous que la référence de script dans votre code correspond à la version de la bibliothèque de scripts installée dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="12f60-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="12f60-173">**Enregistrer tous les** pour le projet.</span><span class="sxs-lookup"><span data-stu-id="12f60-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="12f60-174">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="12f60-174">Run the Sample</span></span>

1. <span data-ttu-id="12f60-175">Appuyez sur F5 pour exécuter le projet en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="12f60-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="12f60-176">Dans la ligne d’adresse de navigateur, ajoutez **chat/home/** à l’URL de la page par défaut pour le projet.</span><span class="sxs-lookup"><span data-stu-id="12f60-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="12f60-177">La page de la conversation se charge dans une instance du navigateur et des invites pour un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="12f60-179">Entrez un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-179">Enter a user name.</span></span>
4. <span data-ttu-id="12f60-180">Copiez l’URL de la ligne d’adresse du navigateur et l’utiliser pour ouvrir les deux autres instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="12f60-181">Dans chaque instance du navigateur, entrez un nom d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="12f60-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="12f60-182">Dans chaque instance du navigateur, ajoutez un commentaire et cliquez sur **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="12f60-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="12f60-183">Les commentaires doivent s’afficher dans toutes les instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="12f60-184">Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="12f60-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="12f60-185">Le hub diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="12f60-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="12f60-186">Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.</span><span class="sxs-lookup"><span data-stu-id="12f60-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="12f60-187">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="12f60-189">Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="12f60-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="12f60-190">Ce nœud est visible en mode débogage si vous utilisez Internet Explorer comme navigateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="12f60-191">Il existe un fichier de script nommé **hubs** que la bibliothèque SignalR génère dynamiquement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="12f60-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="12f60-192">Ce fichier gère la communication entre le script de jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="12f60-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="12f60-193">Si vous utilisez un navigateur autre que Internet Explorer, vous pouvez également accéder à la dynamique **hubs** fichier en y accédant directement, par exemple http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="12f60-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="12f60-194">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="12f60-194">Examine the Code</span></span>

<span data-ttu-id="12f60-195">L’application de conversation SignalR montre deux tâches de développement SignalR base : création d’un hub en tant que l’objet principal de coordination sur le serveur et à l’aide de la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="12f60-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="12f60-196">Concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="12f60-196">SignalR Hubs</span></span>

<span data-ttu-id="12f60-197">Dans l’exemple de code la **ChatHub** classe dérive de la **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="12f60-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="12f60-198">Dérivation à partir de la **Hub** classe est un moyen utile pour créer une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="12f60-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="12f60-199">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts dans une page web.</span><span class="sxs-lookup"><span data-stu-id="12f60-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="12f60-200">Dans le code de la conversation, les clients appellent le **ChatHub.Send** méthode pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="12f60-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="12f60-201">Le concentrateur à son tour envoie le message à tous les clients en appelant **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="12f60-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="12f60-202">Le **envoyer** méthode illustre plusieurs concepts de hub :</span><span class="sxs-lookup"><span data-stu-id="12f60-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="12f60-203">Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.</span><span class="sxs-lookup"><span data-stu-id="12f60-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="12f60-204">Utilisez le **Microsoft.AspNet.SignalR.Hub.Clients** propriété pour accéder à tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="12f60-205">Appeler une fonction sur le client (tel que le `addNewMessageToPage` (fonction)) pour mettre à jour des clients.</span><span class="sxs-lookup"><span data-stu-id="12f60-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="12f60-206">SignalR et jQuery</span><span class="sxs-lookup"><span data-stu-id="12f60-206">SignalR and jQuery</span></span>

<span data-ttu-id="12f60-207">Le **Chat.cshtml** afficher le fichier dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="12f60-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="12f60-208">Les tâches essentielles dans le code sont création d’une référence pour le proxy généré automatiquement pour le hub, déclarant une fonction que le serveur peut appeler pour transmettre le contenu aux clients et le démarrage d’une connexion pour envoyer des messages au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="12f60-209">Le code suivant déclare une référence à un concentrateur proxy.</span><span class="sxs-lookup"><span data-stu-id="12f60-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="12f60-210">Dans JavaScript, la référence à la classe de serveur et de ses membres est en casse mixte.</span><span class="sxs-lookup"><span data-stu-id="12f60-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="12f60-211">L’exemple de code fait référence à celle de C# **ChatHub** classe dans JavaScript en tant que **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="12f60-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="12f60-212">Si vous souhaitez faire référence à la `ChatHub` classe dans jQuery avec Pascal conventionnel mise en majuscules comme vous le feriez en c#, modifiez le fichier de classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="12f60-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="12f60-213">Ajouter un `using` instruction pour référencer le `Microsoft.AspNet.SignalR.Hubs` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="12f60-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="12f60-214">Ajoutez ensuite le `HubName` attribut le `ChatHub` de classe, par exemple `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="12f60-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="12f60-215">Enfin, mettez à jour de votre référence de jQuery à la `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="12f60-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="12f60-216">Le code suivant montre comment créer une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="12f60-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="12f60-217">La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="12f60-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="12f60-218">L’appel facultatif à la `htmlEncode` affiche la fonction moyen HTML encode le contenu du message avant de les afficher dans la page, comme un moyen d’empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="12f60-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="12f60-219">Le code suivant montre comment ouvrir une connexion avec le hub.</span><span class="sxs-lookup"><span data-stu-id="12f60-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="12f60-220">Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page de la conversation.</span><span class="sxs-lookup"><span data-stu-id="12f60-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="12f60-221">Cette approche garantit que la connexion est établie avant que le Gestionnaire d’événements s’exécute.</span><span class="sxs-lookup"><span data-stu-id="12f60-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="12f60-222">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="12f60-222">Next Steps</span></span>

<span data-ttu-id="12f60-223">Vous avez appris que SignalR est une infrastructure pour générer des applications web en temps réel.</span><span class="sxs-lookup"><span data-stu-id="12f60-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="12f60-224">Vous avez également appris à plusieurs tâches de développement de SignalR : comment ajouter SignalR à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="12f60-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="12f60-225">Pour une procédure pas à pas sur la façon de déployer l’exemple d’application SignalR dans Azure, consultez [à l’aide de SignalR avec Web Apps dans Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="12f60-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="12f60-226">Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur un Site Web de Windows Azure, consultez [créer une application web ASP.NET dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="12f60-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="12f60-227">Pour en savoir plus les concepts de développements SignalR plus avancés, consultez les sites suivants pour le code source de SignalR et de ressources :</span><span class="sxs-lookup"><span data-stu-id="12f60-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="12f60-228">Projet de SignalR</span><span class="sxs-lookup"><span data-stu-id="12f60-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="12f60-229">SignalR Github et exemples</span><span class="sxs-lookup"><span data-stu-id="12f60-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="12f60-230">Wiki de SignalR</span><span class="sxs-lookup"><span data-stu-id="12f60-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
