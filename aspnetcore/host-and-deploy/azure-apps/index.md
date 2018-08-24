---
title: Héberger ASP.NET Core sur Azure App Service
author: guardrex
description: Découvrez comment héberger des applications ASP.NET Core dans Azure App Service avec des liens vers des ressources utiles.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 9a7d20378cac597b748d8a60eb0f0bf17c9ba082
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2018
ms.locfileid: "41746417"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="2c4fb-103">Héberger ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c4fb-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="2c4fb-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="2c4fb-105">Ressources utiles</span><span class="sxs-lookup"><span data-stu-id="2c4fb-105">Useful resources</span></span>

<span data-ttu-id="2c4fb-106">La documentation d’[Azure Web Apps](/azure/app-service/) contient la documentation, les didacticiels, les exemples, les guides pratiques et d’autres ressources sur les applications Azure.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="2c4fb-107">Voici deux didacticiels importants qui abordent l’hébergement d’applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="2c4fb-108">Démarrage rapide : créer une application web ASP.NET Core dans Azure</span><span class="sxs-lookup"><span data-stu-id="2c4fb-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="2c4fb-109">Utilisez Visual Studio pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Windows.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="2c4fb-110">Démarrage rapide : créer une application web .NET Core dans App Service sur Linux</span><span class="sxs-lookup"><span data-stu-id="2c4fb-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="2c4fb-111">Utilisez la ligne de commande pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Linux.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="2c4fb-112">Les articles suivants sont disponibles dans la documentation d’ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="2c4fb-113">Publier sur Azure avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c4fb-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="2c4fb-114">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="2c4fb-115">Publier sur Azure avec les outils CLI</span><span class="sxs-lookup"><span data-stu-id="2c4fb-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="2c4fb-116">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="2c4fb-117">Déploiement continu sur Azure avec Visual Studio et Git</span><span class="sxs-lookup"><span data-stu-id="2c4fb-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="2c4fb-118">Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="2c4fb-119">Déploiement continu sur Azure avec VSTS</span><span class="sxs-lookup"><span data-stu-id="2c4fb-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="2c4fb-120">Configurez une build CI pour une application ASP.NET Core, puis créez une version de déploiement continu sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="2c4fb-121">Bac à sable Azure Web App</span><span class="sxs-lookup"><span data-stu-id="2c4fb-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="2c4fb-122">Découvrez les limitations d’exécution du runtime Azure App Service appliquées par la plateforme Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="2c4fb-123">Configuration d’application</span><span class="sxs-lookup"><span data-stu-id="2c4fb-123">Application configuration</span></span>

