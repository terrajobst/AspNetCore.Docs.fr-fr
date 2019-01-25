---
uid: signalr/overview/performance/scaleout-with-redis
title: Montée en puissance parallèle de SignalR avec Redis | Microsoft Docs
author: bradygaster
description: Versions des logiciels utilisés dans cette rubrique Visual Studio 2013, .NET 4.5 SignalR les versions précédentes de la version 2 de cette rubrique pour plus d’informations sur les versions antérieures de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 22b8379fd2d97aeb85137e1cc128fe5d053f44ed
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837024"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="8b6b5-103">Montée en puissance parallèle de SignalR avec Redis</span><span class="sxs-lookup"><span data-stu-id="8b6b5-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="8b6b5-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8b6b5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8b6b5-105">Versions des logiciels utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="8b6b5-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="8b6b5-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8b6b5-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="8b6b5-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8b6b5-107">.NET 4.5</span></span>
> - <span data-ttu-id="8b6b5-108">SignalR version 2.4</span><span class="sxs-lookup"><span data-stu-id="8b6b5-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8b6b5-109">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="8b6b5-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="8b6b5-110">Pour plus d’informations sur les versions antérieures de SignalR, consultez [les Versions antérieures de SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="8b6b5-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="8b6b5-111">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="8b6b5-111">Questions and comments</span></span>
>
> <span data-ttu-id="8b6b5-112">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8b6b5-113">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8b6b5-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="8b6b5-114">Dans ce didacticiel, vous allez utiliser [Redis](http://redis.io/) pour distribuer les messages sur une application de SignalR est déployée sur deux instances distinctes d’IIS.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="8b6b5-115">Redis est un magasin de clé-valeur en mémoire.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="8b6b5-116">Il prend également en charge un système de messagerie avec un modèle de publication/abonnement.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="8b6b5-117">Le fond de panier SignalR Redis utilise la fonctionnalité de pub/sub pour transférer des messages vers d’autres serveurs.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="8b6b5-118">Pour ce didacticiel, vous allez utiliser les trois serveurs :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="8b6b5-119">Deux serveurs exécutant Windows, que vous utiliserez pour déployer une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="8b6b5-120">Un serveur exécutant Linux, que vous utiliserez pour exécuter Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="8b6b5-121">Pour les captures d’écran dans ce didacticiel, j’ai utilisé Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="8b6b5-122">Si vous n’avez trois serveurs physiques à utiliser, vous pouvez créer des machines virtuelles sur Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="8b6b5-123">Une autre option consiste à créer des machines virtuelles sur Azure.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="8b6b5-124">Bien que ce didacticiel utilise l’implémentation de Redis officielle, il existe également un [Windows port de Redis](https://github.com/MSOpenTech/redis) à partir de MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="8b6b5-125">Le programme d’installation et de configuration sont différentes, mais sinon les étapes sont identiques.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="8b6b5-126">Montée en puissance parallèle de SignalR avec Redis ne prend pas en charge les clusters Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="8b6b5-127">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="8b6b5-127">Overview</span></span>

<span data-ttu-id="8b6b5-128">Avant de passer au didacticiel détaillé, Voici un aperçu rapide de la procédure à suivre.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="8b6b5-129">Installez Redis et démarrer le serveur Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="8b6b5-130">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="8b6b5-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="8b6b5-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="8b6b5-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="8b6b5-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="8b6b5-133">Créez une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="8b6b5-134">Ajoutez le code suivant à Startup.cs pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="8b6b5-135">Ubuntu sur Hyper-V</span><span class="sxs-lookup"><span data-stu-id="8b6b5-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="8b6b5-136">À l’aide de Windows Hyper-V, vous pouvez facilement créer une VM Ubuntu sur Windows Server.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="8b6b5-137">Téléchargez le fichier ISO Ubuntu de [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="8b6b5-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="8b6b5-138">Dans Hyper-V, ajoutez une nouvelle machine virtuelle.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="8b6b5-139">Dans le **connecter un disque dur virtuel** étape, sélectionnez **créer un disque dur virtuel**.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="8b6b5-140">Dans le **Options d’Installation** étape, sélectionnez **fichier Image (.iso)**, cliquez sur **Parcourir**, puis accédez à l’installation d’Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="8b6b5-141">Installez Redis</span><span class="sxs-lookup"><span data-stu-id="8b6b5-141">Install Redis</span></span>

