---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Didacticiel : Auto-hébergement de SignalR | Microsoft Docs'
author: pfletcher
description: Ce didacticiel montre comment créer un serveur de SignalR 2 auto-hébergé et comment s’y connecter avec un client JavaScript. Versions des logiciels utilisées dans le didacticiel V...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 5d7d485357a6c820f11e0135e2ff9479c1965d96
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827699"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="534ec-104">Didacticiel : Auto-hébergement de SignalR</span><span class="sxs-lookup"><span data-stu-id="534ec-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="534ec-105">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="534ec-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="534ec-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="534ec-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="534ec-107">Ce didacticiel montre comment créer un serveur de SignalR 2 auto-hébergé et comment s’y connecter avec un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="534ec-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="534ec-108">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="534ec-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="534ec-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="534ec-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="534ec-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="534ec-110">.NET 4.5</span></span>
> - <span data-ttu-id="534ec-111">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="534ec-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="534ec-112">À l’aide de Visual Studio 2012 avec ce didacticiel</span><span class="sxs-lookup"><span data-stu-id="534ec-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="534ec-113">Pour utiliser Visual Studio 2012 avec ce didacticiel, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="534ec-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="534ec-114">Mise à jour votre [Gestionnaire de Package](http://docs.nuget.org/docs/start-here/installing-nuget) vers la dernière version.</span><span class="sxs-lookup"><span data-stu-id="534ec-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="534ec-115">Installer le [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="534ec-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="534ec-116">Dans le programme Web Platform Installer, recherchez et installez **ASP.NET et Web Tools 2013.1 pour Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="534ec-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="534ec-117">Cela installera les modèles Visual Studio pour les classes de SignalR comme **Hub**.</span><span class="sxs-lookup"><span data-stu-id="534ec-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="534ec-118">Certains modèles (tels que **classe de démarrage OWIN**) ne sera pas disponible ; dans ce cas, utilisez un fichier de classe à la place.</span><span class="sxs-lookup"><span data-stu-id="534ec-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="534ec-119">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="534ec-119">Questions and comments</span></span>
> 
> <span data-ttu-id="534ec-120">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="534ec-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="534ec-121">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="534ec-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="534ec-122">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="534ec-122">Overview</span></span>

<span data-ttu-id="534ec-123">Un serveur SignalR est généralement hébergé dans une application ASP.NET dans IIS, mais il peut également être auto-hébergé (comme dans une application console ou un service de Windows) à l’aide de la bibliothèque Self-host.</span><span class="sxs-lookup"><span data-stu-id="534ec-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="534ec-124">Cette bibliothèque, comme tous SignalR 2, est basée sur OWIN ([Open Web Interface pour .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="534ec-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="534ec-125">OWIN définit une abstraction entre les serveurs web de .NET et des applications web.</span><span class="sxs-lookup"><span data-stu-id="534ec-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="534ec-126">OWIN dissocie de l’application web à partir du serveur, ce qui rend OWIN idéale pour l’hébergement automatique d’une application web dans votre propre processus, en dehors d’IIS.</span><span class="sxs-lookup"><span data-stu-id="534ec-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="534ec-127">Voici quelques raisons pour ne pas d’hébergement dans IIS :</span><span class="sxs-lookup"><span data-stu-id="534ec-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="534ec-128">Environnements où IIS n’est pas disponible ou souhaitable, par exemple une batterie de serveurs existante sans IIS.</span><span class="sxs-lookup"><span data-stu-id="534ec-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="534ec-129">La surcharge de performances d’IIS doit être évitée.</span><span class="sxs-lookup"><span data-stu-id="534ec-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="534ec-130">Fonctionnalité de SignalR consiste à être ajouté à une application existante qui s’exécute dans un Service Windows, rôle de travail Azure ou autre processus.</span><span class="sxs-lookup"><span data-stu-id="534ec-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="534ec-131">Si une solution est en cours de développement en tant que Self-host pour des raisons de performances, il est recommandé pour le test également l’application hébergée dans IIS pour déterminer l’amélioration des performances.</span><span class="sxs-lookup"><span data-stu-id="534ec-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="534ec-132">Ce didacticiel contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="534ec-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="534ec-133">Création du serveur</span><span class="sxs-lookup"><span data-stu-id="534ec-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="534ec-134">L’accès au serveur avec un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="534ec-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="534ec-135">Création du serveur</span><span class="sxs-lookup"><span data-stu-id="534ec-135">Creating the server</span></span>

<span data-ttu-id="534ec-136">Dans ce didacticiel, vous allez créer un serveur qui est hébergé dans une application console, mais le serveur peut être hébergé dans une forme quelconque de processus, tel qu’un service de Windows ou le rôle de travail Azure.</span><span class="sxs-lookup"><span data-stu-id="534ec-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="534ec-137">Pour l’exemple de code pour l’hébergement d’un serveur de SignalR dans un Service Windows, consultez [SignalR Self-Hosting dans un Service Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="534ec-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="534ec-138">Ouvrez Visual Studio 2013 avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="534ec-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="534ec-139">Sélectionnez **fichier**, **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="534ec-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="534ec-140">Sélectionnez **Windows** sous le **Visual C#** nœud dans le **modèles** volet, puis sélectionnez le **Application Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="534ec-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="534ec-141">Nommez le nouveau projet « SignalRSelfHost » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="534ec-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="534ec-142">Ouvrez la console de gestionnaire de package de bibliothèque en sélectionnant **outils**, **Library Package Manager**, **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="534ec-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="534ec-143">Dans la console Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="534ec-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="534ec-144">Cette commande ajoute les bibliothèques de SignalR 2 auto-héberger au projet.</span><span class="sxs-lookup"><span data-stu-id="534ec-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="534ec-145">Dans la console Gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="534ec-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="534ec-146">Cette commande ajoute la bibliothèque Microsoft.Owin.Cors au projet.</span><span class="sxs-lookup"><span data-stu-id="534ec-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="534ec-147">Cette bibliothèque est utilisée pour la prise en charge de domaines, qui est requis pour les applications qui hébergent SignalR et un client de la page web dans des domaines différents.</span><span class="sxs-lookup"><span data-stu-id="534ec-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="534ec-148">Dans la mesure où vous hébergerez le serveur SignalR et le client web sur des ports différents, cela signifie qu’inter-domaines doit être activé pour la communication entre ces composants.</span><span class="sxs-lookup"><span data-stu-id="534ec-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="534ec-149">Remplacez le contenu de Program.cs par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="534ec-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="534ec-150">Le code ci-dessus comprend trois classes :</span><span class="sxs-lookup"><span data-stu-id="534ec-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="534ec-151">**Programme**, y compris le **Main** méthode définissant le chemin d’accès principal d’exécution.</span><span class="sxs-lookup"><span data-stu-id="534ec-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="534ec-152">Dans cette méthode, une application web de type **démarrage** est démarré à l’URL spécifiée (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="534ec-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="534ec-153">Si la sécurité est requise sur le point de terminaison, SSL peut être implémenté.</span><span class="sxs-lookup"><span data-stu-id="534ec-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="534ec-154">Consultez [Comment : configurer un Port avec un certificat SSL](https://msdn.microsoft.com/library/ms733791.aspx) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="534ec-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="534ec-155">**Démarrage**, la classe contenant la configuration pour le serveur de SignalR (la seule configuration de ce didacticiel utilise est l’appel à `UseCors`) et l’appel à `MapSignalR`, ce qui crée des itinéraires pour tous les objets Hub dans le projet.</span><span class="sxs-lookup"><span data-stu-id="534ec-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="534ec-156">**Monconcentrateur**, la classe de concentrateur SignalR l’application fournira aux clients.</span><span class="sxs-lookup"><span data-stu-id="534ec-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="534ec-157">Cette classe a une méthode unique, **envoyer**, que les clients appelleront pour diffuser un message à tous les autres clients connectés.</span><span class="sxs-lookup"><span data-stu-id="534ec-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="534ec-158">Compilez et exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="534ec-158">Compile and run the application.</span></span> <span data-ttu-id="534ec-159">L’adresse que le serveur est en cours d’exécution doit afficher dans une fenêtre de console.</span><span class="sxs-lookup"><span data-stu-id="534ec-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="534ec-160">Si l’exécution échoue avec l’exception `System.Reflection.TargetInvocationException was unhandled`, vous devez redémarrer Visual Studio avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="534ec-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="534ec-161">Arrêter l’application avant de passer à la section suivante.</span><span class="sxs-lookup"><span data-stu-id="534ec-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="534ec-162">L’accès au serveur avec un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="534ec-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="534ec-163">Dans cette section, vous allez utiliser le même client JavaScript à partir de la [didacticiel mise en route](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="534ec-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="534ec-164">Nous veillerons à ce seulement une modification au client, qui consiste à définir explicitement l’URL du hub.</span><span class="sxs-lookup"><span data-stu-id="534ec-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="534ec-165">Avec une application auto-hébergée, le serveur nécessairement peut-être pas à la même adresse en tant que l’URL de connexion (en raison des proxys inverses et les équilibreurs de charge), par conséquent, l’URL doit être défini explicitement.</span><span class="sxs-lookup"><span data-stu-id="534ec-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="534ec-166">Dans **l’Explorateur de solutions**, avec le bouton droit sur la solution et sélectionnez **ajouter**, **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="534ec-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="534ec-167">Sélectionnez le **Web** nœud, puis sélectionnez le **Application Web ASP.NET** modèle.</span><span class="sxs-lookup"><span data-stu-id="534ec-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="534ec-168">Nommez le projet « JavascriptClient » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="534ec-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="534ec-169">Sélectionnez le **vide** modèle et laissez les options restantes désélectionnées.</span><span class="sxs-lookup"><span data-stu-id="534ec-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="534ec-170">Sélectionnez **créer le projet**.</span><span class="sxs-lookup"><span data-stu-id="534ec-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="534ec-171">Dans la console Gestionnaire de package, sélectionnez le projet « JavascriptClient » dans le **projet par défaut** liste déroulante et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="534ec-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="534ec-172">Cette commande installe les bibliothèques de SignalR et JQuery dont vous aurez besoin dans le client.</span><span class="sxs-lookup"><span data-stu-id="534ec-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="534ec-173">Avec le bouton droit sur votre projet, puis sélectionnez **ajouter**, **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="534ec-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="534ec-174">Sélectionnez le **Web** nœud et sélectionner la HTML Page.</span><span class="sxs-lookup"><span data-stu-id="534ec-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="534ec-175">Nommez la page **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="534ec-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="534ec-176">Remplacez le contenu de la nouvelle page HTML par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="534ec-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="534ec-177">Vérifiez que les références de script ici correspondent les scripts dans le dossier Scripts du projet.</span><span class="sxs-lookup"><span data-stu-id="534ec-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="534ec-178">Le code suivant (mis en surbrillance dans l’exemple de code ci-dessus) est l’ajout que vous avez apportées au client utilisé dans le didacticiel bien regardais (en plus de mise à niveau le code vers SignalR version 2 Bêta).</span><span class="sxs-lookup"><span data-stu-id="534ec-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="534ec-179">Cette ligne de code définit explicitement l’URL de connexion de base pour SignalR sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="534ec-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="534ec-180">Avec le bouton droit sur la solution, puis sélectionnez **définir les projets de démarrage...** . Sélectionnez le **plusieurs projets de démarrage** case d’option et définissez des deux projets **Action** à **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="534ec-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="534ec-181">Avec le bouton droit sur « Default.html » et sélectionnez **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="534ec-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="534ec-182">Exécutez l'application.</span><span class="sxs-lookup"><span data-stu-id="534ec-182">Run the application.</span></span> <span data-ttu-id="534ec-183">Lancement la page et le serveur.</span><span class="sxs-lookup"><span data-stu-id="534ec-183">The server and page will launch.</span></span> <span data-ttu-id="534ec-184">Vous devrez peut-être recharger la page web (ou sélectionnez **continuer** dans le débogueur) si la page se charge avant que le serveur est démarré.</span><span class="sxs-lookup"><span data-stu-id="534ec-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="534ec-185">Dans le navigateur, fournissez un nom d’utilisateur lorsque vous y êtes invité.</span><span class="sxs-lookup"><span data-stu-id="534ec-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="534ec-186">Copier l’URL de la page dans une autre fenêtre ou un onglet de navigateur et fournir un nom d’utilisateur différent.</span><span class="sxs-lookup"><span data-stu-id="534ec-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="534ec-187">Vous serez en mesure d’envoyer des messages à partir du volet d’un navigateur à l’autre, comme dans le didacticiel de mise en route.</span><span class="sxs-lookup"><span data-stu-id="534ec-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
