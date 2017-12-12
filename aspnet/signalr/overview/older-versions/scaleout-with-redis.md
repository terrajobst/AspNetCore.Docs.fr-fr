---
uid: signalr/overview/older-versions/scaleout-with-redis
title: "Montée en charge SignalR avec Redis (SignalR 1.x) | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: c0d6fd421dad02298326d1975ae68d1e7cc78d8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="f6039-102">Montée en charge SignalR avec Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="f6039-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f6039-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f6039-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="f6039-104">Dans ce didacticiel, vous allez utiliser [Redis](http://redis.io/) pour distribuer des messages entre une application SignalR qui est déployée sur deux instances distinctes d’IIS.</span><span class="sxs-lookup"><span data-stu-id="f6039-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="f6039-105">Redis est un magasin clé-valeur de mémoire.</span><span class="sxs-lookup"><span data-stu-id="f6039-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="f6039-106">Il prend également en charge un système de messagerie avec un modèle de publication/abonnement.</span><span class="sxs-lookup"><span data-stu-id="f6039-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="f6039-107">Le fond de panier SignalR Redis utilise la fonctionnalité de pub/sub pour transférer des messages vers d’autres serveurs.</span><span class="sxs-lookup"><span data-stu-id="f6039-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="f6039-108">Pour ce didacticiel, vous allez utiliser trois serveurs :</span><span class="sxs-lookup"><span data-stu-id="f6039-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="f6039-109">Deux serveurs qui exécutent Windows, que vous utiliserez pour déployer une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="f6039-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="f6039-110">Un serveur exécutant Linux, que vous utiliserez pour exécuter Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="f6039-111">Pour les captures d’écran de ce didacticiel, j’ai utilisé Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="f6039-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="f6039-112">Si vous n’avez pas trois serveurs physiques à utiliser, vous pouvez créer des machines virtuelles sur Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f6039-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="f6039-113">Une autre option consiste à créer des machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="f6039-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="f6039-114">Bien que ce didacticiel utilise l’implémentation de Redis officielle, il existe également un [Windows port de Redis](https://github.com/MSOpenTech/redis) de MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="f6039-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="f6039-115">Le programme d’installation et de configuration sont différents, mais sinon les étapes sont les mêmes.</span><span class="sxs-lookup"><span data-stu-id="f6039-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f6039-116">Montée en charge SignalR avec Redis ne prend pas en charge les clusters de Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="f6039-117">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f6039-117">Overview</span></span>

<span data-ttu-id="f6039-118">Avant du didacticiel détaillé, voici une vue d’ensemble rapide des opérations à effectuer.</span><span class="sxs-lookup"><span data-stu-id="f6039-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="f6039-119">Installer Redis et démarrer le serveur Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="f6039-120">Ajoutez ces packages NuGet pour votre application :</span><span class="sxs-lookup"><span data-stu-id="f6039-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="f6039-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="f6039-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="f6039-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="f6039-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="f6039-123">Créez une application SignalR.</span><span class="sxs-lookup"><span data-stu-id="f6039-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="f6039-124">Ajoutez le code suivant à Global.asax pour configurer l’infrastructure d’intégration :</span><span class="sxs-lookup"><span data-stu-id="f6039-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="f6039-125">Ubuntu sur Hyper-V</span><span class="sxs-lookup"><span data-stu-id="f6039-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="f6039-126">À l’aide de Windows Hyper-V, vous pouvez facilement créer une VM Ubuntu sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f6039-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="f6039-127">Télécharger le fichier ISO d’Ubuntu de [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="f6039-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="f6039-128">Dans Hyper-V, ajoutez une nouvelle machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="f6039-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="f6039-129">Dans le **connecter un disque dur virtuel** étape, sélectionnez **créer un disque dur virtuel**.</span><span class="sxs-lookup"><span data-stu-id="f6039-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="f6039-130">Dans le **Options d’Installation** étape, sélectionnez **le fichier Image (.iso)**, cliquez sur **Parcourir**, puis accédez à l’installation d’Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="f6039-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="f6039-131">Installer Redis</span><span class="sxs-lookup"><span data-stu-id="f6039-131">Install Redis</span></span>

<span data-ttu-id="f6039-132">Suivez les étapes à [http://redis.io/download](http://redis.io/download) pour télécharger et générer Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="f6039-133">Cette opération génère les fichiers binaires de Redis le `src` active.</span><span class="sxs-lookup"><span data-stu-id="f6039-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="f6039-134">Par défaut, Redis ne nécessite pas un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="f6039-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="f6039-135">Pour définir un mot de passe, modifier le `redis.conf` fichier, qui se trouve dans le répertoire racine du code source.</span><span class="sxs-lookup"><span data-stu-id="f6039-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="f6039-136">(Vérifiez une copie de sauvegarde du fichier avant de le modifier) ! Ajoutez la directive suivante à `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="f6039-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="f6039-137">Maintenant démarrer le serveur Redis :</span><span class="sxs-lookup"><span data-stu-id="f6039-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="f6039-138">Écoute le port ouvert 6379, qui est le port par défaut Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="f6039-139">(Vous pouvez modifier le numéro de port dans le fichier de configuration).</span><span class="sxs-lookup"><span data-stu-id="f6039-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="f6039-140">Créer l’Application SignalR</span><span class="sxs-lookup"><span data-stu-id="f6039-140">Create the SignalR Application</span></span>

<span data-ttu-id="f6039-141">Créer une application SignalR en suivant une de ces didacticiels :</span><span class="sxs-lookup"><span data-stu-id="f6039-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="f6039-142">Mise en route avec SignalR</span><span class="sxs-lookup"><span data-stu-id="f6039-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="f6039-143">Prise en main SignalR et MVC 4</span><span class="sxs-lookup"><span data-stu-id="f6039-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="f6039-144">Ensuite, nous allons modifier l’application de conversation pour prendre en charge la montée en puissance parallèle avec Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="f6039-145">Tout d’abord, ajoutez le package NuGet de SignalR.Redis à votre projet.</span><span class="sxs-lookup"><span data-stu-id="f6039-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="f6039-146">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package de bibliothèque**, puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f6039-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f6039-147">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f6039-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="f6039-148">Ensuite, ouvrez le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="f6039-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="f6039-149">Ajoutez le code suivant à la **Application\_Démarrer** méthode :</span><span class="sxs-lookup"><span data-stu-id="f6039-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="f6039-150">« serveur » est le nom du serveur qui est en cours d’exécution Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="f6039-151">*port* est le numéro de port</span><span class="sxs-lookup"><span data-stu-id="f6039-151">*port* is the port number</span></span>
- <span data-ttu-id="f6039-152">« password » est le mot de passe que vous avez définie dans le fichier redis.conf.</span><span class="sxs-lookup"><span data-stu-id="f6039-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="f6039-153">« AppName » est une chaîne.</span><span class="sxs-lookup"><span data-stu-id="f6039-153">"AppName" is any string.</span></span> <span data-ttu-id="f6039-154">SignalR crée un canal de pub/sub Redis portant ce nom.</span><span class="sxs-lookup"><span data-stu-id="f6039-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="f6039-155">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f6039-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="f6039-156">Déployer et exécuter l’Application</span><span class="sxs-lookup"><span data-stu-id="f6039-156">Deploy and Run the Application</span></span>

<span data-ttu-id="f6039-157">Préparez vos instances de Windows Server pour déployer l’application SignalR.</span><span class="sxs-lookup"><span data-stu-id="f6039-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="f6039-158">Ajouter le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="f6039-158">Add the IIS role.</span></span> <span data-ttu-id="f6039-159">Inclure les fonctionnalités de « Développement d’applications », y compris le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f6039-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="f6039-160">Incluent également le Service de gestion (répertoriées sous « Outils de gestion »).</span><span class="sxs-lookup"><span data-stu-id="f6039-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="f6039-161">**Installer Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="f6039-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="f6039-162">Lorsque vous exécutez le Gestionnaire des services Internet, il vous invite à installer Microsoft Web Platform, ou vous pouvez [télécharger l’intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="f6039-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="f6039-163">Dans le programme d’installation de la plateforme, Web Deploy rechercher et installer Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="f6039-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="f6039-164">Vérifiez que le Service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="f6039-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="f6039-165">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="f6039-165">If not, start the service.</span></span> <span data-ttu-id="f6039-166">(Si vous ne voyez pas Service de gestion Web dans la liste des services Windows, assurez-vous que vous avez installé le Service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="f6039-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="f6039-167">Par défaut, le Service de gestion Web écoute sur le port TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="f6039-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="f6039-168">Dans le pare-feu Windows, créez une règle de trafic entrant pour autoriser le trafic TCP sur le port 8172.</span><span class="sxs-lookup"><span data-stu-id="f6039-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="f6039-169">Pour plus d’informations, consultez [configuration des règles de pare-feu](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="f6039-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="f6039-170">(Si vous hébergez les machines virtuelles sur Azure, vous pouvez faire directement dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="f6039-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="f6039-171">Consultez [comment faire pour configurer des points de terminaison à une Machine virtuelle](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="f6039-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="f6039-172">Vous êtes maintenant prêt à déployer le projet de Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f6039-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="f6039-173">Dans l’Explorateur de solutions, avec le bouton droit de la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="f6039-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="f6039-174">Pour plus d’informations de déploiement web, consultez [plan de contenu de déploiement Web pour Visual Studio et ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="f6039-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="f6039-175">Si vous déployez l’application à deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent des messages SignalR à partir de l’autre.</span><span class="sxs-lookup"><span data-stu-id="f6039-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="f6039-176">(Bien entendu, dans un environnement de production, les deux serveurs sont placé derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="f6039-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="f6039-177">Si vous n’êtes pour découvrir les messages sont envoyés vers Redis, vous pouvez utiliser la **redis-cli** client, qui est installé avec Redis.</span><span class="sxs-lookup"><span data-stu-id="f6039-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
