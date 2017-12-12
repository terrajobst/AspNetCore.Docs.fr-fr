---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: "Héberger OWIN dans un rôle de travail Azure | Documents Microsoft"
author: MikeWasson
description: "Ce didacticiel montre comment auto-hébergement OWIN dans un rôle de travail Microsoft Azure. Interface Web ouverte pour .NET (OWIN) définit une abstraction entre le serveur web .NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 647514ae5a92b9d729179327fb97bd8005b0a4b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="f38b8-104">Hôte OWIN dans un rôle de travail Azure</span><span class="sxs-lookup"><span data-stu-id="f38b8-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="f38b8-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f38b8-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f38b8-106">Ce didacticiel montre comment auto-hébergement OWIN dans un rôle de travail Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f38b8-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="f38b8-107">[Ouvrir l’Interface Web pour .NET](http://owin.org/) (OWIN) définit une abstraction entre les serveurs web .NET et des applications web.</span><span class="sxs-lookup"><span data-stu-id="f38b8-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f38b8-108">OWIN dissocie l’application web à partir du serveur, ce qui rend OWIN idéale pour l’auto-hébergement une application web dans votre propre processus, en dehors d’IIS – par exemple, à l’intérieur d’un rôle de travail Azure.</span><span class="sxs-lookup"><span data-stu-id="f38b8-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="f38b8-109">Dans ce didacticiel, vous allez apprendre à auto-héberger un applications OWIN à l’intérieur d’un rôle de travail Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f38b8-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="f38b8-110">Pour en savoir plus sur les rôles de travail, consultez [modèles d’exécution Azure](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="f38b8-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f38b8-111">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="f38b8-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f38b8-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f38b8-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="f38b8-113">Azure SDK pour .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="f38b8-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/en-us/downloads/)
> - [<span data-ttu-id="f38b8-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="f38b8-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="f38b8-115">Créez un projet Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f38b8-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="f38b8-116">Démarrez Visual Studio avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="f38b8-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="f38b8-117">Des privilèges d’administrateur sont nécessaires pour déboguer l’application localement, à l’aide de l’émulateur de calcul Azure.</span><span class="sxs-lookup"><span data-stu-id="f38b8-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="f38b8-118">Sur le **fichier** menu, cliquez sur **nouveau**, puis cliquez sur **projet**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="f38b8-119">À partir de **modèles installés**, sous Visual c#, cliquez sur **Cloud** puis cliquez sur **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="f38b8-120">Nommez le projet « AzureApp » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="f38b8-121">Dans le **nouveau Windows Azure Cloud Service** boîte de dialogue, double-cliquez sur **rôle de travail**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="f38b8-122">Laissez le nom par défaut (« WorkerRole1 »).</span><span class="sxs-lookup"><span data-stu-id="f38b8-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="f38b8-123">Cette étape ajoute un rôle de travail à la solution.</span><span class="sxs-lookup"><span data-stu-id="f38b8-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="f38b8-124">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="f38b8-125">La solution Visual Studio qui est créée contient deux projets :</span><span class="sxs-lookup"><span data-stu-id="f38b8-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="f38b8-126">&quot;AzureApp&quot; définit les rôles et la configuration de l’application Azure.</span><span class="sxs-lookup"><span data-stu-id="f38b8-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="f38b8-127">&quot;WorkerRole1&quot; contient le code pour le rôle de travail.</span><span class="sxs-lookup"><span data-stu-id="f38b8-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="f38b8-128">En règle générale, une application Windows Azure peut contenir plusieurs rôles, bien que ce didacticiel utilise un seul rôle.</span><span class="sxs-lookup"><span data-stu-id="f38b8-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="f38b8-129">Ajouter les Packages d’auto-hébergement OWIN</span><span class="sxs-lookup"><span data-stu-id="f38b8-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="f38b8-130">À partir de la **outils** menu, cliquez sur **Gestionnaire de Package de bibliothèque**, puis cliquez sur **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="f38b8-131">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f38b8-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="f38b8-132">Ajouter un point de terminaison HTTP</span><span class="sxs-lookup"><span data-stu-id="f38b8-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="f38b8-133">Dans l’Explorateur de solutions, développez le projet AzureApp.</span><span class="sxs-lookup"><span data-stu-id="f38b8-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="f38b8-134">Développez le nœud rôles, cliquez sur WorkerRole1, puis sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="f38b8-135">Cliquez sur **points de terminaison**, puis cliquez sur **ajouter le point de terminaison**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="f38b8-136">Dans le **protocole** liste déroulante, sélectionnez « http ».</span><span class="sxs-lookup"><span data-stu-id="f38b8-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="f38b8-137">Dans **Port Public** et **Port privé**, tapez 80.</span><span class="sxs-lookup"><span data-stu-id="f38b8-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="f38b8-138">Ces numéros de port peut être différent.</span><span class="sxs-lookup"><span data-stu-id="f38b8-138">These port numbers can be different.</span></span> <span data-ttu-id="f38b8-139">Le port public est que les clients utilisent lorsqu’ils envoient une demande pour le rôle.</span><span class="sxs-lookup"><span data-stu-id="f38b8-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="f38b8-140">Créer la classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="f38b8-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="f38b8-141">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet WorkerRole1 et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe.</span><span class="sxs-lookup"><span data-stu-id="f38b8-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f38b8-142">Nommez la classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f38b8-142">Name the class `Startup`.</span></span>

<span data-ttu-id="f38b8-143">Remplacez tout le code réutilisable avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="f38b8-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="f38b8-144">Le `UseWelcomePage` méthode d’extension ajoute une page HTML à votre application, pour vérifier le site fonctionne.</span><span class="sxs-lookup"><span data-stu-id="f38b8-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="f38b8-145">Démarrer l’hôte OWIN</span><span class="sxs-lookup"><span data-stu-id="f38b8-145">Start the OWIN Host</span></span>

<span data-ttu-id="f38b8-146">Ouvrez le fichier WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="f38b8-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="f38b8-147">Cette classe définit le code qui s’exécute lorsque le rôle de travail est démarré et arrêté.</span><span class="sxs-lookup"><span data-stu-id="f38b8-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="f38b8-148">Ajoutez le code suivant à l’aide d’instruction :</span><span class="sxs-lookup"><span data-stu-id="f38b8-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="f38b8-149">Ajouter un **IDisposable** membre à la `WorkerRole` classe :</span><span class="sxs-lookup"><span data-stu-id="f38b8-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="f38b8-150">Dans le `OnStart` (méthode), ajoutez le code suivant pour démarrer l’ordinateur hôte :</span><span class="sxs-lookup"><span data-stu-id="f38b8-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="f38b8-151">Le **WebApp.Start** méthode démarre l’hôte OWIN.</span><span class="sxs-lookup"><span data-stu-id="f38b8-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="f38b8-152">Le nom de la `Startup` classe est un paramètre de type à la méthode.</span><span class="sxs-lookup"><span data-stu-id="f38b8-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="f38b8-153">Par convention, l’hôte appelle la `Configure` méthode de cette classe.</span><span class="sxs-lookup"><span data-stu-id="f38b8-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="f38b8-154">Remplacer la `OnStop` pour les supprimer de la  *\_application* instance :</span><span class="sxs-lookup"><span data-stu-id="f38b8-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="f38b8-155">Voici le code complet pour WorkerRole.cs :</span><span class="sxs-lookup"><span data-stu-id="f38b8-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="f38b8-156">Générez la solution, appuyez sur F5 pour exécuter l’application localement dans l’émulateur de calcul Azure.</span><span class="sxs-lookup"><span data-stu-id="f38b8-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="f38b8-157">En fonction de vos paramètres de pare-feu, vous devrez peut-être autoriser l’émulateur dans votre pare-feu.</span><span class="sxs-lookup"><span data-stu-id="f38b8-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="f38b8-158">L’émulateur de calcul affecte une adresse IP locale pour le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="f38b8-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="f38b8-159">Vous pouvez rechercher l’adresse IP en affichant l’interface d’émulateur de calcul.</span><span class="sxs-lookup"><span data-stu-id="f38b8-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="f38b8-160">Cliquez sur l’icône de l’émulateur dans la zone de notification de la barre des tâches, puis sélectionnez **Show Compute Emulator UI**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="f38b8-161">Rechercher l’adresse IP dans les déploiements de Service, déploiement [id], détails du Service.</span><span class="sxs-lookup"><span data-stu-id="f38b8-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="f38b8-162">Ouvrez un navigateur web et accédez à http://*adresse*, où *adresse* est l’adresse IP affectée par l’émulateur de calcul ; par exemple, `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="f38b8-162">Open a web browser and navigate to http://*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="f38b8-163">Vous devez voir la page d’accueil OWIN :</span><span class="sxs-lookup"><span data-stu-id="f38b8-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="f38b8-164">Déployer vers Azure</span><span class="sxs-lookup"><span data-stu-id="f38b8-164">Deploy to Azure</span></span>

<span data-ttu-id="f38b8-165">Pour cette étape, vous devez disposer d’un compte Azure.</span><span class="sxs-lookup"><span data-stu-id="f38b8-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="f38b8-166">Si vous n’en avez déjà, vous pouvez créer un compte d’évaluation gratuit en quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="f38b8-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f38b8-167">Pour plus d’informations, consultez [version d’évaluation gratuite de Microsoft Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="f38b8-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="f38b8-168">Dans l’Explorateur de solutions, cliquez sur le projet AzureApp.</span><span class="sxs-lookup"><span data-stu-id="f38b8-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="f38b8-169">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="f38b8-170">Si vous n’êtes pas connecté à votre compte Azure, cliquez sur **connexion**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="f38b8-171">Une fois que vous êtes connecté, choisissez un abonnement, puis cliquez sur **suivant**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="f38b8-172">Entrez un nom pour le service cloud et choisissez une région.</span><span class="sxs-lookup"><span data-stu-id="f38b8-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="f38b8-173">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="f38b8-174">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="f38b8-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="f38b8-175">La fenêtre de journal des activités Azure affiche la progression du déploiement.</span><span class="sxs-lookup"><span data-stu-id="f38b8-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="f38b8-176">Lorsque l’application est déployée, accédez à `http://appname.cloudapp.net/`, où *appname* est le nom de votre service cloud.</span><span class="sxs-lookup"><span data-stu-id="f38b8-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f38b8-177">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f38b8-177">Additional Resources</span></span>

- [<span data-ttu-id="f38b8-178">Une vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="f38b8-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="f38b8-179">Projet Katana sur GitHub</span><span class="sxs-lookup"><span data-stu-id="f38b8-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
