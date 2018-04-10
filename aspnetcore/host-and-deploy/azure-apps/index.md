---
title: Héberger ASP.NET Core sur Azure App Service
author: guardrex
description: Découvrez comment héberger des applications ASP.NET Core dans Azure App Service avec des liens vers des ressources utiles.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="64b83-103">Héberger ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="64b83-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="64b83-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64b83-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="64b83-105">Ressources utiles</span><span class="sxs-lookup"><span data-stu-id="64b83-105">Useful resources</span></span>

<span data-ttu-id="64b83-106">La documentation d’[Azure Web Apps](/azure/app-service/) contient la documentation, les didacticiels, les exemples, les guides pratiques et d’autres ressources sur les applications Azure.</span><span class="sxs-lookup"><span data-stu-id="64b83-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="64b83-107">Voici deux didacticiels importants qui abordent l’hébergement d’applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="64b83-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="64b83-108">Démarrage rapide : créer une application web ASP.NET Core dans Azure</span><span class="sxs-lookup"><span data-stu-id="64b83-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="64b83-109">Utilisez Visual Studio pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Windows.</span><span class="sxs-lookup"><span data-stu-id="64b83-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="64b83-110">Démarrage rapide : créer une application web .NET Core dans App Service sur Linux</span><span class="sxs-lookup"><span data-stu-id="64b83-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="64b83-111">Utilisez la ligne de commande pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Linux.</span><span class="sxs-lookup"><span data-stu-id="64b83-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="64b83-112">Les articles suivants sont disponibles dans la documentation d’ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="64b83-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="64b83-113">Publier sur Azure avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="64b83-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="64b83-114">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="64b83-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="64b83-115">Publier sur Azure avec les outils CLI</span><span class="sxs-lookup"><span data-stu-id="64b83-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="64b83-116">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="64b83-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="64b83-117">Déploiement continu sur Azure avec Visual Studio et Git</span><span class="sxs-lookup"><span data-stu-id="64b83-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="64b83-118">Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.</span><span class="sxs-lookup"><span data-stu-id="64b83-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="64b83-119">Déploiement continu sur Azure avec VSTS</span><span class="sxs-lookup"><span data-stu-id="64b83-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="64b83-120">Configurez une build CI pour une application ASP.NET Core, puis créez une version de déploiement continu sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="64b83-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="64b83-121">Bac à sable Azure Web App</span><span class="sxs-lookup"><span data-stu-id="64b83-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="64b83-122">Découvrez les limitations d’exécution du runtime Azure App Service appliquées par la plateforme Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="64b83-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="64b83-123">Configuration d’application</span><span class="sxs-lookup"><span data-stu-id="64b83-123">Application configuration</span></span>

<span data-ttu-id="64b83-124">Avec ASP.NET Core 2.0 et ultérieur, trois packages du [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) fournissent des fonctionnalités de journalisation automatique pour les applications déployées sur Azure App Service :</span><span class="sxs-lookup"><span data-stu-id="64b83-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="64b83-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utilise [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) pour intégrer la fonction d’éclairage ASP.NET Core à Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="64b83-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="64b83-126">Les fonctionnalités de journalisation ajoutées sont fournies par le package `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="64b83-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="64b83-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) exécute [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) pour ajouter des fournisseurs de journalisation de diagnostics Azure App Service dans le package `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="64b83-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="64b83-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fournit des implémentations de journaliseur pour prendre en charge les journaux de diagnostics et les fonctionnalités de streaming de journal Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="64b83-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="64b83-129">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="64b83-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="64b83-130">L’intergiciel (middleware) d’intégration IIS, qui configure l’intergiciel des en-têtes transférés, et le module ASP.NET Core, sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête.</span><span class="sxs-lookup"><span data-stu-id="64b83-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="64b83-131">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="64b83-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="64b83-132">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="64b83-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="64b83-133">Surveillance et journalisation</span><span class="sxs-lookup"><span data-stu-id="64b83-133">Monitoring and logging</span></span>

