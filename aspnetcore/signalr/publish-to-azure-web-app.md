---
title: Publier une ASP.NET Core application SignalR dans Azure App Service
author: bradygaster
description: Découvrez comment publier une application ASP.NET Core SignalR sur Azure App Service.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406113"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a><span data-ttu-id="9d01a-103">Publier une ASP.NET Core application SignalR dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9d01a-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="9d01a-104">Par [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="9d01a-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="9d01a-105">[Azure App Service](/azure/app-service/app-service-web-overview) est un [informatique en nuage Microsoft](https://azure.microsoft.com/) service de plateforme pour l’hébergement d’applications web, y compris ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9d01a-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="9d01a-106">Cet article fait référence à la publication d’une application ASP.NET Core SignalR à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d01a-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="9d01a-107">Pour plus d’informations, consultez [service SignalR pour Azure](https://azure.microsoft.com/services/signalr-service).</span><span class="sxs-lookup"><span data-stu-id="9d01a-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="9d01a-108">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="9d01a-108">Publish the app</span></span>

<span data-ttu-id="9d01a-109">Cet article couvre la publication avec les outils dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d01a-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="9d01a-110">Visual Studio Code les utilisateurs peuvent utiliser [Azure CLI](/cli/azure) commandes pour publier des applications sur Azure.</span><span class="sxs-lookup"><span data-stu-id="9d01a-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="9d01a-111">Pour plus d’informations, consultez [publier une application ASP.NET Core sur Azure avec les outils de ligne de commande](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="9d01a-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="9d01a-112">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="9d01a-113">Vérifiez que **App Service** et **créer** sont sélectionnés dans le **choisir une cible de publication** boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="9d01a-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="9d01a-114">Sélectionnez **créer un profil** à partir de la **publier** bouton Déplacer vers le bas.</span><span class="sxs-lookup"><span data-stu-id="9d01a-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="9d01a-115">Entrez les informations décrites dans le tableau suivant dans le **créer App Service** boîte de dialogue et sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="9d01a-116">Élément</span><span class="sxs-lookup"><span data-stu-id="9d01a-116">Item</span></span>               | <span data-ttu-id="9d01a-117">Description</span><span class="sxs-lookup"><span data-stu-id="9d01a-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="9d01a-118">**Name**</span><span class="sxs-lookup"><span data-stu-id="9d01a-118">**Name**</span></span>           | <span data-ttu-id="9d01a-119">Nom unique de l’application.</span><span class="sxs-lookup"><span data-stu-id="9d01a-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="9d01a-120">**Abonnement**</span><span class="sxs-lookup"><span data-stu-id="9d01a-120">**Subscription**</span></span>   | <span data-ttu-id="9d01a-121">Abonnement Azure qui utilise l’application.</span><span class="sxs-lookup"><span data-stu-id="9d01a-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="9d01a-122">**Groupe de ressources**</span><span class="sxs-lookup"><span data-stu-id="9d01a-122">**Resource Group**</span></span> | <span data-ttu-id="9d01a-123">Groupe de ressources liées à laquelle appartient l’application.</span><span class="sxs-lookup"><span data-stu-id="9d01a-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="9d01a-124">**Plan d’hébergement**</span><span class="sxs-lookup"><span data-stu-id="9d01a-124">**Hosting Plan**</span></span>   | <span data-ttu-id="9d01a-125">Plan de tarification pour l’application web.</span><span class="sxs-lookup"><span data-stu-id="9d01a-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="9d01a-126">Sélectionnez le **Service Azure SignalR** dans le **dépendances** > **ajouter** liste déroulante :</span><span class="sxs-lookup"><span data-stu-id="9d01a-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![Zone de dépendances montrant la sélection du Service Azure SignalR dans la liste déroulante Ajouter](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="9d01a-128">Dans le **Service Azure SignalR** boîte de dialogue, sélectionnez **créer une nouvelle instance de Service Azure SignalR**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="9d01a-129">Fournir un **nom**, **groupe de ressources**, et **emplacement**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="9d01a-130">Retour à la **Service Azure SignalR** boîte de dialogue et sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="9d01a-131">Visual Studio effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d01a-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="9d01a-132">Crée un profil de publication contenant les paramètres de publication.</span><span class="sxs-lookup"><span data-stu-id="9d01a-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="9d01a-133">Crée un *Azure Web App* avec les détails fournis.</span><span class="sxs-lookup"><span data-stu-id="9d01a-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="9d01a-134">Publie l’application.</span><span class="sxs-lookup"><span data-stu-id="9d01a-134">Publishes the app.</span></span>
* <span data-ttu-id="9d01a-135">Lance un navigateur, ce qui charge l’application web.</span><span class="sxs-lookup"><span data-stu-id="9d01a-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="9d01a-136">Le format d’URL de l’application est `{APP SERVICE NAME}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="9d01a-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="9d01a-137">Par exemple, une application nommée `SignalRChatApp` possède une URL de `https://signalrchatapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="9d01a-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="9d01a-138">Si HTTP *502.2 - passerelle incorrecte* erreur se produit lors du déploiement d’une application qui cible une version préliminaire de version de .NET Core, consultez [préversion déployer ASP.NET Core sur Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="9d01a-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="9d01a-139">Configurer l’application dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9d01a-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="9d01a-140">*Cette section s’applique uniquement aux applications n’utilisent ne pas le Service Azure SignalR.*</span><span class="sxs-lookup"><span data-stu-id="9d01a-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="9d01a-141">Si l’application utilise le Service Azure SignalR, le Service d’application ne nécessite pas la configuration de l’affinité de l’Application Request Routing (ARR) et Web Sockets décrites dans cette section.</span><span class="sxs-lookup"><span data-stu-id="9d01a-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="9d01a-142">Les clients connectent à leurs Sockets Web au Service Azure SignalR, pas directement à l’application.</span><span class="sxs-lookup"><span data-stu-id="9d01a-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="9d01a-143">Pour les applications hébergées sans le Service Azure SignalR, activer :</span><span class="sxs-lookup"><span data-stu-id="9d01a-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="9d01a-144">[Affinité ARR](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) pour acheminer les demandes à partir d’un utilisateur à la même instance d’App Service.</span><span class="sxs-lookup"><span data-stu-id="9d01a-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="9d01a-145">Le paramètre par défaut est **sur**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-145">The default setting is **On**.</span></span>
* <span data-ttu-id="9d01a-146">[Web Sockets](xref:fundamentals/websockets) pour permettre le transport WebSocket de la fonction.</span><span class="sxs-lookup"><span data-stu-id="9d01a-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="9d01a-147">Le paramètre par défaut est **hors**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="9d01a-148">Dans le portail Azure, accédez à l’application web dans **App Services**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="9d01a-149">Ouvrez **Configuration** > **paramètres généraux**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="9d01a-150">Définissez **Web sockets** à **sur**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="9d01a-151">Vérifiez que **affinité ARR** a la valeur **sur**.</span><span class="sxs-lookup"><span data-stu-id="9d01a-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="9d01a-152">Plan App Service limite</span><span class="sxs-lookup"><span data-stu-id="9d01a-152">App Service Plan limits</span></span>

<span data-ttu-id="9d01a-153">Web Sockets et les autres transports sont limitées selon le Plan App Service sélectionné.</span><span class="sxs-lookup"><span data-stu-id="9d01a-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="9d01a-154">Pour plus d’informations, consultez le *Azure Cloud Services limite* et *limites App Service* sections de la [abonnement Azure et limites de service, quotas et contraintes](/azure/azure-subscription-service-limits#app-service-limits) article.</span><span class="sxs-lookup"><span data-stu-id="9d01a-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d01a-155">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9d01a-155">Additional resources</span></span>

* [<span data-ttu-id="9d01a-156">Qu’est le Service Azure SignalR ?</span><span class="sxs-lookup"><span data-stu-id="9d01a-156">What is Azure SignalR Service?</span></span>](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="9d01a-157">Publier une application ASP.NET Core sur Azure avec les outils de ligne de commande</span><span class="sxs-lookup"><span data-stu-id="9d01a-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="9d01a-158">Héberger et déployer des applications ASP.NET Core Preview sur Azure</span><span class="sxs-lookup"><span data-stu-id="9d01a-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
