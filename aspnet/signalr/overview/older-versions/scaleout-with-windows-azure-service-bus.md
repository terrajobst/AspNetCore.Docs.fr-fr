---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Montée en puissance parallèle de SignalR avec Azure Service Bus (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 77186f43b38a8423a1cbd4cf42723c5b9ccdd953
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837908"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="941e0-102">Montée en puissance parallèle de SignalR avec Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="941e0-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="941e0-103">par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="941e0-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="941e0-104">Dans ce didacticiel, vous allez déployer une application de SignalR pour un rôle Web Windows Azure à l’aide de l’infrastructure d’intégration Service Bus pour distribuer les messages à chaque instance de rôle.</span><span class="sxs-lookup"><span data-stu-id="941e0-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="941e0-105">Conditions préalables :</span><span class="sxs-lookup"><span data-stu-id="941e0-105">Prerequisites:</span></span>

- <span data-ttu-id="941e0-106">Un compte Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="941e0-106">A Windows Azure account.</span></span>
- <span data-ttu-id="941e0-107">Le [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="941e0-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="941e0-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="941e0-108">Visual Studio 2012.</span></span>

<span data-ttu-id="941e0-109">Le fond de panier de bus de service est également compatible avec [Service Bus pour Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span><span class="sxs-lookup"><span data-stu-id="941e0-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="941e0-110">Toutefois, il n’est pas compatible avec la version 1.0 de Service Bus pour Windows Server.</span><span class="sxs-lookup"><span data-stu-id="941e0-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="941e0-111">Tarification</span><span class="sxs-lookup"><span data-stu-id="941e0-111">Pricing</span></span>

<span data-ttu-id="941e0-112">Le fond de panier de Service Bus utilise les rubriques pour envoyer des messages.</span><span class="sxs-lookup"><span data-stu-id="941e0-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="941e0-113">Pour les informations de tarification les plus récentes, consultez [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="941e0-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="941e0-114">Au moment de cet article est rédigé, vous pouvez envoyer 1 000 000 messages par mois pour moins de 1 $.</span><span class="sxs-lookup"><span data-stu-id="941e0-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="941e0-115">Le fond de panier envoie un message de bus de service pour chaque appel d’une méthode de concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="941e0-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="941e0-116">Il existe également des messages de contrôle pour les connexions, déconnexions, joindre ou quitter groupes et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="941e0-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="941e0-117">Dans la plupart des applications, la majorité du trafic message seront des appels de méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="941e0-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="941e0-118">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="941e0-118">Overview</span></span>

<span data-ttu-id="941e0-119">Avant de passer au didacticiel détaillé, Voici un aperçu rapide de la procédure à suivre.</span><span class="sxs-lookup"><span data-stu-id="941e0-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="941e0-120">Utilisez le portail Windows Azure pour créer un nouvel espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="941e0-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="941e0-121">Ajoutez ces packages NuGet à votre application :</span><span class="sxs-lookup"><span data-stu-id="941e0-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="941e0-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="941e0-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="941e0-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="941e0-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="941e0-124">Créez une application de SignalR.</span><span class="sxs-lookup"><span data-stu-id="941e0-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="941e0-125">Ajoutez le code suivant à global.asax de façon à configurer le fond de panier :</span><span class="sxs-lookup"><span data-stu-id="941e0-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="941e0-126">Pour chaque application, choisissez une autre valeur pour « Nomapp ».</span><span class="sxs-lookup"><span data-stu-id="941e0-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="941e0-127">N’utilisez pas la même valeur dans plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="941e0-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="941e0-128">Créer les Services Azure</span><span class="sxs-lookup"><span data-stu-id="941e0-128">Create the Azure Services</span></span>

<span data-ttu-id="941e0-129">Créer un Service Cloud, comme décrit dans [comment créer et déployer un Service Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="941e0-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="941e0-130">Suivez les étapes décrites dans la section « Comment : Créer un service cloud à l’aide de la création rapide ».</span><span class="sxs-lookup"><span data-stu-id="941e0-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="941e0-131">Pour ce didacticiel, vous n’avez pas besoin de télécharger un certificat.</span><span class="sxs-lookup"><span data-stu-id="941e0-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="941e0-132">Créez un nouvel espace de noms Service Bus, comme décrit dans [comment faire pour utiliser rubriques/abonnements Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="941e0-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="941e0-133">Suivez les étapes décrites dans la section « Créer un Namespace de Service ».</span><span class="sxs-lookup"><span data-stu-id="941e0-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="941e0-134">Veillez à sélectionner la même région pour le service cloud et l’espace de noms Service Bus.</span><span class="sxs-lookup"><span data-stu-id="941e0-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="941e0-135">Créer le projet Visual Studio</span><span class="sxs-lookup"><span data-stu-id="941e0-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="941e0-136">Démarrez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="941e0-136">Start Visual Studio.</span></span> <span data-ttu-id="941e0-137">À partir de la **fichier** menu, cliquez sur **nouveau projet**.</span><span class="sxs-lookup"><span data-stu-id="941e0-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="941e0-138">Dans le **nouveau projet** boîte de dialogue, développez **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="941e0-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="941e0-139">Sous **modèles installés**, sélectionnez **Cloud** , puis sélectionnez **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="941e0-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="941e0-140">Conservez la valeur par défaut .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="941e0-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="941e0-141">Nommez l’application « ChatService » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="941e0-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="941e0-142">Dans le **nouveau Windows Azure Cloud Service** boîte de dialogue Sélectionner un rôle ASP.NET MVC 4 Web.</span><span class="sxs-lookup"><span data-stu-id="941e0-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="941e0-143">Cliquez sur le bouton de flèche droite (**&gt;**) pour ajouter le rôle à votre solution.</span><span class="sxs-lookup"><span data-stu-id="941e0-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="941e0-144">Pointez la souris sur le nouveau rôle, par conséquent, l’icône de crayon visible.</span><span class="sxs-lookup"><span data-stu-id="941e0-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="941e0-145">Cliquez sur cette icône pour renommer le rôle.</span><span class="sxs-lookup"><span data-stu-id="941e0-145">Click this icon to rename the role.</span></span> <span data-ttu-id="941e0-146">Nom du rôle « SignalRChat » et cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="941e0-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="941e0-147">Dans le **nouveau projet ASP.NET MVC 4** Assistant, sélectionnez **Application Internet**.</span><span class="sxs-lookup"><span data-stu-id="941e0-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="941e0-148">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="941e0-148">Click **OK**.</span></span> <span data-ttu-id="941e0-149">L’Assistant de projet crée deux projets :</span><span class="sxs-lookup"><span data-stu-id="941e0-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="941e0-150">« ChatService » : Ce projet est l’application Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="941e0-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="941e0-151">Il définit les rôles Azure et autres options de configuration.</span><span class="sxs-lookup"><span data-stu-id="941e0-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="941e0-152">SignalRChat : Ce projet est votre projet ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="941e0-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="941e0-153">Créer l’Application de conversation de SignalR</span><span class="sxs-lookup"><span data-stu-id="941e0-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="941e0-154">Pour créer l’application de conversation, suivez les étapes dans le didacticiel [bien démarrer avec SignalR et MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="941e0-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="941e0-155">Utilisez NuGet pour installer les bibliothèques requises.</span><span class="sxs-lookup"><span data-stu-id="941e0-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="941e0-156">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis sélectionnez **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="941e0-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="941e0-157">Dans le **Console du Gestionnaire de Package** fenêtre, entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="941e0-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="941e0-158">Utilisez la `-ProjectName` option pour installer les packages du projet ASP.NET MVC, plutôt que le projet Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="941e0-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="941e0-159">Configurer le fond de panier</span><span class="sxs-lookup"><span data-stu-id="941e0-159">Configure the Backplane</span></span>

<span data-ttu-id="941e0-160">Dans le fichier Global.asax de votre application, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="941e0-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="941e0-161">Vous devez maintenant obtenir votre chaîne de connexion de service bus.</span><span class="sxs-lookup"><span data-stu-id="941e0-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="941e0-162">Dans le portail Azure, sélectionnez l’espace de noms service bus que vous avez créé et cliquez sur l’icône de clé d’accès.</span><span class="sxs-lookup"><span data-stu-id="941e0-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="941e0-163">Copier la chaîne de connexion dans le Presse-papiers, puis collez-la dans la *connectionString* variable.</span><span class="sxs-lookup"><span data-stu-id="941e0-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="941e0-164">Déployer sur Azure</span><span class="sxs-lookup"><span data-stu-id="941e0-164">Deploy to Azure</span></span>

<span data-ttu-id="941e0-165">Dans l’Explorateur de solutions, développez le **rôles** dossier dans le projet « ChatService ».</span><span class="sxs-lookup"><span data-stu-id="941e0-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="941e0-166">Cliquez sur le rôle SignalRChat et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="941e0-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="941e0-167">Sélectionnez le **Configuration** onglet. Sous **Instances** sélectionnez 2.</span><span class="sxs-lookup"><span data-stu-id="941e0-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="941e0-168">Vous pouvez également définir la taille de machine virtuelle **très petite**.</span><span class="sxs-lookup"><span data-stu-id="941e0-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="941e0-169">Enregistrez les modifications.</span><span class="sxs-lookup"><span data-stu-id="941e0-169">Save the changes.</span></span>

<span data-ttu-id="941e0-170">Dans l’Explorateur de solutions, cliquez sur le projet « ChatService ».</span><span class="sxs-lookup"><span data-stu-id="941e0-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="941e0-171">Sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="941e0-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="941e0-172">S’il s’agit de votre première publication de temps sur Windows Azure, vous devez télécharger vos informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="941e0-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="941e0-173">Dans le **publier** Assistant, cliquez sur « Se connecter télécharger les informations d’identification ».</span><span class="sxs-lookup"><span data-stu-id="941e0-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="941e0-174">Cela vous invitera à vous connecter au portail Windows Azure et télécharger un fichier de paramètres de publication.</span><span class="sxs-lookup"><span data-stu-id="941e0-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="941e0-175">Cliquez sur **importation** et sélectionnez le fichier de paramètres de publication que vous avez téléchargé.</span><span class="sxs-lookup"><span data-stu-id="941e0-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="941e0-176">Cliquez sur **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="941e0-176">Click **Next**.</span></span> <span data-ttu-id="941e0-177">Dans le **paramètres de publication** boîte de dialogue, sous **Service Cloud**, sélectionnez le service cloud que vous avez créé précédemment.</span><span class="sxs-lookup"><span data-stu-id="941e0-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="941e0-178">Cliquez sur **Publier**.</span><span class="sxs-lookup"><span data-stu-id="941e0-178">Click **Publish**.</span></span> <span data-ttu-id="941e0-179">Il peut prendre quelques minutes pour déployer l’application et de démarrer les machines virtuelles.</span><span class="sxs-lookup"><span data-stu-id="941e0-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="941e0-180">Maintenant lorsque vous exécutez l’application de conversation, les instances de rôle communiquent via Azure Service Bus, à l’aide d’une rubrique Service Bus.</span><span class="sxs-lookup"><span data-stu-id="941e0-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="941e0-181">Une rubrique est une file d’attente qui permet à plusieurs abonnés.</span><span class="sxs-lookup"><span data-stu-id="941e0-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="941e0-182">Le fond de panier crée automatiquement la rubrique et les abonnements.</span><span class="sxs-lookup"><span data-stu-id="941e0-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="941e0-183">Pour afficher les abonnements et l’activité des messages, ouvrez le portail Azure, sélectionnez l’espace de noms Service Bus et cliquez sur « Rubriques ».</span><span class="sxs-lookup"><span data-stu-id="941e0-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="941e0-184">Il peut prendre quelques minutes avant que l’activité d’un message s’affiche dans le tableau de bord.</span><span class="sxs-lookup"><span data-stu-id="941e0-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="941e0-185">SignalR gère la durée de vie de rubrique.</span><span class="sxs-lookup"><span data-stu-id="941e0-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="941e0-186">Tant que votre application est déployée, n’essayez pas de supprimer les rubriques manuellement ou de modifier les paramètres sur le sujet.</span><span class="sxs-lookup"><span data-stu-id="941e0-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