<span data-ttu-id="2c4fb-124">Dans ASP.NET Core 2.0 ou version ultérieure, les packages NuGet suivants fournissent des fonctionnalités de journalisation automatique pour les applications déployées sur Azure App Service :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="2c4fb-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utilise [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) pour intégrer la fonction d’éclairage ASP.NET Core dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="2c4fb-126">Les fonctionnalités de journalisation ajoutées sont fournies par le package `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="2c4fb-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) exécute [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) pour ajouter des fournisseurs de journalisation de diagnostics Azure App Service dans le package `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="2c4fb-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fournit des implémentations de journaliseur pour prendre en charge les journaux de diagnostics et les fonctionnalités de streaming de journal Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="2c4fb-129">Si vous ciblez .NET Core et référencez le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage), les packages sont toujours inclus.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="2c4fb-130">Les packages sont absents du [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) plus récent.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="2c4fb-131">Si vous ciblez le .NET Framework ou référencez le métapackage `Microsoft.AspNetCore.App`, référencez individuellement les packages de journalisation.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="2c4fb-132">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="2c4fb-132">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="2c4fb-133">L’intergiciel (middleware) d’intégration IIS, qui configure l’intergiciel des en-têtes transférés, et le module ASP.NET Core, sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-133">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="2c4fb-134">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-134">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="2c4fb-135">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="2c4fb-135">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="2c4fb-136">Surveillance et journalisation</span><span class="sxs-lookup"><span data-stu-id="2c4fb-136">Monitoring and logging</span></span>

<span data-ttu-id="2c4fb-137">Pour des informations de surveillance, de journalisation et de dépannage, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-137">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="2c4fb-138">Guide pratique pour surveiller des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c4fb-138">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="2c4fb-139">Découvrez comment consulter les quotas et les métriques des applications et des plans App Service.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-139">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="2c4fb-140">Activer la journalisation des diagnostics pour les applications web dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c4fb-140">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="2c4fb-141">Découvrez comment activer et accéder à la journalisation des diagnostics pour les codes d’état HTTP, les requêtes en échec et les activités de serveur web.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-141">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="2c4fb-142">Présentation de la gestion des erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c4fb-142">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="2c4fb-143">Découvrez les approches courantes permettant de gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-143">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="2c4fb-144">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c4fb-144">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="2c4fb-145">Découvrez comment diagnostiquer les problèmes de déploiements Azure App Service avec les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-145">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="2c4fb-146">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c4fb-146">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="2c4fb-147">Découvrez les erreurs de configuration de déploiement courantes dans les applications hébergées par Azure App Service/IIS, ainsi que des conseils de dépannage.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-147">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="2c4fb-148">Porte-clés de Protection des données et emplacements de déploiement</span><span class="sxs-lookup"><span data-stu-id="2c4fb-148">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="2c4fb-149">Les [clés de Protection des données](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sont persistantes dans le dossier *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-149">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="2c4fb-150">Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-150">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="2c4fb-151">Les clés ne sont pas protégées au repos.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-151">Keys aren't protected at rest.</span></span> <span data-ttu-id="2c4fb-152">Ce dossier fournit le porte-clés à toutes les instances d’une application dans un seul emplacement de déploiement.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-152">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="2c4fb-153">Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-153">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="2c4fb-154">Lors d’une permutation entre les emplacements de déploiement, aucun système utilisant la protection des données ne peut déchiffrer les données stockées à l’aide du porte-clés au sein de l’emplacement précédent.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-154">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="2c4fb-155">L’intergiciel (middleware) ASP.NET Cookie utilise la protection des données pour protéger ses cookies.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-155">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="2c4fb-156">Cela entraîne la déconnexion des utilisateurs des applications qui utilisent l’intergiciel ASP.NET Cookie standard.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-156">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="2c4fb-157">Pour une solution de porte-clés indépendante de l’emplacement, utilisez un fournisseur de porte-clés externe, tel que :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-157">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="2c4fb-158">Stockage Blob Azure</span><span class="sxs-lookup"><span data-stu-id="2c4fb-158">Azure Blob Storage</span></span>
* <span data-ttu-id="2c4fb-159">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="2c4fb-159">Azure Key Vault</span></span>
* <span data-ttu-id="2c4fb-160">Magasin SQL</span><span class="sxs-lookup"><span data-stu-id="2c4fb-160">SQL store</span></span>
* <span data-ttu-id="2c4fb-161">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="2c4fb-161">Redis cache</span></span>

<span data-ttu-id="2c4fb-162">Pour plus d’informations, consultez [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="2c4fb-162">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="2c4fb-163">Déployer la version préliminaire d’ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c4fb-163">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="2c4fb-164">Les applications basées sur la version préliminaire d’ASP.NET Core peuvent être déployées sur Azure App Service avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-164">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="2c4fb-165">[Installer l’extension de site de version Preview](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="2c4fb-165">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="2c4fb-166">Utiliser Docker avec Web Apps pour conteneurs</span><span class="sxs-lookup"><span data-stu-id="2c4fb-166">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

<span data-ttu-id="2c4fb-167">Si un problème se produit quand vous utilisez l’extension de site de version Preview, ouvrez un problème sur [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="2c4fb-167">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="2c4fb-168">Installer l’extension de site de version Preview</span><span class="sxs-lookup"><span data-stu-id="2c4fb-168">Install the preview site extension</span></span>

1. <span data-ttu-id="2c4fb-169">Dans le portail Azure, accédez au panneau App Service.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-169">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="2c4fb-170">Sélectionnez l’application web.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-170">Select the web app.</span></span>
1. <span data-ttu-id="2c4fb-171">Entrez « ex » dans la zone de recherche ou faites défiler la liste des volets de gestion jusqu’à **OUTILS DE DEVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-171">Enter "ex" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="2c4fb-172">Sélectionnez **OUTILS DE DEVELOPPEMENT** > **Extensions**.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-172">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="2c4fb-173">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-173">Select **Add**.</span></span>

   ![Panneau de l’application Azure avec les étapes précédentes](index/_static/x1.png)

1. <span data-ttu-id="2c4fb-175">Sélectionnez **Extensions ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-175">Select **ASP.NET Core Extensions**.</span></span>
1. <span data-ttu-id="2c4fb-176">Sélectionnez **OK** pour accepter les conditions légales.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-176">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="2c4fb-177">Sélectionnez **OK** pour installer l’extension.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-177">Select **OK** to install the extension.</span></span>

<span data-ttu-id="2c4fb-178">Quand les opérations d’ajout sont terminées, la dernière préversion de .NET Core est installée.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-178">When the add operations complete, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="2c4fb-179">Vérifiez l’installation en exécutant `dotnet --info` dans la console.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-179">Verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="2c4fb-180">Dans le panneau **App Service** :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-180">From the **App Service** blade:</span></span>

1. <span data-ttu-id="2c4fb-181">Entrez « con » dans la zone de recherche ou faites défiler la liste des volets de gestion jusqu’à **OUTILS DE DEVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-181">Enter "con" in the search box or scroll down the list of management panes to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="2c4fb-182">Sélectionnez **OUTILS DE DEVELOPPEMENT** > **Console**.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-182">Select **DEVELOPMENT TOOLS** > **Console**.</span></span>
1. <span data-ttu-id="2c4fb-183">Entrez `dotnet --info` dans la console.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-183">Enter `dotnet --info` in the console.</span></span>

<span data-ttu-id="2c4fb-184">Si la version `2.1.300-preview1-008174` est la dernière préversion, vous obtenez le résultat suivant en exécutant `dotnet --info` depuis l’invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-184">If version `2.1.300-preview1-008174` is the latest preview release, the following output is obtained by running `dotnet --info` at the command prompt:</span></span>

![Panneau de l’application Azure avec les étapes précédentes](index/_static/cons.png)

<span data-ttu-id="2c4fb-186">La version d’ASP.NET Core indiquée dans l’image précédente, `2.1.300-preview1-008174`, est un exemple.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-186">The version of ASP.NET Core shown in the preceding image, `2.1.300-preview1-008174`, is an example.</span></span> <span data-ttu-id="2c4fb-187">La dernière préversion d’ASP.NET Core au moment de la configuration de l’extension de site s’affiche quand vous exécutez `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-187">The latest preview version of ASP.NET Core at the time the site extension is configured appears when you execute `dotnet --info`.</span></span>

