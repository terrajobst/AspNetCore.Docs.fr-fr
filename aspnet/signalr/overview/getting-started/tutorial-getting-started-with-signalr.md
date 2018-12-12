---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Didacticiel : Bien démarrer avec SignalR 2 | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel. Vous ajouter SignalR à une application de web ASP.NET vide et créerez un pa HTML...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3b06e7d0a602e061112adbceba92276f836f6311
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287336"
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="db518-104">Didacticiel : Bien démarrer avec SignalR 2</span><span class="sxs-lookup"><span data-stu-id="db518-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="db518-105">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="db518-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[<span data-ttu-id="db518-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="db518-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="db518-107">Ce didacticiel montre comment utiliser SignalR pour créer une application de conversation en temps réel.</span><span class="sxs-lookup"><span data-stu-id="db518-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="db518-108">Vous ajouter SignalR à une application de web ASP.NET vide et créer une page HTML pour envoyer et afficher des messages.</span><span class="sxs-lookup"><span data-stu-id="db518-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="db518-109">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="db518-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="db518-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="db518-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="db518-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="db518-111">.NET 4.5</span></span>
> - <span data-ttu-id="db518-112">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="db518-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="db518-113">À l’aide de Visual Studio 2012 avec ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="db518-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="db518-114">Pour utiliser Visual Studio 2012 avec ce didacticiel, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="db518-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="db518-115">Mise à jour votre [Gestionnaire de Package](http://docs.nuget.org/docs/start-here/installing-nuget) vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="db518-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="db518-116">Installer le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="db518-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="db518-117">Dans le programme Web Platform Installer, recherchez et installez **ASP.NET et Web Tools 2013.1 pour Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="db518-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="db518-118">Cela installera les modèles Visual Studio pour les classes de SignalR comme **Hub**.</span><span class="sxs-lookup"><span data-stu-id="db518-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="db518-119">Certains modèles (tels que **classe de démarrage OWIN**) ne sera pas disponible ; dans ce cas, utilisez un fichier de classe à la place.</span><span class="sxs-lookup"><span data-stu-id="db518-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="db518-120">Versions de didacticiels</span><span class="sxs-lookup"><span data-stu-id="db518-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="db518-121">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="db518-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="db518-122">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="db518-122">Questions and comments</span></span>
> 
> <span data-ttu-id="db518-123">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="db518-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="db518-124">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="db518-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="db518-125">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="db518-125">Overview</span></span>

<span data-ttu-id="db518-126">Ce didacticiel présente le développement de SignalR en montrant comment créer une application de service de conversation simple basée sur navigateur.</span><span class="sxs-lookup"><span data-stu-id="db518-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="db518-127">Vous ajoute la bibliothèque SignalR à une application de web ASP.NET vide, créez une classe de hub pour envoyer des messages aux clients et créer une page HTML qui permet aux utilisateurs d’envoyer et recevoir des messages de conversation.</span><span class="sxs-lookup"><span data-stu-id="db518-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="db518-128">Pour obtenir un didacticiel similaire qui montre comment créer une application de conversation dans MVC 5 à l’aide d’une vue MVC, consultez [bien démarrer avec SignalR 2 et MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="db518-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="db518-129">Ce didacticiel montre comment créer des applications SignalR dans la version 2.</span><span class="sxs-lookup"><span data-stu-id="db518-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="db518-130">Pour plus d’informations sur les modifications entre SignalR 1.x et 2, consultez [SignalR la mise à niveau les projets 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) et [Notes de publication de Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="db518-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="db518-131">SignalR est une bibliothèque de .NET open source pour la création d’applications web qui requièrent l’intervention de l’utilisateur en direct ou de mises à jour des données en temps réel.</span><span class="sxs-lookup"><span data-stu-id="db518-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="db518-132">Exemples : applications des réseaux sociaux, jeux multi-utilisateur, météo de collaboration et de news, entreprise ou les applications financières de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="db518-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="db518-133">Il s’agit souvent d’applications en temps réel.</span><span class="sxs-lookup"><span data-stu-id="db518-133">These are often called real-time applications.</span></span>

<span data-ttu-id="db518-134">SignalR simplifie le processus de génération d’applications en temps réel.</span><span class="sxs-lookup"><span data-stu-id="db518-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="db518-135">Il inclut une bibliothèque de serveur ASP.NET et une bibliothèque de client JavaScript pour le rendre plus facile à gérer les connexions client-serveur et d’envoyer des mises à jour de contenu aux clients.</span><span class="sxs-lookup"><span data-stu-id="db518-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="db518-136">Vous pouvez ajouter la bibliothèque SignalR à une application ASP.NET existante pour obtenir des fonctionnalités en temps réel.</span><span class="sxs-lookup"><span data-stu-id="db518-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="db518-137">Le didacticiel présente les tâches de développement SignalR suivantes :</span><span class="sxs-lookup"><span data-stu-id="db518-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="db518-138">Ajout de la bibliothèque de SignalR pour une application web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="db518-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="db518-139">Création d’une classe de hub pour envoyer le contenu vers les clients.</span><span class="sxs-lookup"><span data-stu-id="db518-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="db518-140">Création d’une classe de démarrage OWIN pour configurer l’application.</span><span class="sxs-lookup"><span data-stu-id="db518-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="db518-141">À l’aide de la bibliothèque jQuery SignalR dans une page web pour envoyer des messages et d’afficher les mises à jour à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="db518-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="db518-142">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="db518-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="db518-143">Chaque nouvel utilisateur peut publier des commentaires et voir les commentaires ajoutés après que l’utilisateur rejoint la conversation.</span><span class="sxs-lookup"><span data-stu-id="db518-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instances de conversation](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="db518-145">Sections :</span><span class="sxs-lookup"><span data-stu-id="db518-145">Sections:</span></span>

- [<span data-ttu-id="db518-146">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="db518-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="db518-147">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="db518-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="db518-148">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="db518-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="db518-149">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="db518-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="db518-150">Configurer le projet</span><span class="sxs-lookup"><span data-stu-id="db518-150">Set up the Project</span></span>

<span data-ttu-id="db518-151">Cette section montre comment utiliser Visual Studio 2013 et SignalR version 2 pour créer une application web ASP.NET vide, ajouter SignalR et créer l’application de conversation.</span><span class="sxs-lookup"><span data-stu-id="db518-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="db518-152">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="db518-152">Prerequisites:</span></span>

- <span data-ttu-id="db518-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="db518-153">Visual Studio 2013.</span></span> <span data-ttu-id="db518-154">Si vous n’avez pas Visual Studio, consultez [téléchargements ASP.NET](https://www.asp.net/downloads) pour obtenir le Visual Studio 2013 Express outil de développement gratuit.</span><span class="sxs-lookup"><span data-stu-id="db518-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="db518-155">Les étapes suivantes utilisent Visual Studio 2013 pour créer une Application Web ASP.NET vide et ajouter la bibliothèque SignalR :</span><span class="sxs-lookup"><span data-stu-id="db518-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="db518-156">Dans Visual Studio, créez une Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="db518-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer le web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="db518-158">Dans le **nouveau projet ASP.NET** fenêtre, laissez le champ **vide** sélectionné, cliquez sur **créer un projet**.</span><span class="sxs-lookup"><span data-stu-id="db518-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Créer le site web vide](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="db518-160">Dans **l’Explorateur de solutions**, cliquez sur le projet, sélectionnez **ajouter | Classe de concentrateur SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="db518-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="db518-161">Nommez la classe **ChatHub.cs** et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="db518-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="db518-162">Cette étape crée la **ChatHub** classe et l’ajoute au projet un ensemble de fichiers de script et les références d’assembly qui prennent en charge SignalR.</span><span class="sxs-lookup"><span data-stu-id="db518-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db518-163">Vous pouvez également ajouter SignalR à un projet en ouvrant le **Outils > Gestionnaire de Package NuGet > Console du Gestionnaire de Package** et en exécutant une commande :</span><span class="sxs-lookup"><span data-stu-id="db518-163">You can also add SignalR to a project by opening the **Tools > NuGet Package Manager > Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="db518-164">Si vous utilisez la console pour ajouter SignalR, créez la classe de concentrateur SignalR comme une étape distincte après avoir ajouté SignalR.</span><span class="sxs-lookup"><span data-stu-id="db518-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db518-165">Si vous utilisez Visual Studio 2012, le **classe de concentrateur SignalR (v2)** modèle ne sera pas disponible.</span><span class="sxs-lookup"><span data-stu-id="db518-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="db518-166">Vous pouvez ajouter un brut **classe** appelé `ChatHub` à la place.</span><span class="sxs-lookup"><span data-stu-id="db518-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="db518-167">Dans **l’Explorateur de solutions**, développez le nœud de Scripts.</span><span class="sxs-lookup"><span data-stu-id="db518-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="db518-168">Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="db518-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="db518-169">Remplacez le code dans le nouveau **ChatHub** classe par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="db518-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="db518-170">Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Classe de démarrage OWIN**.</span><span class="sxs-lookup"><span data-stu-id="db518-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="db518-171">Nommez la nouvelle classe `Startup` et cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="db518-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db518-172">Si vous utilisez Visual Studio 2012, le **classe de démarrage OWIN** modèle ne sera pas disponible.</span><span class="sxs-lookup"><span data-stu-id="db518-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="db518-173">Vous pouvez ajouter un brut **classe** appelé `Startup` à la place.</span><span class="sxs-lookup"><span data-stu-id="db518-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="db518-174">Modifier le contenu de la nouvelle classe de démarrage à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="db518-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="db518-175">Dans **l’Explorateur de solutions**, cliquez sur le projet, puis cliquez sur **ajouter | Page HTML**.</span><span class="sxs-lookup"><span data-stu-id="db518-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="db518-176">Nommez la nouvelle page `index.html`.</span><span class="sxs-lookup"><span data-stu-id="db518-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="db518-177">Vous devrez peut-être modifier les numéros de version pour les références aux bibliothèques JQuery et SignalR</span><span class="sxs-lookup"><span data-stu-id="db518-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="db518-178">Dans **l’Explorateur de solutions**, avec le bouton droit de la page HTML que vous venez de créer, puis cliquez sur **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="db518-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="db518-179">Remplacez le code par défaut dans la page HTML par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="db518-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db518-180">Une version ultérieure des scripts SignalR peut-être être installée par le Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="db518-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="db518-181">Vérifiez que les références de script ci-dessous correspondent aux versions des fichiers de script dans le projet (ils sera différents si vous avez ajouté à l’aide de NuGet plutôt que de l’ajout d’un concentrateur de SignalR.)</span><span class="sxs-lookup"><span data-stu-id="db518-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="db518-182">**Enregistrer tous les** pour le projet.</span><span class="sxs-lookup"><span data-stu-id="db518-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="db518-183">Exécuter l’exemple</span><span class="sxs-lookup"><span data-stu-id="db518-183">Run the Sample</span></span>

1. <span data-ttu-id="db518-184">Appuyez sur F5 pour exécuter le projet en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="db518-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="db518-185">La page HTML se charge dans une instance du navigateur et des invites pour un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="db518-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Entrer un nom d'utilisateur](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="db518-187">Entrez un nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="db518-187">Enter a user name.</span></span>
3. <span data-ttu-id="db518-188">Copiez l’URL de la ligne d’adresse du navigateur et l’utiliser pour ouvrir les deux autres instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="db518-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="db518-189">Dans chaque instance du navigateur, entrez un nom d’utilisateur unique.</span><span class="sxs-lookup"><span data-stu-id="db518-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="db518-190">Dans chaque instance du navigateur, ajoutez un commentaire et cliquez sur **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="db518-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="db518-191">Les commentaires doivent s’afficher dans toutes les instances de navigateur.</span><span class="sxs-lookup"><span data-stu-id="db518-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db518-192">Cette application de conversation simple ne conserve pas le contexte de la discussion sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="db518-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="db518-193">Le hub diffuse des commentaires à tous les utilisateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="db518-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="db518-194">Les utilisateurs qui accèdent à la conversation ultérieurement verrez messages ajoutés à partir du moment qu'où ils joignent.</span><span class="sxs-lookup"><span data-stu-id="db518-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="db518-195">La capture d’écran suivante montre l’application de conversation en cours d’exécution dans les trois instances de navigateur, qui sont mis à jour lorsqu’une instance envoie un message :</span><span class="sxs-lookup"><span data-stu-id="db518-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Navigateurs de conversation](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="db518-197">Dans **l’Explorateur de solutions**, inspecter la **Documents de Script** nœud pour l’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="db518-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="db518-198">Il existe un fichier de script nommé **hubs** que la bibliothèque SignalR génère dynamiquement lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="db518-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="db518-199">Ce fichier gère la communication entre le script de jQuery et le code côté serveur.</span><span class="sxs-lookup"><span data-stu-id="db518-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="db518-200">Examinez le Code</span><span class="sxs-lookup"><span data-stu-id="db518-200">Examine the Code</span></span>

<span data-ttu-id="db518-201">L’application de conversation SignalR montre deux tâches de développement SignalR base : création d’un hub en tant que l’objet principal de coordination sur le serveur et à l’aide de la bibliothèque jQuery de SignalR pour envoyer et recevoir des messages.</span><span class="sxs-lookup"><span data-stu-id="db518-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="db518-202">Concentrateurs SignalR</span><span class="sxs-lookup"><span data-stu-id="db518-202">SignalR Hubs</span></span>

<span data-ttu-id="db518-203">Dans l’exemple de code la **ChatHub** classe dérive de la **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="db518-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="db518-204">Dérivation à partir de la **Hub** classe est un moyen utile pour créer une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="db518-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="db518-205">Vous pouvez créer des méthodes publiques sur votre classe de concentrateur et ensuite accéder à ces méthodes en les appelant à partir de scripts dans une page web.</span><span class="sxs-lookup"><span data-stu-id="db518-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="db518-206">Dans le code de la conversation, les clients appellent le **ChatHub.Send** méthode pour envoyer un nouveau message.</span><span class="sxs-lookup"><span data-stu-id="db518-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="db518-207">Le concentrateur à son tour envoie le message à tous les clients en appelant **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="db518-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="db518-208">Le **envoyer** méthode illustre plusieurs concepts de hub :</span><span class="sxs-lookup"><span data-stu-id="db518-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="db518-209">Déclarer des méthodes publiques sur un concentrateur afin que les clients peuvent appeler les.</span><span class="sxs-lookup"><span data-stu-id="db518-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="db518-210">Utilisez le **Microsoft.AspNet.SignalR.Hub.Clients** propriété dynamique à accéder à tous les clients connectés à ce concentrateur.</span><span class="sxs-lookup"><span data-stu-id="db518-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="db518-211">Appeler une fonction sur le client (tel que le `broadcastMessage` (fonction)) pour mettre à jour des clients.</span><span class="sxs-lookup"><span data-stu-id="db518-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="db518-212">SignalR et jQuery</span><span class="sxs-lookup"><span data-stu-id="db518-212">SignalR and jQuery</span></span>

<span data-ttu-id="db518-213">La page HTML dans l’exemple de code montre comment utiliser la bibliothèque jQuery de SignalR pour communiquer avec un concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="db518-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="db518-214">Les tâches essentielles dans le code déclarez un proxy pour référencer le hub, la déclaration d’une fonction que le serveur peut appeler pour transmettre le contenu aux clients et le démarrage d’une connexion pour envoyer des messages au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="db518-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="db518-215">Le code suivant déclare une référence à un concentrateur proxy.</span><span class="sxs-lookup"><span data-stu-id="db518-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="db518-216">Dans JavaScript, la référence à la classe de serveur et de ses membres est en casse mixte.</span><span class="sxs-lookup"><span data-stu-id="db518-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="db518-217">L’exemple de code fait référence à celle de C# **ChatHub** classe dans JavaScript en tant que **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="db518-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="db518-218">Le code suivant est la façon dont vous créez une fonction de rappel dans le script.</span><span class="sxs-lookup"><span data-stu-id="db518-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="db518-219">La classe de concentrateur sur le serveur appelle cette fonction pour envoyer des mises à jour de contenu à chaque client.</span><span class="sxs-lookup"><span data-stu-id="db518-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="db518-220">Les deux lignes qu’encoder en HTML le contenu avant de les afficher sont facultatifs et affichent un moyen simple d’empêcher l’injection de script.</span><span class="sxs-lookup"><span data-stu-id="db518-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="db518-221">Le code suivant montre comment ouvrir une connexion avec le hub.</span><span class="sxs-lookup"><span data-stu-id="db518-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="db518-222">Le code démarre la connexion et passe une fonction pour gérer l’événement de clic sur le **envoyer** bouton dans la page HTML.</span><span class="sxs-lookup"><span data-stu-id="db518-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="db518-223">Cette approche garantit que la connexion est établie avant que le Gestionnaire d’événements s’exécute.</span><span class="sxs-lookup"><span data-stu-id="db518-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="db518-224">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="db518-224">Next Steps</span></span>

<span data-ttu-id="db518-225">Vous avez appris que SignalR est une infrastructure pour générer des applications web en temps réel.</span><span class="sxs-lookup"><span data-stu-id="db518-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="db518-226">Vous avez également appris à plusieurs tâches de développement de SignalR : comment ajouter SignalR à une application ASP.NET, comment créer une classe de concentrateur et comment envoyer et recevoir des messages à partir du concentrateur.</span><span class="sxs-lookup"><span data-stu-id="db518-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="db518-227">Pour une procédure pas à pas sur la façon de déployer l’exemple d’application SignalR dans Azure, consultez [à l’aide de SignalR avec Web Apps dans Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="db518-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="db518-228">Pour plus d’informations sur la façon de déployer un projet web Visual Studio sur un Site Web de Windows Azure, consultez [créer une application web ASP.NET dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="db518-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="db518-229">Pour en savoir plus les concepts de développements SignalR plus avancés, consultez les sites suivants pour le code source de SignalR et de ressources :</span><span class="sxs-lookup"><span data-stu-id="db518-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="db518-230">Projet de SignalR</span><span class="sxs-lookup"><span data-stu-id="db518-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="db518-231">SignalR Github et exemples</span><span class="sxs-lookup"><span data-stu-id="db518-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="db518-232">Wiki de SignalR</span><span class="sxs-lookup"><span data-stu-id="db518-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
