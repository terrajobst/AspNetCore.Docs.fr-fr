---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Montée en puissance parallèle de SignalR avec Azure Service Bus | Microsoft Docs
author: MikeWasson
description: Versions des logiciels utilisés dans cette version de Visual Studio 2013, .NET 4.5 SignalR rubrique 2 Previous versions du serveur de cette rubrique pour le SignalR 1.x de cette rubrique...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b87eb9f2df82d92c07ea0c86873849a44660e5c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830936"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="461e3-103">Montée en puissance parallèle de SignalR avec Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="461e3-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="461e3-104">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="461e3-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="461e3-105">Dans ce didacticiel, vous allez déployer une application de SignalR pour un rôle Web Windows Azure à l’aide de l’infrastructure d’intégration Service Bus pour distribuer les messages à chaque instance de rôle.</span><span class="sxs-lookup"><span data-stu-id="461e3-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="461e3-106">(Vous pouvez également utiliser le fond de panier de Service Bus avec [applications dans Azure App Service web](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="461e3-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="461e3-107">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="461e3-107">Prerequisites:</span></span>

- <span data-ttu-id="461e3-108">Un compte Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="461e3-108">A Windows Azure account.</span></span>
- <span data-ttu-id="461e3-109">Le [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="461e3-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="461e3-110">Visual Studio 2012 ou 2013.</span><span class="sxs-lookup"><span data-stu-id="461e3-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="461e3-111">Le fond de panier de bus de service est également compatible avec [Service Bus pour Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span><span class="sxs-lookup"><span data-stu-id="461e3-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="461e3-112">Toutefois, il n’est pas compatible avec la version 1.0 de Service Bus pour Windows Server.</span><span class="sxs-lookup"><span data-stu-id="461e3-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="461e3-113">Tarification</span><span class="sxs-lookup"><span data-stu-id="461e3-113">Pricing</span></span>

<span data-ttu-id="461e3-114">Le fond de panier de Service Bus utilise les rubriques pour envoyer des messages.</span><span class="sxs-lookup"><span data-stu-id="461e3-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="461e3-115">Pour les informations de tarification les plus récentes, consultez [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="461e3-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="461e3-116">Au moment de cet article est rédigé, vous pouvez envoyer 1 000 000 messages par mois pour moins de 1 $.</span><span class="sxs-lookup"><span data-stu-id="461e3-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="461e3-117">Le fond de panier envoie un message de bus de service pour chaque appel d’une méthode de concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="461e3-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="461e3-118">Il existe également des messages de contrôle pour les connexions, déconnexions, joindre ou quitter groupes et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="461e3-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="461e3-119">Dans la plupart des applications, la majorité du trafic message seront des appels de méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="461e3-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="461e3-120">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="461e3-120">Overview</span></span>

<span data-ttu-id="461e3-121">Avant de passer au didacticiel détaillé, Voici un aperçu rapide de la procédure à suivre.</span><span class="sxs-lookup"><span data-stu-id="461e3-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="461e3-122">Utilisez le portail Windows Azure pour créer un nouvel espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="461e3-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="461e3-123">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="461e3-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="461e3-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="461e3-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="461e3-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) ou [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="461e3-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="461e3-126">Créez une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="461e3-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="461e3-127">Ajoutez le code suivant à Startup.cs pour configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="461e3-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="461e3-128">Ce code configure le fond de panier avec les valeurs par défaut [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) et [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="461e3-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="461e3-129">Pour plus d’informations sur la modification de ces valeurs, consultez [les performances de SignalR : métriques de montée en puissance parallèle](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="461e3-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="461e3-130">Pour chaque application, choisissez une autre valeur pour « Nomapp ».</span><span class="sxs-lookup"><span data-stu-id="461e3-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="461e3-131">N’utilisez pas la même valeur dans plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="461e3-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="461e3-132">Créer les Services Azure</span><span class="sxs-lookup"><span data-stu-id="461e3-132">Create the Azure Services</span></span>

<span data-ttu-id="461e3-133">Créer un Service Cloud, comme décrit dans [comment créer et déployer un Service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="461e3-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="461e3-134">Suivez les étapes décrites dans la section « Comment : créer un service cloud à l’aide de la création rapide ».</span><span class="sxs-lookup"><span data-stu-id="461e3-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="461e3-135">Pour ce didacticiel, vous n’avez pas besoin de télécharger un certificat.</span><span class="sxs-lookup"><span data-stu-id="461e3-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="461e3-136">Créez un nouvel espace de noms Service Bus, comme décrit dans [comment faire pour utiliser rubriques/abonnements Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="461e3-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="461e3-137">Suivez les étapes décrites dans la section « Créer un Namespace de Service ».</span><span class="sxs-lookup"><span data-stu-id="461e3-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="461e3-138">Veillez à sélectionner la même région pour le service cloud et l’espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="461e3-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="461e3-139">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="461e3-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="461e3-140">Démarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="461e3-140">Start Visual Studio.</span></span> <span data-ttu-id="461e3-141">À partir de la **fichier** menu, cliquez sur **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="461e3-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="461e3-142">Dans le **nouveau projet** boîte de dialogue, développez **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="461e3-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="461e3-143">Sous **modèles installés**, sélectionnez **Cloud** , puis sélectionnez **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="461e3-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="461e3-144">Conservez la valeur par défaut .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="461e3-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="461e3-145">Nommez l’application « ChatService » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="461e3-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="461e3-146">Dans le **nouveau Windows Azure Cloud Service** boîte de dialogue Sélectionner un rôle Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="461e3-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="461e3-147">Cliquez sur le bouton de flèche droite (**&gt;**) pour ajouter le rôle à votre solution.</span><span class="sxs-lookup"><span data-stu-id="461e3-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="461e3-148">Pointez la souris sur le nouveau rôle, par conséquent, l’icône de crayon visible.</span><span class="sxs-lookup"><span data-stu-id="461e3-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="461e3-149">Cliquez sur cette icône pour renommer le rôle.</span><span class="sxs-lookup"><span data-stu-id="461e3-149">Click this icon to rename the role.</span></span> <span data-ttu-id="461e3-150">Nom du rôle « SignalRChat » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="461e3-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="461e3-151">Dans le **nouveau projet ASP.NET** boîte de dialogue, sélectionnez **MVC**, puis cliquez sur OK.</span><span class="sxs-lookup"><span data-stu-id="461e3-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="461e3-152">L’Assistant de projet crée deux projets :</span><span class="sxs-lookup"><span data-stu-id="461e3-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="461e3-153">« ChatService » : Ce projet est l’application Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="461e3-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="461e3-154">Il définit les rôles Azure et autres options de configuration.</span><span class="sxs-lookup"><span data-stu-id="461e3-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="461e3-155">SignalRChat : Ce projet est votre projet ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="461e3-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="461e3-156">Créer l’Application de conversation de SignalR</span><span class="sxs-lookup"><span data-stu-id="461e3-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="461e3-157">Pour créer l’application de conversation, suivez les étapes dans le didacticiel [bien démarrer avec SignalR et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="461e3-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="461e3-158">Utilisez NuGet pour installer les bibliothèques requises.</span><span class="sxs-lookup"><span data-stu-id="461e3-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="461e3-159">À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="461e3-159">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="461e3-160">Dans le **Console du Gestionnaire de Package** fenêtre, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="461e3-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="461e3-161">Utilisez la `-ProjectName` option pour installer les packages du projet ASP.NET MVC, plutôt que le projet Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="461e3-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="461e3-162">Configurer le fond de panier</span><span class="sxs-lookup"><span data-stu-id="461e3-162">Configure the Backplane</span></span>

<span data-ttu-id="461e3-163">Dans le fichier de votre application Startup.cs, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="461e3-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="461e3-164">Vous devez maintenant obtenir votre chaîne de connexion de service bus.</span><span class="sxs-lookup"><span data-stu-id="461e3-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="461e3-165">Dans le portail Azure, sélectionnez l’espace de noms service bus que vous avez créé et cliquez sur l’icône de clé d’accès.</span><span class="sxs-lookup"><span data-stu-id="461e3-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="461e3-166">Copier la chaîne de connexion dans le Presse-papiers, puis collez-la dans la *connectionString* variable.</span><span class="sxs-lookup"><span data-stu-id="461e3-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="461e3-167">Déployer sur Azure</span><span class="sxs-lookup"><span data-stu-id="461e3-167">Deploy to Azure</span></span>

<span data-ttu-id="461e3-168">Dans l’Explorateur de solutions, développez le **rôles** dossier dans le projet « ChatService ».</span><span class="sxs-lookup"><span data-stu-id="461e3-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="461e3-169">Cliquez sur le rôle SignalRChat et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="461e3-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="461e3-170">Sélectionnez le **Configuration** onglet. Sous **Instances** sélectionnez 2.</span><span class="sxs-lookup"><span data-stu-id="461e3-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="461e3-171">Vous pouvez également définir la taille de machine virtuelle **très petite**.</span><span class="sxs-lookup"><span data-stu-id="461e3-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="461e3-172">Enregistrez les modifications.</span><span class="sxs-lookup"><span data-stu-id="461e3-172">Save the changes.</span></span>

<span data-ttu-id="461e3-173">Dans l’Explorateur de solutions, cliquez sur le projet « ChatService ».</span><span class="sxs-lookup"><span data-stu-id="461e3-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="461e3-174">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="461e3-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="461e3-175">S’il s’agit de votre première publication de temps sur Windows Azure, vous devez télécharger vos informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="461e3-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="461e3-176">Dans le **publier** Assistant, cliquez sur « Se connecter télécharger les informations d’identification ».</span><span class="sxs-lookup"><span data-stu-id="461e3-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="461e3-177">Cela vous invitera à vous connecter au portail Windows Azure et télécharger un fichier de paramètres de publication.</span><span class="sxs-lookup"><span data-stu-id="461e3-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="461e3-178">Cliquez sur **importation** et sélectionnez le fichier de paramètres de publication que vous avez téléchargé.</span><span class="sxs-lookup"><span data-stu-id="461e3-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="461e3-179">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="461e3-179">Click **Next**.</span></span> <span data-ttu-id="461e3-180">Dans le **paramètres de publication** boîte de dialogue, sous **Service Cloud**, sélectionnez le service cloud que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="461e3-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="461e3-181">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="461e3-181">Click **Publish**.</span></span> <span data-ttu-id="461e3-182">Il peut prendre quelques minutes pour déployer l’application et de démarrer les machines virtuelles.</span><span class="sxs-lookup"><span data-stu-id="461e3-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="461e3-183">Maintenant lorsque vous exécutez l’application de conversation, les instances de rôle communiquent via Azure Service Bus, à l’aide d’une rubrique Service Bus.</span><span class="sxs-lookup"><span data-stu-id="461e3-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="461e3-184">Une rubrique est une file d’attente qui permet à plusieurs abonnés.</span><span class="sxs-lookup"><span data-stu-id="461e3-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="461e3-185">Le fond de panier crée automatiquement la rubrique et les abonnements.</span><span class="sxs-lookup"><span data-stu-id="461e3-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="461e3-186">Pour afficher les abonnements et l’activité des messages, ouvrez le portail Azure, sélectionnez l’espace de noms Service Bus et cliquez sur « Rubriques ».</span><span class="sxs-lookup"><span data-stu-id="461e3-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="461e3-187">Il peut prendre quelques minutes avant que l’activité d’un message s’affiche dans le tableau de bord.</span><span class="sxs-lookup"><span data-stu-id="461e3-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="461e3-188">SignalR gère la durée de vie de rubrique.</span><span class="sxs-lookup"><span data-stu-id="461e3-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="461e3-189">Tant que votre application est déployée, n’essayez pas de supprimer les rubriques manuellement ou de modifier les paramètres sur le sujet.</span><span class="sxs-lookup"><span data-stu-id="461e3-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="461e3-190">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="461e3-190">Troubleshooting</span></span>

<span data-ttu-id="461e3-191">**System.InvalidOperationException « La seule IsolationLevel pris en charge est 'IsolationLevel.Serializable' ».**</span><span class="sxs-lookup"><span data-stu-id="461e3-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="461e3-192">Cette erreur peut se produire si le niveau de transaction pour une opération est défini sur une valeur autre que `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="461e3-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="461e3-193">Vérifiez qu’aucune opération n’est effectuée avec les autres niveaux de transaction.</span><span class="sxs-lookup"><span data-stu-id="461e3-193">Verify that no operations are being performed with other transaction levels.</span></span>