<span data-ttu-id="2c4fb-188">La commande `dotnet --info` affiche le chemin de l’extension de site où la préversion a été installée.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-188">The `dotnet --info` displays the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="2c4fb-189">Il indique que l’application s’exécute depuis l’extension de site au lieu de l’emplacement de *ProgramFiles* par défaut.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-189">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="2c4fb-190">Si vous voyez *ProgramFiles*, redémarrez le site et exécutez `dotnet --info`.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-190">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

<span data-ttu-id="2c4fb-191">**Utiliser l’extension de site de la version Preview avec un modèle ARM**</span><span class="sxs-lookup"><span data-stu-id="2c4fb-191">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="2c4fb-192">Si un modèle ARM est utilisé pour créer et déployer des applications, le type de ressource `siteextensions` peut être utilisé pour ajouter l’extension de site à une application web.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-192">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="2c4fb-193">Exemple :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-193">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="2c4fb-194">Utiliser Docker avec Web Apps pour conteneurs</span><span class="sxs-lookup"><span data-stu-id="2c4fb-194">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="2c4fb-195">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contient les dernières images de Docker en préversion.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-195">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="2c4fb-196">Les images peuvent être utilisées comme images de base.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-196">The images can be used as a base image.</span></span> <span data-ttu-id="2c4fb-197">Utilisez l’image pour effectuer un déploiement sur Web Apps pour conteneurs normalement.</span><span class="sxs-lookup"><span data-stu-id="2c4fb-197">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2c4fb-198">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2c4fb-198">Additional resources</span></span>

* [<span data-ttu-id="2c4fb-199">Vue d’ensemble de Web Apps (vidéo de 5 minutes)</span><span class="sxs-lookup"><span data-stu-id="2c4fb-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="2c4fb-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span><span class="sxs-lookup"><span data-stu-id="2c4fb-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="2c4fb-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span><span class="sxs-lookup"><span data-stu-id="2c4fb-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="2c4fb-202">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c4fb-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="2c4fb-203">Azure App Service sur Windows Server utilise [IIS (Internet Information Services)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="2c4fb-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="2c4fb-204">Les rubriques suivantes se rapportent à la technologie IIS sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="2c4fb-204">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="2c4fb-205">Bibliothèque Microsoft TechNet : Windows Server</span><span class="sxs-lookup"><span data-stu-id="2c4fb-205">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
