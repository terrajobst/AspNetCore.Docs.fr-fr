---
title: "Héberger ASP.NET Core sur Azure App Service"
author: guardrex
description: "Découvrez comment héberger des applications ASP.NET Core dans Azure App Service avec des liens vers des ressources utiles."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: cefbc27c8091a2ed1441663e3779d67aae2c64dd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="5c46f-103">Héberger ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5c46f-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="5c46f-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c46f-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

## <a name="useful-resources"></a><span data-ttu-id="5c46f-105">Ressources utiles</span><span class="sxs-lookup"><span data-stu-id="5c46f-105">Useful resources</span></span>

<span data-ttu-id="5c46f-106">La documentation d’[Azure Web Apps](/azure/app-service/) contient la documentation, les didacticiels, les exemples, les guides pratiques et d’autres ressources sur les applications Azure.</span><span class="sxs-lookup"><span data-stu-id="5c46f-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="5c46f-107">Voici deux didacticiels importants qui abordent l’hébergement d’applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="5c46f-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="5c46f-108">Démarrage rapide : créer une application web ASP.NET Core dans Azure</span><span class="sxs-lookup"><span data-stu-id="5c46f-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="5c46f-109">Utilisez Visual Studio pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Windows.</span><span class="sxs-lookup"><span data-stu-id="5c46f-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="5c46f-110">Démarrage rapide : créer une application web .NET Core dans App Service sur Linux</span><span class="sxs-lookup"><span data-stu-id="5c46f-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="5c46f-111">Utilisez la ligne de commande pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Linux.</span><span class="sxs-lookup"><span data-stu-id="5c46f-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="5c46f-112">Les articles suivants sont disponibles dans la documentation d’ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="5c46f-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="5c46f-113">Publier sur Azure avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c46f-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="5c46f-114">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c46f-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="5c46f-115">Publier sur Azure avec les outils CLI</span><span class="sxs-lookup"><span data-stu-id="5c46f-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="5c46f-116">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="5c46f-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="5c46f-117">Déploiement continu sur Azure avec Visual Studio et Git</span><span class="sxs-lookup"><span data-stu-id="5c46f-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="5c46f-118">Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.</span><span class="sxs-lookup"><span data-stu-id="5c46f-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="5c46f-119">Déploiement continu sur Azure avec VSTS</span><span class="sxs-lookup"><span data-stu-id="5c46f-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="5c46f-120">Configurez une build CI pour une application ASP.NET Core, puis créez une version de déploiement continu sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5c46f-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="5c46f-121">Bac à sable Azure Web App</span><span class="sxs-lookup"><span data-stu-id="5c46f-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="5c46f-122">Découvrez les limitations d’exécution du runtime Azure App Service appliquées par la plateforme Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="5c46f-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="5c46f-123">Configuration d’application</span><span class="sxs-lookup"><span data-stu-id="5c46f-123">Application configuration</span></span>

<span data-ttu-id="5c46f-124">Avec ASP.NET Core 2.0 et ultérieur, trois packages du [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) fournissent des fonctionnalités de journalisation automatique pour les applications déployées sur Azure App Service :</span><span class="sxs-lookup"><span data-stu-id="5c46f-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="5c46f-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utilise [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) pour intégrer la fonction d’éclairage ASP.NET Core à Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5c46f-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="5c46f-126">Les fonctionnalités de journalisation ajoutées sont fournies par le package `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="5c46f-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="5c46f-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) exécute [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) pour ajouter des fournisseurs de journalisation de diagnostics Azure App Service dans le package `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="5c46f-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="5c46f-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fournit des implémentations de journaliseur pour prendre en charge les journaux de diagnostics et les fonctionnalités de streaming de journal Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5c46f-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="5c46f-129">Surveillance et journalisation</span><span class="sxs-lookup"><span data-stu-id="5c46f-129">Monitoring and logging</span></span>