<span data-ttu-id="64b83-134">Pour des informations de surveillance, de journalisation et de dépannage, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="64b83-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="64b83-135">Guide pratique pour surveiller des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="64b83-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="64b83-136">Découvrez comment consulter les quotas et les métriques des applications et des plans App Service.</span><span class="sxs-lookup"><span data-stu-id="64b83-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="64b83-137">Activer la journalisation des diagnostics pour les applications web dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="64b83-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="64b83-138">Découvrez comment activer et accéder à la journalisation des diagnostics pour les codes d’état HTTP, les requêtes en échec et les activités de serveur web.</span><span class="sxs-lookup"><span data-stu-id="64b83-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="64b83-139">Présentation de la gestion des erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b83-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="64b83-140">Découvrez les approches courantes permettant de gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64b83-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="64b83-141">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="64b83-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="64b83-142">Découvrez comment diagnostiquer les problèmes de déploiements Azure App Service avec les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="64b83-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="64b83-143">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b83-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="64b83-144">Découvrez les erreurs de configuration de déploiement courantes dans les applications hébergées par Azure App Service/IIS, ainsi que des conseils de dépannage.</span><span class="sxs-lookup"><span data-stu-id="64b83-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="64b83-145">Porte-clés de Protection des données et emplacements de déploiement</span><span class="sxs-lookup"><span data-stu-id="64b83-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="64b83-146">Les [clés de Protection des données](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sont persistantes dans le dossier *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="64b83-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="64b83-147">Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application.</span><span class="sxs-lookup"><span data-stu-id="64b83-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="64b83-148">Les clés ne sont pas protégées au repos.</span><span class="sxs-lookup"><span data-stu-id="64b83-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="64b83-149">Ce dossier fournit le porte-clés à toutes les instances d’une application dans un seul emplacement de déploiement.</span><span class="sxs-lookup"><span data-stu-id="64b83-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="64b83-150">Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés.</span><span class="sxs-lookup"><span data-stu-id="64b83-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="64b83-151">Lors d’une permutation entre les emplacements de déploiement, aucun système utilisant la protection des données ne peut déchiffrer les données stockées à l’aide du porte-clés au sein de l’emplacement précédent.</span><span class="sxs-lookup"><span data-stu-id="64b83-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="64b83-152">L’intergiciel (middleware) ASP.NET Cookie utilise la protection des données pour protéger ses cookies.</span><span class="sxs-lookup"><span data-stu-id="64b83-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="64b83-153">Cela entraîne la déconnexion des utilisateurs des applications qui utilisent l’intergiciel ASP.NET Cookie standard.</span><span class="sxs-lookup"><span data-stu-id="64b83-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="64b83-154">Pour une solution de porte-clés indépendante de l’emplacement, utilisez un fournisseur de porte-clés externe, tel que :</span><span class="sxs-lookup"><span data-stu-id="64b83-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="64b83-155">Stockage Blob Azure</span><span class="sxs-lookup"><span data-stu-id="64b83-155">Azure Blob Storage</span></span>
* <span data-ttu-id="64b83-156">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="64b83-156">Azure Key Vault</span></span>
* <span data-ttu-id="64b83-157">Magasin SQL</span><span class="sxs-lookup"><span data-stu-id="64b83-157">SQL store</span></span>
* <span data-ttu-id="64b83-158">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="64b83-158">Redis cache</span></span>

