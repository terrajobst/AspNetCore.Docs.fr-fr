---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: À l’aide de SignalR avec Web Apps dans Azure App Service | Microsoft Docs
author: pfletcher
description: Ce document décrit comment configurer une application de SignalR qui s’exécute sur Microsoft Azure. Versions des logiciels utilisaient dans le didacticiel Visual Studio 2013 ou vis...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: dabf0f6cfed401e10d2c1134c260022d94c3ab92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389571"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="cad21-104">À l’aide de SignalR avec Web Apps dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cad21-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="cad21-105">par [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="cad21-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="cad21-106">Ce document décrit comment configurer une application de SignalR qui s’exécute sur Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="cad21-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cad21-107">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="cad21-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="cad21-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) ou Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="cad21-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="cad21-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="cad21-109">.NET 4.5</span></span>
> - <span data-ttu-id="cad21-110">SignalR version 2</span><span class="sxs-lookup"><span data-stu-id="cad21-110">SignalR version 2</span></span>
> - <span data-ttu-id="cad21-111">Azure SDK 2.3 pour Visual Studio 2013 ou 2012</span><span class="sxs-lookup"><span data-stu-id="cad21-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="cad21-112">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="cad21-112">Questions and comments</span></span>
> 
> <span data-ttu-id="cad21-113">Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="cad21-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="cad21-114">Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), ou le [forums Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="cad21-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="cad21-115">Sommaire</span><span class="sxs-lookup"><span data-stu-id="cad21-115">Table of Contents</span></span>