<span data-ttu-id="8b6b5-142">Suivez les étapes de [ http://redis.io/download ](http://redis.io/download) pour télécharger et générer Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="8b6b5-143">Cette opération génère les fichiers binaires de Redis le `src` directory.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="8b6b5-144">Par défaut, Redis ne nécessite pas un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="8b6b5-145">Pour définir un mot de passe, modifier le `redis.conf` fichier, ce qui se trouve dans le répertoire racine du code source.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="8b6b5-146">(Faites une copie de sauvegarde du fichier avant de le modifier !) Ajoutez la directive suivante à `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="8b6b5-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="8b6b5-147">Démarrer maintenant le serveur Redis :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="8b6b5-148">Ouvrir le port 6379, qui est le port par défaut Redis écoute.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="8b6b5-149">(Vous pouvez modifier le numéro de port dans le fichier de configuration.)</span><span class="sxs-lookup"><span data-stu-id="8b6b5-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="8b6b5-150">Créer l’Application de SignalR</span><span class="sxs-lookup"><span data-stu-id="8b6b5-150">Create the SignalR Application</span></span>

<span data-ttu-id="8b6b5-151">Créez une application de SignalR en suivant une de ces didacticiels :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="8b6b5-152">Bien démarrer avec SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="8b6b5-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="8b6b5-153">Bien démarrer avec SignalR 2.0 et MVC 5</span><span class="sxs-lookup"><span data-stu-id="8b6b5-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="8b6b5-154">Ensuite, nous allons modifier l’application de conversation pour prendre en charge la montée en puissance parallèle avec Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="8b6b5-155">Tout d’abord, ajoutez le `Microsoft.AspNet.SignalR.StackExchangeRedis` package NuGet à votre projet.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="8b6b5-156">Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8b6b5-157">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="8b6b5-158">Ensuite, ouvrez le fichier Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="8b6b5-159">Ajoutez le code suivant à la **Configuration** méthode :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="8b6b5-160">« serveur » est le nom du serveur qui est en cours d’exécution Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="8b6b5-161">*port* est le numéro de port</span><span class="sxs-lookup"><span data-stu-id="8b6b5-161">*port* is the port number</span></span>
- <span data-ttu-id="8b6b5-162">« password » est le mot de passe que vous avez défini dans le fichier redis.conf.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="8b6b5-163">« AppName » est n’importe quelle chaîne.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-163">"AppName" is any string.</span></span> <span data-ttu-id="8b6b5-164">SignalR crée un canal de pub/sub Redis portant ce nom.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="8b6b5-165">Exemple :</span><span class="sxs-lookup"><span data-stu-id="8b6b5-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="8b6b5-166">Déployer et exécuter l’Application</span><span class="sxs-lookup"><span data-stu-id="8b6b5-166">Deploy and Run the Application</span></span>

<span data-ttu-id="8b6b5-167">Préparez vos instances de Windows Server pour déployer l’application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="8b6b5-168">Ajouter le rôle IIS.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-168">Add the IIS role.</span></span> <span data-ttu-id="8b6b5-169">Inclut des fonctionnalités de « Développement d’applications », y compris le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="8b6b5-170">Incluent également le Service de gestion (répertorié sous « Outils de gestion »).</span><span class="sxs-lookup"><span data-stu-id="8b6b5-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="8b6b5-171">**Installer Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="8b6b5-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="8b6b5-172">Lorsque vous exécutez le Gestionnaire des services Internet, il vous invite à installer Microsoft Web Platform, ou vous pouvez [télécharger l’intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="8b6b5-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="8b6b5-173">Dans le programme d’installation de la plateforme, recherchez de Web Deploy et installer Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="8b6b5-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="8b6b5-174">Vérifiez que le Service de gestion Web est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="8b6b5-175">Si ce n’est pas le cas, démarrez le service.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-175">If not, start the service.</span></span> <span data-ttu-id="8b6b5-176">(Si vous ne voyez pas Service de gestion Web dans la liste des services de Windows, assurez-vous que vous avez installé le Service de gestion lorsque vous avez ajouté le rôle IIS.)</span><span class="sxs-lookup"><span data-stu-id="8b6b5-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="8b6b5-177">Par défaut, le Service de gestion Web écoute sur le port TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="8b6b5-178">Dans le pare-feu Windows, créez une règle de trafic entrant pour autoriser le trafic TCP sur le port 8172.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="8b6b5-179">Pour plus d’informations, consultez [configuration des règles de pare-feu](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="8b6b5-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="8b6b5-180">(Si vous hébergez les machines virtuelles sur Azure, vous pouvez le faire directement dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="8b6b5-181">Consultez [procédure pour configurer des points de terminaison sur une Machine virtuelle](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="8b6b5-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="8b6b5-182">Vous êtes maintenant prêt à déployer le projet de Visual Studio à partir de votre ordinateur de développement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="8b6b5-183">Dans l’Explorateur de solutions, avec le bouton droit de la solution, puis cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="8b6b5-184">Pour plus de documentation sur le déploiement web, consultez [plan de contenu de déploiement Web pour Visual Studio et ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="8b6b5-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="8b6b5-185">Si vous déployez l’application à deux serveurs, vous pouvez ouvrir chaque instance dans une fenêtre de navigateur distincte et voir qu’ils reçoivent des messages SignalR à partir de l’autre.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="8b6b5-186">(Bien sûr, dans un environnement de production, les deux serveurs passait souvent derrière un équilibreur de charge.)</span><span class="sxs-lookup"><span data-stu-id="8b6b5-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="8b6b5-187">Si vous êtes impatient de voir les messages qui sont envoyés avec Redis, vous pouvez utiliser la **redis-cli** client, qui s’installe avec Redis.</span><span class="sxs-lookup"><span data-stu-id="8b6b5-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