<span data-ttu-id="64b83-159">Pour plus d’informations, consultez [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="64b83-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="64b83-160">Déployer la version préliminaire d’ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="64b83-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="64b83-161">Les applications basées sur la version préliminaire d’ASP.NET Core peuvent être déployées sur Azure App Service avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="64b83-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="64b83-162">Installer l’extension de site préliminaire</span><span class="sxs-lookup"><span data-stu-id="64b83-162">Install the preview site extention</span></span>](#site-x)
* [<span data-ttu-id="64b83-163">Déployer l’application autonome</span><span class="sxs-lookup"><span data-stu-id="64b83-163">Deploy the app self contained</span></span>](#self)
* [<span data-ttu-id="64b83-164">Utiliser Docker avec Web Apps pour conteneurs</span><span class="sxs-lookup"><span data-stu-id="64b83-164">Use Docker with Web Apps for containers</span></span>](#docker)

<span data-ttu-id="64b83-165">Si vous rencontrez des difficultés pour utiliser l’extension de site préliminaire, ouvrez un problème sur [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="64b83-165">If you have a problem using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a><span data-ttu-id="64b83-166">Installer l’extension de site préliminaire</span><span class="sxs-lookup"><span data-stu-id="64b83-166">Install the preview site extention</span></span>

* <span data-ttu-id="64b83-167">Dans le portail Azure, accédez au panneau App Service.</span><span class="sxs-lookup"><span data-stu-id="64b83-167">From the Azure portal, navigate to the App Service blade.</span></span>
* <span data-ttu-id="64b83-168">Entrez « ex » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="64b83-168">Enter "ex" in the search box.</span></span>
* <span data-ttu-id="64b83-169">Sélectionner **Extensions**.</span><span class="sxs-lookup"><span data-stu-id="64b83-169">Select **Extensions**.</span></span>
* <span data-ttu-id="64b83-170">Sélectionner « Ajouter ».</span><span class="sxs-lookup"><span data-stu-id="64b83-170">Select "Add".</span></span>

![Panneau de l’application Azure avec les étapes précédentes](index/_static/x1.png)

* <span data-ttu-id="64b83-172">Sélectionnez **Extensions du runtime ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="64b83-172">Select **ASP.NET Core Runtime Extensions**.</span></span>
* <span data-ttu-id="64b83-173">Sélectionnez **OK** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="64b83-173">Select **OK** > **OK**.</span></span>

<span data-ttu-id="64b83-174">Quand les opérations d’ajout sont terminées, la dernière version préliminaire de .NET Core 2.1 est installée.</span><span class="sxs-lookup"><span data-stu-id="64b83-174">When the add operations completes, the latest .NET Core 2.1 preview is installed.</span></span> <span data-ttu-id="64b83-175">Vous pouvez vérifier l’installation en exécutant `dotnet --info` dans la console.</span><span class="sxs-lookup"><span data-stu-id="64b83-175">You can verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="64b83-176">Dans le panneau App Service :</span><span class="sxs-lookup"><span data-stu-id="64b83-176">From the App Service blade:</span></span>

* <span data-ttu-id="64b83-177">Entrez « con » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="64b83-177">Enter "con" in the search box.</span></span>
* <span data-ttu-id="64b83-178">Sélectionnez **Console**.</span><span class="sxs-lookup"><span data-stu-id="64b83-178">Select **Console**.</span></span>
* <span data-ttu-id="64b83-179">Entrez `dotnet --info` dans la console.</span><span class="sxs-lookup"><span data-stu-id="64b83-179">Enter `dotnet --info` in the console.</span></span>

![Panneau de l’application Azure avec les étapes précédentes](index/_static/cons.png)

<span data-ttu-id="64b83-181">L’image précédente date du moment où ceci a été écrit.</span><span class="sxs-lookup"><span data-stu-id="64b83-181">The preceding image was current at the time this was written.</span></span> <span data-ttu-id="64b83-182">La version que vous voyez peut être différente.</span><span class="sxs-lookup"><span data-stu-id="64b83-182">You may see a different version.</span></span>

<span data-ttu-id="64b83-183">La commande `dotnet --info` affiche le chemin de l’extension de site où la version préliminaire a été installée.</span><span class="sxs-lookup"><span data-stu-id="64b83-183">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="64b83-184">Il indique que l’application s’exécute depuis l’extension de site au lieu de l’emplacement de *ProgramFiles* par défaut.</span><span class="sxs-lookup"><span data-stu-id="64b83-184">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="64b83-185">Si vous voyez *ProgramFiles*, redémarrez le site et exécutez `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="64b83-185">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a><span data-ttu-id="64b83-186">Utiliser l’extension de site en version préliminaire avec un modèle ARM</span><span class="sxs-lookup"><span data-stu-id="64b83-186">Use the preview site extention with an ARM template</span></span>

<span data-ttu-id="64b83-187">Si vous utilisez un modèle ARM pour créer et déployer des applications, vous pouvez utiliser le type de ressource `siteextensions` pour ajouter l’extension de site à une application web.</span><span class="sxs-lookup"><span data-stu-id="64b83-187">If you are using an ARM template to create and deploy applications you can use the `siteextensions` resource type to add the site extension to a Web App.</span></span> <span data-ttu-id="64b83-188">Exemple :</span><span class="sxs-lookup"><span data-stu-id="64b83-188">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="64b83-189">Déployer l’application autonome</span><span class="sxs-lookup"><span data-stu-id="64b83-189">Deploy the app self contained</span></span>

<span data-ttu-id="64b83-190">Vous pouvez déployer une [application autonome](/dotnet/core/deploying/#self-contained-deployments-scd) qui contient le runtime en version préliminaire lors du déploiement.</span><span class="sxs-lookup"><span data-stu-id="64b83-190">You can deploy a [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) that carries the preview runtime with it when being deployed.</span></span> <span data-ttu-id="64b83-191">Lors du déploiement d’une application autonome :</span><span class="sxs-lookup"><span data-stu-id="64b83-191">When deploying a self contained app:</span></span>

* <span data-ttu-id="64b83-192">Vous n’avez pas besoin de préparer votre site.</span><span class="sxs-lookup"><span data-stu-id="64b83-192">You don’t need to prepare your site.</span></span>
* <span data-ttu-id="64b83-193">Nécessite de publier votre application différemment de ce que vous feriez lors du déploiement d’une application une fois le SDK installé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="64b83-193">Requires you to publish your application differently than you would when deploying an app once the SDK is installed on the server.</span></span>

<span data-ttu-id="64b83-194">Les applications autonomes sont une option pour toutes les applications .NET Core.</span><span class="sxs-lookup"><span data-stu-id="64b83-194">Self-contained apps are an option for all .NET Core applications.</span></span>

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="64b83-195">Utiliser Docker avec Web Apps pour conteneurs</span><span class="sxs-lookup"><span data-stu-id="64b83-195">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="64b83-196">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contient les dernières images de Docker 2.1 en version préliminaire.</span><span class="sxs-lookup"><span data-stu-id="64b83-196">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest 2.1 preview Docker images.</span></span> <span data-ttu-id="64b83-197">Vous pouvez les utiliser comme image de base et les déployer sur Web Apps pour conteneurs comme vous le feriez normalement.</span><span class="sxs-lookup"><span data-stu-id="64b83-197">You can use them as your base image and deploy to Web Apps for Containers as you normally would.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64b83-198">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="64b83-198">Additional resources</span></span>

* [<span data-ttu-id="64b83-199">Vue d’ensemble de Web Apps (vidéo de 5 minutes)</span><span class="sxs-lookup"><span data-stu-id="64b83-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="64b83-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span><span class="sxs-lookup"><span data-stu-id="64b83-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="64b83-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span><span class="sxs-lookup"><span data-stu-id="64b83-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="64b83-202">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="64b83-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="64b83-203">Azure App Service sur Windows Server utilise [IIS (Internet Information Services)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="64b83-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="64b83-204">Les rubriques suivantes se rapportent à la technologie IIS sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="64b83-204">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="64b83-205">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="64b83-205">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="64b83-206">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b83-206">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="64b83-207">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b83-207">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="64b83-208">Modules IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64b83-208">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="64b83-209">Bibliothèque Microsoft TechNet : Windows Server</span><span class="sxs-lookup"><span data-stu-id="64b83-209">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