- [<span data-ttu-id="cad21-116">Introduction</span><span class="sxs-lookup"><span data-stu-id="cad21-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="cad21-117">Déploiement d’une application Web de SignalR sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cad21-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="cad21-118">L’activation du protocole WebSocket sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cad21-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="cad21-119">À l’aide de l’infrastructure d’intégration du Cache Redis Azure</span><span class="sxs-lookup"><span data-stu-id="cad21-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="cad21-120">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="cad21-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="cad21-121">Introduction</span><span class="sxs-lookup"><span data-stu-id="cad21-121">Introduction</span></span>

<span data-ttu-id="cad21-122">ASP.NET SignalR peut servir à récupérer un nouveau niveau d’interactivité entre serveurs et web ou les clients .NET.</span><span class="sxs-lookup"><span data-stu-id="cad21-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="cad21-123">Quand ils sont hébergés dans Azure, des applications SignalR peuvent tirer parti de haut niveau de disponibilité, évolutives et performantes environnement en cours d’exécution dans le cloud fournit.</span><span class="sxs-lookup"><span data-stu-id="cad21-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="cad21-124">Déploiement d’une application Web de SignalR sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cad21-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="cad21-125">SignalR n’ajoute pas les complications particuliers pour déployer une application dans Azure et un déploiement à un serveur local.</span><span class="sxs-lookup"><span data-stu-id="cad21-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="cad21-126">Une application qui utilise SignalR peut être hébergée dans Azure sans modification de configuration ou d’autres paramètres (Cependant, pour la prise en charge du protocole WebSocket, consultez [l’activation du protocole WebSocket sur Azure App Service](#websocket) ci-dessous.) Pour ce didacticiel, vous allez déployer l’application créée dans le [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) vers Azure.</span><span class="sxs-lookup"><span data-stu-id="cad21-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="cad21-127">**Composants requis**</span><span class="sxs-lookup"><span data-stu-id="cad21-127">**Prerequisites**</span></span>

- <span data-ttu-id="cad21-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="cad21-128">Visual Studio 2013.</span></span> <span data-ttu-id="cad21-129">Si vous n’avez pas Visual Studio, Visual Studio 2013 Express pour le Web est inclus dans l’installation du SDK Azure.</span><span class="sxs-lookup"><span data-stu-id="cad21-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="cad21-130">[Azure SDK 2.3 pour Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou [Azure SDK 2.3 pour Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="cad21-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="cad21-131">Pour suivre ce didacticiel, vous aurez besoin un abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="cad21-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="cad21-132">Vous pouvez [activer vos avantages d’abonné MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), ou [souscrire un abonnement d’évaluation](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cad21-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="cad21-133">Déploiement d’une application web de SignalR vers Azure</span><span class="sxs-lookup"><span data-stu-id="cad21-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="cad21-134">Terminer la [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), ou téléchargez le projet terminé sur [galerie de Code](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="cad21-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="cad21-135">Dans Visual Studio, sélectionnez **Build**, **publier le SignalR Chat**.</span><span class="sxs-lookup"><span data-stu-id="cad21-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="cad21-136">Dans la boîte de dialogue « Publier le site Web », sélectionnez « Sites Web Windows Azure ».</span><span class="sxs-lookup"><span data-stu-id="cad21-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Sélectionnez les Sites Web Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="cad21-138">Si vous n’êtes pas connecté à votre compte Microsoft, cliquez sur **se connecter...**  dans la boîte de dialogue « Sélectionner un Site Web existant », puis connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="cad21-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Sélectionnez le Site Web existant](using-signalr-with-azure-web-sites/_static/image2.png)    ![Se connecter à Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="cad21-141">Dans la boîte de dialogue « Sélectionner un Site Web existant », cliquez sur **New**.</span><span class="sxs-lookup"><span data-stu-id="cad21-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Nouveau site Web](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="cad21-143">Dans la boîte de dialogue « Créer un site sur Windows Azure », entrez un nom d’application unique.</span><span class="sxs-lookup"><span data-stu-id="cad21-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="cad21-144">Dans la liste déroulante région, sélectionnez la région la plus proche pour vous.</span><span class="sxs-lookup"><span data-stu-id="cad21-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="cad21-145">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="cad21-145">Click **Create**.</span></span>

    ![Créer un site sur Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="cad21-147">Dans la boîte de dialogue « Publier le site Web », cliquez sur **publier**.</span><span class="sxs-lookup"><span data-stu-id="cad21-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publier le site](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="cad21-149">Lorsque l’application a terminé la publication, l’application de conversation de SignalR hébergée dans Azure App Service Web Apps s’ouvre dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="cad21-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Site dans un navigateur](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="cad21-151">L’activation du protocole WebSocket sur Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="cad21-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="cad21-152">WebSocket doit être explicitement activée dans votre application web à utiliser dans une application SignalR ; Sinon, les autres protocoles seront utilisés (voir [Transports et les solutions de secours](../getting-started/introduction-to-signalr.md#transports) pour plus d’informations).</span><span class="sxs-lookup"><span data-stu-id="cad21-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="cad21-153">Pour pouvoir utiliser les WebSockets sur Azure App Service Web Apps, vous devez l’activer dans la section de configuration de l’application web.</span><span class="sxs-lookup"><span data-stu-id="cad21-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="cad21-154">Pour ce faire, ouvrez votre application web dans le [portail de gestion](https://manage.windowsazure.com/)et sélectionnez Configurer.</span><span class="sxs-lookup"><span data-stu-id="cad21-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Onglet Configurer](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="cad21-156">En haut de la page de configuration, assurez-vous que le .NET 4.5 est utilisée pour votre application web.</span><span class="sxs-lookup"><span data-stu-id="cad21-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Paramètre de .NET framework version 4.5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="cad21-158">Dans la page de configuration, dans le **WebSockets** , sélectionnez **sur**.</span><span class="sxs-lookup"><span data-stu-id="cad21-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Paramètre de protocole WebSocket : sur](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="cad21-160">Au bas de la page de Configuration, sélectionnez **enregistrer** pour enregistrer vos modifications.</span><span class="sxs-lookup"><span data-stu-id="cad21-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Enregistrer les paramètres](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="cad21-162">À l’aide de l’infrastructure d’intégration du Cache Redis Azure</span><span class="sxs-lookup"><span data-stu-id="cad21-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="cad21-163">Si vous utilisez plusieurs instances de votre application web, et les utilisateurs de ces instances doivent interagir avec eux (afin que, par exemple, les messages de conversation créées dans une seule instance peuvent atteindre les utilisateurs connectés à d’autres instances), le [du Cache Redis Azure fond de panier](../performance/scaleout-with-redis.md) doit être implémentée dans votre application.</span><span class="sxs-lookup"><span data-stu-id="cad21-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="cad21-164">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="cad21-164">Next Steps</span></span>

<span data-ttu-id="cad21-165">Pour plus d’informations sur les applications Web dans Azure App Service, consultez [vue d’ensemble des applications Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="cad21-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
