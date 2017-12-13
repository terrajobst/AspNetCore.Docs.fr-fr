---
uid: signalr/overview/performance/scaleout-with-sql-server
title: "Montée en charge SignalR avec SQL Server | Documents Microsoft"
author: MikeWasson
description: "Versions du logiciel utilisé dans cette rubrique Visual Studio 2013 .NET 4.5 SignalR les versions précédentes de la version 2 de cette rubrique pour plus d’informations sur les versions antérieures de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5bf625a1ef8cc8ceab0014fadfab0c8a23dbc8da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="9847a-103">Montée en charge SignalR avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="9847a-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="9847a-104">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="9847a-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="9847a-105">Versions du logiciel utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="9847a-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="9847a-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9847a-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="9847a-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9847a-107">.NET 4.5</span></span>
> - <span data-ttu-id="9847a-108">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="9847a-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="9847a-109">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="9847a-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="9847a-110">Pour plus d’informations sur les versions antérieures de SignalR, consultez [SignalR des Versions antérieures](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="9847a-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="9847a-111">Questions et des commentaires</span><span class="sxs-lookup"><span data-stu-id="9847a-111">Questions and comments</span></span>
> 
> <span data-ttu-id="9847a-112">Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="9847a-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="9847a-113">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les valider pour le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="9847a-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="9847a-114">Dans ce didacticiel, vous utiliserez SQL Server pour distribuer des messages entre une application SignalR qui est déployée dans deux instances distinctes d’IIS.</span><span class="sxs-lookup"><span data-stu-id="9847a-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="9847a-115">Vous pouvez également exécuter ce didacticiel sur un ordinateur de test unique, mais pour obtenir l’effet, vous devez déployer l’application SignalR à deux ou plusieurs serveurs.</span><span class="sxs-lookup"><span data-stu-id="9847a-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="9847a-116">Vous devez également installer SQL Server sur l’un des serveurs ou sur un serveur dédié distinct.</span><span class="sxs-lookup"><span data-stu-id="9847a-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="9847a-117">Une autre option consiste à exécuter le didacticiel à l’aide de machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="9847a-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="9847a-118">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="9847a-118">Prerequisites</span></span>

<span data-ttu-id="9847a-119">Microsoft SQL Server 2005 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="9847a-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="9847a-120">Le fond de panier prend en charge les éditions de bureau et serveur de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9847a-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="9847a-121">Il ne prend pas en charge SQL Server Compact Edition ou la base de données SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="9847a-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="9847a-122">(Si votre application est hébergée sur Azure, envisagez le fond de panier de Service Bus à la place.)</span><span class="sxs-lookup"><span data-stu-id="9847a-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="9847a-123">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="9847a-123">Overview</span></span>

<span data-ttu-id="9847a-124">Avant du didacticiel détaillé, voici une vue d’ensemble rapide des opérations à effectuer.</span><span class="sxs-lookup"><span data-stu-id="9847a-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="9847a-125">Créer une nouvelle base de données vide.</span><span class="sxs-lookup"><span data-stu-id="9847a-125">Create a new empty database.</span></span> <span data-ttu-id="9847a-126">Le fond de panier créera les tables nécessaires dans cette base de données.</span><span class="sxs-lookup"><span data-stu-id="9847a-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="9847a-127">Ajoutez ces packages NuGet pour votre application :</span><span class="sxs-lookup"><span data-stu-id="9847a-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="9847a-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="9847a-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="9847a-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="9847a-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="9847a-130">Créez une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="9847a-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="9847a-131">Ajoutez le code suivant à Startup.cs pour configurer l’infrastructure d’intégration :</span><span class="sxs-lookup"><span data-stu-id="9847a-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 <span data-ttu-id="9847a-132">Ce code configure le fond de panier avec les valeurs par défaut [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) et [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="9847a-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="9847a-133">Pour plus d’informations sur la modification de ces valeurs, consultez [SignalR performances : montée en puissance parallèle métriques](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="9847a-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="9847a-134">Configurer la base de données</span><span class="sxs-lookup"><span data-stu-id="9847a-134">Configure the Database</span></span>

<span data-ttu-id="9847a-135">Décider si l’application doit utiliser l’authentification Windows ou authentification SQL Server pour accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="9847a-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="9847a-136">Tous les cas, assurez-vous que l’utilisateur de base de données dispose des autorisations pour vous connecter, de créer des schémas et de créer des tables.</span><span class="sxs-lookup"><span data-stu-id="9847a-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="9847a-137">Créer une nouvelle base de données pour l’infrastructure d’intégration à utiliser.</span><span class="sxs-lookup"><span data-stu-id="9847a-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="9847a-138">Vous pouvez donner à la base de données n’importe quel nom.</span><span class="sxs-lookup"><span data-stu-id="9847a-138">You can give the database any name.</span></span> <span data-ttu-id="9847a-139">Vous n’avez pas besoin de créer des tables dans la base de données ; l’infrastructure d’intégration va créer les tables nécessaires.</span><span class="sxs-lookup"><span data-stu-id="9847a-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="9847a-140">Activer Service Broker</span><span class="sxs-lookup"><span data-stu-id="9847a-140">Enable Service Broker</span></span>

<span data-ttu-id="9847a-141">Il est recommandé d’activer Service Broker pour la base de données de fond de panier.</span><span class="sxs-lookup"><span data-stu-id="9847a-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="9847a-142">Service Broker fournit une prise en charge native pour la messagerie et de files d’attente dans SQL Server, ce qui permet de recevoir des mises à jour plus efficacement le fond de panier.</span><span class="sxs-lookup"><span data-stu-id="9847a-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="9847a-143">(Toutefois, le fond de panier fonctionne également sans Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="9847a-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="9847a-144">Pour vérifier si le Service Broker est activé, interrogez la **est\_broker\_activé** colonne dans la **sys.databases** vue de catalogue.</span><span class="sxs-lookup"><span data-stu-id="9847a-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="9847a-145">Pour activer Service Broker, utilisez la requête SQL suivante :</span><span class="sxs-lookup"><span data-stu-id="9847a-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="9847a-146">Si cette requête s’affiche en interblocage, vérifiez qu’il n’y a aucune application connectée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="9847a-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="9847a-147">Si vous avez activé le suivi, les traces affiche également si le Service Broker est activé.</span><span class="sxs-lookup"><span data-stu-id="9847a-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="9847a-148">Créer une Application SignalR</span><span class="sxs-lookup"><span data-stu-id="9847a-148">Create a SignalR Application</span></span>

<span data-ttu-id="9847a-149">Créer une application SignalR en suivant une de ces didacticiels :</span><span class="sxs-lookup"><span data-stu-id="9847a-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="9847a-150">Mise en route avec SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="9847a-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="9847a-151">Prise en main SignalR 2.0 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="9847a-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="9847a-152">Ensuite, nous allons modifier l’application de conversation pour prendre en charge la montée en puissance parallèle avec SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9847a-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="9847a-153">Tout d’abord, ajoutez le package NuGet de SignalR.SqlServer à votre projet.</span><span class="sxs-lookup"><span data-stu-id="9847a-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="9847a-154">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package de bibliothèque**, puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="9847a-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="9847a-155">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9847a-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="9847a-156">Ensuite, ouvrez le fichier Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="9847a-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="9847a-157">Ajoutez le code suivant à la **configurer** méthode :</span><span class="sxs-lookup"><span data-stu-id="9847a-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="9847a-158">Déployer et exécuter l’Application</span><span class="sxs-lookup"><span data-stu-id="9847a-158">Deploy and Run the Application</span></span>

<span data-ttu-id="9847a-159">Préparez vos instances de Windows Server pour déployer l’application SignalR.</span><span class="sxs-lookup"><span data-stu-id="9847a-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="9847a-160">Ajouter le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="9847a-160">Add the IIS role.</span></span> <span data-ttu-id="9847a-161">Inclure les fonctionnalités de « Développement d’applications », y compris le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="9847a-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="9847a-162">Incluent également le Service de gestion (répertoriées sous « Outils de gestion »).</span><span class="sxs-lookup"><span data-stu-id="9847a-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="9847a-163">**Installer Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="9847a-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="9847a-164">Lorsque vous exécutez le Gestionnaire des services Internet, il vous invite à installer Microsoft Web Platform, ou vous pouvez [télécharger l’intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="9847a-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="9847a-165">Dans le programme d’installation de la plateforme, Web Deploy rechercher et installer Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="9847a-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="9847a-166">Vérifiez que le Service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="9847a-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="9847a-167">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="9847a-167">If not, start the service.</span></span> <span data-ttu-id="9847a-168">(Si vous ne voyez pas Service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le Service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="9847a-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="9847a-169">Enfin, ouvrez le port 8172 pour TCP.</span><span class="sxs-lookup"><span data-stu-id="9847a-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="9847a-170">Il s’agit de port utilisé par l’outil Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="9847a-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="9847a-171">Vous êtes maintenant prêt à déployer le projet de Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9847a-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="9847a-172">Dans l’Explorateur de solutions, avec le bouton droit de la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="9847a-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="9847a-173">Pour plus d’informations de déploiement web, consultez [plan de contenu de déploiement Web pour Visual Studio et ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="9847a-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="9847a-174">Si vous déployez l’application à deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent des messages SignalR à partir de l’autre.</span><span class="sxs-lookup"><span data-stu-id="9847a-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="9847a-175">(Bien entendu, dans un environnement de production, les deux serveurs sont placé derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="9847a-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="9847a-176">Après avoir exécuté l’application, vous pouvez voir que SignalR a créé automatiquement une table dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="9847a-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="9847a-177">SignalR gère les tables.</span><span class="sxs-lookup"><span data-stu-id="9847a-177">SignalR manages the tables.</span></span> <span data-ttu-id="9847a-178">Tant que votre application est déployée, ne pas supprimer des lignes, modifier la table et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="9847a-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
