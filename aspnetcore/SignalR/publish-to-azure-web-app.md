---
title: Publier un noyau ASP.NET applications SignalR à l’application Web Azure
author: rachelappel
description: Publier un noyau ASP.NET applications SignalR à l’application Web Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="5616f-103">Publier un noyau ASP.NET applications SignalR à une application Web Azure</span><span class="sxs-lookup"><span data-stu-id="5616f-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="5616f-104">[Application Web Azure](/azure/app-service/app-service-web-overview) est un [informatique en nuage Microsoft](https://azure.microsoft.com/) service de plateforme pour l’hébergement d’applications web, y compris ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5616f-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="5616f-105">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="5616f-105">Publish the app</span></span>

<span data-ttu-id="5616f-106">Visual Studio fournit des outils intégrés pour la publication vers une application Web Azure.</span><span class="sxs-lookup"><span data-stu-id="5616f-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="5616f-107">Visual Studio Code utilisateur peut utiliser [CLI d’Azure](/cli/azure) commandes pour publier des applications pour Azure.</span><span class="sxs-lookup"><span data-stu-id="5616f-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="5616f-108">Cet article traite de publication utilisant les outils dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5616f-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="5616f-109">Pour publier une application à l’aide de CLI d’Azure, consultez [publier une application ASP.NET Core dans Windows Azure avec les outils de ligne de commande](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="5616f-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="5616f-110">Avec le bouton droit sur le projet dans **l’Explorateur de solutions** et sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="5616f-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="5616f-111">Vérifiez que **nouvel** est activée dans le **choisir une cible de publication** boîte de dialogue, puis sélectionnez **publier**.</span><span class="sxs-lookup"><span data-stu-id="5616f-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Choix de publication cible](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="5616f-113">Entrez les informations suivantes dans le **créer un Service application** boîte de dialogue et sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="5616f-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="5616f-114">Élément</span><span class="sxs-lookup"><span data-stu-id="5616f-114">Item</span></span> | <span data-ttu-id="5616f-115">Description</span><span class="sxs-lookup"><span data-stu-id="5616f-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="5616f-116">**Nom de l’application**</span><span class="sxs-lookup"><span data-stu-id="5616f-116">**App name**</span></span> | <span data-ttu-id="5616f-117">Un nom unique de l’application.</span><span class="sxs-lookup"><span data-stu-id="5616f-117">A unique name of the app.</span></span> |
| <span data-ttu-id="5616f-118">**Abonnement**</span><span class="sxs-lookup"><span data-stu-id="5616f-118">**Subscription**</span></span> | <span data-ttu-id="5616f-119">L’abonnement Azure utilisé par l’application.</span><span class="sxs-lookup"><span data-stu-id="5616f-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="5616f-120">**Groupe de ressources**</span><span class="sxs-lookup"><span data-stu-id="5616f-120">**Resource Group**</span></span> | <span data-ttu-id="5616f-121">Le groupe de ressources liées à laquelle appartient l’application.</span><span class="sxs-lookup"><span data-stu-id="5616f-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="5616f-122">**Plan d’hébergement**</span><span class="sxs-lookup"><span data-stu-id="5616f-122">**Hosting Plan**</span></span> | <span data-ttu-id="5616f-123">Le plan de tarification pour l’application web.</span><span class="sxs-lookup"><span data-stu-id="5616f-123">The pricing plan for the web app.</span></span> |

![Créer le service d’applications](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="5616f-125">Visual Studio effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="5616f-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="5616f-126">Crée un profil de publication qui contient les paramètres de publication.</span><span class="sxs-lookup"><span data-stu-id="5616f-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="5616f-127">Crée ou utilise un fichier *application Web Azure* avec les détails fournis.</span><span class="sxs-lookup"><span data-stu-id="5616f-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="5616f-128">Publie l’application.</span><span class="sxs-lookup"><span data-stu-id="5616f-128">Publishes the app.</span></span>
* <span data-ttu-id="5616f-129">Lance un navigateur, avec l’application web publiée chargée.</span><span class="sxs-lookup"><span data-stu-id="5616f-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="5616f-130">Notez le format de l’URL pour l’application est *{nom de l’application} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="5616f-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="5616f-131">Par exemple, une application nommée `SignalRChattR` possède une URL qui ressemble à `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="5616f-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="5616f-132">Si une erreur HTTP 502.2 se produit, consultez [préversion déployer ASP.NET Core pour le Service d’applications Azure](xref:host-and-deploy/azure-apps/index) pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="5616f-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="5616f-133">Configurer l’application web de SignalR</span><span class="sxs-lookup"><span data-stu-id="5616f-133">Configure SignalR web app</span></span>

<span data-ttu-id="5616f-134">Les applications ASP.NET Core SignalR qui sont publiées comme une application Web Azure doit avoir [ARR affinité](https://en.wikipedia.org/wiki/Application_Request_Routing) activé.</span><span class="sxs-lookup"><span data-stu-id="5616f-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="5616f-135">[WebSockets](xref:fundamentals/websockets) doit être activée pour permettre le transport WebSocket (fonction).</span><span class="sxs-lookup"><span data-stu-id="5616f-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="5616f-136">Dans le portail Azure, accédez à **paramètres de l’application** pour votre application web.</span><span class="sxs-lookup"><span data-stu-id="5616f-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="5616f-137">Définissez **WebSockets** à **sur**et vérifiez **ARR affinité** est **sur**.</span><span class="sxs-lookup"><span data-stu-id="5616f-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Paramètres de l’application Web Azure dans le portail Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="5616f-139">WebSockets et autres transports [sont limitées, selon le Plan App Service](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="5616f-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="5616f-140">Ressources connexes</span><span class="sxs-lookup"><span data-stu-id="5616f-140">Related resources</span></span>

* [<span data-ttu-id="5616f-141">Publier une application ASP.NET Core dans Windows Azure avec les outils de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="5616f-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="5616f-142">Publier une application ASP.NET Core dans Azure avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5616f-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="5616f-143">Héberger et déployer des applications ASP.NET Core aperçu sur Azure</span><span class="sxs-lookup"><span data-stu-id="5616f-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