<span data-ttu-id="5c46f-130">Pour des informations de surveillance, de journalisation et de dépannage, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="5c46f-130">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="5c46f-131">Guide pratique pour surveiller des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5c46f-131">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="5c46f-132">Découvrez comment consulter les quotas et les métriques des applications et des plans App Service.</span><span class="sxs-lookup"><span data-stu-id="5c46f-132">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="5c46f-133">Activer la journalisation des diagnostics pour les applications web dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5c46f-133">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="5c46f-134">Découvrez comment activer et accéder à la journalisation des diagnostics pour les codes d’état HTTP, les requêtes en échec et les activités de serveur web.</span><span class="sxs-lookup"><span data-stu-id="5c46f-134">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="5c46f-135">Présentation de la gestion des erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c46f-135">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="5c46f-136">Découvrez les approches courantes permettant de gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c46f-136">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="5c46f-137">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5c46f-137">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="5c46f-138">Découvrez comment diagnostiquer les problèmes de déploiements Azure App Service avec les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5c46f-138">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="5c46f-139">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c46f-139">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="5c46f-140">Découvrez les erreurs de configuration de déploiement courantes dans les applications hébergées par Azure App Service/IIS, ainsi que des conseils de dépannage.</span><span class="sxs-lookup"><span data-stu-id="5c46f-140">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="5c46f-141">Porte-clés de Protection des données et emplacements de déploiement</span><span class="sxs-lookup"><span data-stu-id="5c46f-141">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="5c46f-142">Les [clés de Protection des données](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sont persistantes dans le dossier *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="5c46f-142">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="5c46f-143">Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application.</span><span class="sxs-lookup"><span data-stu-id="5c46f-143">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="5c46f-144">Les clés ne sont pas protégées au repos.</span><span class="sxs-lookup"><span data-stu-id="5c46f-144">Keys aren't protected at rest.</span></span> <span data-ttu-id="5c46f-145">Ce dossier fournit le porte-clés à toutes les instances d’une application dans un seul emplacement de déploiement.</span><span class="sxs-lookup"><span data-stu-id="5c46f-145">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="5c46f-146">Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés.</span><span class="sxs-lookup"><span data-stu-id="5c46f-146">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="5c46f-147">Lors d’une permutation entre les emplacements de déploiement, aucun système utilisant la protection des données ne peut déchiffrer les données stockées à l’aide du porte-clés au sein de l’emplacement précédent.</span><span class="sxs-lookup"><span data-stu-id="5c46f-147">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="5c46f-148">L’intergiciel (middleware) ASP.NET Cookie utilise la protection des données pour protéger ses cookies.</span><span class="sxs-lookup"><span data-stu-id="5c46f-148">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="5c46f-149">Cela entraîne la déconnexion des utilisateurs des applications qui utilisent l’intergiciel ASP.NET Cookie standard.</span><span class="sxs-lookup"><span data-stu-id="5c46f-149">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="5c46f-150">Pour une solution de porte-clés indépendante de l’emplacement, utilisez un fournisseur de porte-clés externe, tel que :</span><span class="sxs-lookup"><span data-stu-id="5c46f-150">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="5c46f-151">Stockage Blob Azure</span><span class="sxs-lookup"><span data-stu-id="5c46f-151">Azure Blob Storage</span></span>
* <span data-ttu-id="5c46f-152">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="5c46f-152">Azure Key Vault</span></span>
* <span data-ttu-id="5c46f-153">Magasin SQL</span><span class="sxs-lookup"><span data-stu-id="5c46f-153">SQL store</span></span>
* <span data-ttu-id="5c46f-154">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="5c46f-154">Redis cache</span></span>

<span data-ttu-id="5c46f-155">Pour plus d’informations, consultez [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="5c46f-155">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c46f-156">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5c46f-156">Additional resources</span></span>

* [<span data-ttu-id="5c46f-157">Vue d’ensemble de Web Apps (vidéo de 5 minutes)</span><span class="sxs-lookup"><span data-stu-id="5c46f-157">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="5c46f-158">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span><span class="sxs-lookup"><span data-stu-id="5c46f-158">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="5c46f-159">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span><span class="sxs-lookup"><span data-stu-id="5c46f-159">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="5c46f-160">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5c46f-160">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="5c46f-161">Azure App Service sur Windows Server utilise [IIS (Internet Information Services)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="5c46f-161">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="5c46f-162">Les rubriques suivantes se rapportent à la technologie IIS sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="5c46f-162">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="5c46f-163">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="5c46f-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="5c46f-164">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c46f-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="5c46f-165">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c46f-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="5c46f-166">Utilisation de modules IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c46f-166">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="5c46f-167">Bibliothèque Microsoft TechNet : Windows Server</span><span class="sxs-lookup"><span data-stu-id="5c46f-167">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
