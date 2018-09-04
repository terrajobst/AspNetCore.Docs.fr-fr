---
title: Héberger ASP.NET Core sur Azure App Service
author: guardrex
description: Découvrez comment héberger des applications ASP.NET Core dans Azure App Service avec des liens vers des ressources utiles.
ms.author: riande
ms.custom: mvc
ms.date: 08/29/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: bc2a686c5ddc44fded135c9eed5caf676218773a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312068"
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="02523-103">Héberger ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="02523-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="02523-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02523-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="02523-105">Ressources utiles</span><span class="sxs-lookup"><span data-stu-id="02523-105">Useful resources</span></span>

<span data-ttu-id="02523-106">La documentation d’[Azure Web Apps](/azure/app-service/) contient la documentation, les didacticiels, les exemples, les guides pratiques et d’autres ressources sur les applications Azure.</span><span class="sxs-lookup"><span data-stu-id="02523-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="02523-107">Voici deux didacticiels importants qui abordent l’hébergement d’applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="02523-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="02523-108">Démarrage rapide : créer une application web ASP.NET Core dans Azure</span><span class="sxs-lookup"><span data-stu-id="02523-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="02523-109">Utilisez Visual Studio pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Windows.</span><span class="sxs-lookup"><span data-stu-id="02523-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="02523-110">Démarrage rapide : créer une application web .NET Core dans App Service sur Linux</span><span class="sxs-lookup"><span data-stu-id="02523-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="02523-111">Utilisez la ligne de commande pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Linux.</span><span class="sxs-lookup"><span data-stu-id="02523-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="02523-112">Les articles suivants sont disponibles dans la documentation d’ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="02523-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="02523-113">Publier sur Azure avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02523-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="02523-114">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02523-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="02523-115">Publier sur Azure avec les outils CLI</span><span class="sxs-lookup"><span data-stu-id="02523-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="02523-116">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="02523-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="02523-117">Déploiement continu sur Azure avec Visual Studio et Git</span><span class="sxs-lookup"><span data-stu-id="02523-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="02523-118">Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.</span><span class="sxs-lookup"><span data-stu-id="02523-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="02523-119">Déploiement continu sur Azure avec VSTS</span><span class="sxs-lookup"><span data-stu-id="02523-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="02523-120">Configurez une build CI pour une application ASP.NET Core, puis créez une version de déploiement continu sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="02523-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="02523-121">Bac à sable Azure Web App</span><span class="sxs-lookup"><span data-stu-id="02523-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="02523-122">Découvrez les limitations d’exécution du runtime Azure App Service appliquées par la plateforme Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="02523-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a><span data-ttu-id="02523-123">Configuration d’application</span><span class="sxs-lookup"><span data-stu-id="02523-123">Application configuration</span></span>

<span data-ttu-id="02523-124">Dans ASP.NET Core 2.0 ou version ultérieure, les packages NuGet suivants fournissent des fonctionnalités de journalisation automatique pour les applications déployées sur Azure App Service :</span><span class="sxs-lookup"><span data-stu-id="02523-124">In ASP.NET Core 2.0 or later, the following NuGet packages provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="02523-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utilise [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) pour intégrer la fonction d’éclairage ASP.NET Core dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="02523-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="02523-126">Les fonctionnalités de journalisation ajoutées sont fournies par le package `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="02523-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="02523-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) exécute [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) pour ajouter des fournisseurs de journalisation de diagnostics Azure App Service dans le package `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="02523-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="02523-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fournit des implémentations de journaliseur pour prendre en charge les journaux de diagnostics et les fonctionnalités de streaming de journal Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="02523-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="02523-129">Si vous ciblez .NET Core et référencez le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage), les packages sont toujours inclus.</span><span class="sxs-lookup"><span data-stu-id="02523-129">If targeting .NET Core and referencing the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), the packages are already included.</span></span> <span data-ttu-id="02523-130">Les packages sont absents du [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) plus récent.</span><span class="sxs-lookup"><span data-stu-id="02523-130">The packages are absent from the newer [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="02523-131">Si vous ciblez le .NET Framework ou référencez le métapackage `Microsoft.AspNetCore.App`, référencez individuellement les packages de journalisation.</span><span class="sxs-lookup"><span data-stu-id="02523-131">If targeting .NET Framework or referencing the `Microsoft.AspNetCore.App` metapackage, reference the individual logging packages.</span></span>

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="02523-132">Remplacer la configuration de l’application à l’aide du Portail Azure</span><span class="sxs-lookup"><span data-stu-id="02523-132">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="02523-133">La zone **Paramètres de l’application** du panneau **Paramètres de l’application** permet de définir les variables d’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="02523-133">The **App Settings** area of the **Application settings** blade permits you to set environment variables for the app.</span></span> <span data-ttu-id="02523-134">Celles-ci peuvent être utilisées par le [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="02523-134">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="02523-135">Quand une application utilise [l’Hôte web](xref:fundamentals/host/web-host) et le génère avec [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), les variables d’environnement qui le configurent utilisent le préfixe `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="02523-135">When an app uses the [Web Host](xref:fundamentals/host/web-host) and builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="02523-136">Pour plus d’informations, voir <xref:fundamentals/host/web-host> et [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="02523-136">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="02523-137">Quand une application utilise [l’Hôte générique](xref:fundamentals/host/generic-host), les variables d’environnement ne sont par défaut pas chargées dans une configuration de l’application ; le fournisseur de configuration doit être ajouté par le développeur.</span><span class="sxs-lookup"><span data-stu-id="02523-137">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="02523-138">Ce dernier détermine le préfixe de variable d’environnement lors de l’ajout du fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="02523-138">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="02523-139">Pour plus d’informations, voir <xref:fundamentals/host/generic-host> et [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="02523-139">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="02523-140">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="02523-140">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="02523-141">L’intergiciel (middleware) d’intégration IIS, qui configure l’intergiciel des en-têtes transférés, et le module ASP.NET Core, sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête.</span><span class="sxs-lookup"><span data-stu-id="02523-141">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="02523-142">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="02523-142">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="02523-143">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="02523-143">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="02523-144">Surveillance et journalisation</span><span class="sxs-lookup"><span data-stu-id="02523-144">Monitoring and logging</span></span>

<span data-ttu-id="02523-145">Pour des informations de surveillance, de journalisation et de dépannage, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="02523-145">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="02523-146">Guide pratique pour surveiller des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="02523-146">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="02523-147">Découvrez comment consulter les quotas et les métriques des applications et des plans App Service.</span><span class="sxs-lookup"><span data-stu-id="02523-147">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="02523-148">Activer la journalisation des diagnostics pour les applications web dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="02523-148">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="02523-149">Découvrez comment activer et accéder à la journalisation des diagnostics pour les codes d’état HTTP, les requêtes en échec et les activités de serveur web.</span><span class="sxs-lookup"><span data-stu-id="02523-149">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="02523-150">Présentation de la gestion des erreurs dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02523-150">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="02523-151">Découvrez les approches courantes permettant de gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02523-151">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="02523-152">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="02523-152">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="02523-153">Découvrez comment diagnostiquer les problèmes de déploiements Azure App Service avec les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02523-153">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="02523-154">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02523-154">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="02523-155">Découvrez les erreurs de configuration de déploiement courantes dans les applications hébergées par Azure App Service/IIS, ainsi que des conseils de dépannage.</span><span class="sxs-lookup"><span data-stu-id="02523-155">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="02523-156">Porte-clés de Protection des données et emplacements de déploiement</span><span class="sxs-lookup"><span data-stu-id="02523-156">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="02523-157">Les [clés de Protection des données](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sont persistantes dans le dossier *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="02523-157">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="02523-158">Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application.</span><span class="sxs-lookup"><span data-stu-id="02523-158">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="02523-159">Les clés ne sont pas protégées au repos.</span><span class="sxs-lookup"><span data-stu-id="02523-159">Keys aren't protected at rest.</span></span> <span data-ttu-id="02523-160">Ce dossier fournit le porte-clés à toutes les instances d’une application dans un seul emplacement de déploiement.</span><span class="sxs-lookup"><span data-stu-id="02523-160">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="02523-161">Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés.</span><span class="sxs-lookup"><span data-stu-id="02523-161">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="02523-162">Lors d’une permutation entre les emplacements de déploiement, aucun système utilisant la protection des données ne peut déchiffrer les données stockées à l’aide du porte-clés au sein de l’emplacement précédent.</span><span class="sxs-lookup"><span data-stu-id="02523-162">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="02523-163">L’intergiciel (middleware) ASP.NET Cookie utilise la protection des données pour protéger ses cookies.</span><span class="sxs-lookup"><span data-stu-id="02523-163">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="02523-164">Cela entraîne la déconnexion des utilisateurs des applications qui utilisent l’intergiciel ASP.NET Cookie standard.</span><span class="sxs-lookup"><span data-stu-id="02523-164">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="02523-165">Pour une solution de porte-clés indépendante de l’emplacement, utilisez un fournisseur de porte-clés externe, tel que :</span><span class="sxs-lookup"><span data-stu-id="02523-165">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="02523-166">Stockage Blob Azure</span><span class="sxs-lookup"><span data-stu-id="02523-166">Azure Blob Storage</span></span>
* <span data-ttu-id="02523-167">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02523-167">Azure Key Vault</span></span>
* <span data-ttu-id="02523-168">Magasin SQL</span><span class="sxs-lookup"><span data-stu-id="02523-168">SQL store</span></span>
* <span data-ttu-id="02523-169">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="02523-169">Redis cache</span></span>

<span data-ttu-id="02523-170">Pour plus d’informations, consultez [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="02523-170">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="02523-171">Déployer la version préliminaire d’ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="02523-171">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="02523-172">Les applications basées sur la version préliminaire d’ASP.NET Core peuvent être déployées sur Azure App Service avec les approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="02523-172">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* <span data-ttu-id="02523-173">[Installer l’extension de site de version Preview](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span><span class="sxs-lookup"><span data-stu-id="02523-173">[Install the preview site extension](#install-the-preview-site-extension)
<!-- * [Deploy the app self-contained](#deploy-the-app-self-contained) --></span></span>
* [<span data-ttu-id="02523-174">Utiliser Docker avec Web Apps pour conteneurs</span><span class="sxs-lookup"><span data-stu-id="02523-174">Use Docker with Web Apps for containers</span></span>](#use-docker-with-web-apps-for-containers)

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="02523-175">Installer l’extension de site de version Preview</span><span class="sxs-lookup"><span data-stu-id="02523-175">Install the preview site extension</span></span>

<span data-ttu-id="02523-176">Si un problème se produit quand vous utilisez l’extension de site de version Preview, ouvrez un problème sur [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="02523-176">If a problem occurs using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

1. <span data-ttu-id="02523-177">Dans le portail Azure, accédez au panneau App Service.</span><span class="sxs-lookup"><span data-stu-id="02523-177">From the Azure portal, navigate to the App Service blade.</span></span>
1. <span data-ttu-id="02523-178">Sélectionnez l’application web.</span><span class="sxs-lookup"><span data-stu-id="02523-178">Select the web app.</span></span>
1. <span data-ttu-id="02523-179">Tapez « ex » dans la zone de recherche, ou faites défiler la liste des sections de gestion jusqu’à **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="02523-179">Type "ex" in the search box or scroll down the list of management sections to **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="02523-180">Sélectionnez **OUTILS DE DEVELOPPEMENT** > **Extensions**.</span><span class="sxs-lookup"><span data-stu-id="02523-180">Select **DEVELOPMENT TOOLS** > **Extensions**.</span></span>
1. <span data-ttu-id="02523-181">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="02523-181">Select **Add**.</span></span>
1. <span data-ttu-id="02523-182">Sélectionnez l’extension **ASP.NET Core &lt;x.y&gt; (x86) Runtime** dans la liste, où `<x.y>` correspond à la préversion d’ASP.NET Core (par exemple **ASP.NET Core 2.2 (x86) Runtime**).</span><span class="sxs-lookup"><span data-stu-id="02523-182">Select the **ASP.NET Core &lt;x.y&gt; (x86) Runtime** extension from the list, where `<x.y>` is the ASP.NET Core preview version (for example, **ASP.NET Core 2.2 (x86) Runtime**).</span></span> <span data-ttu-id="02523-183">Le runtime x86 convient aux [déploiements dépendants du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd), qui reposent sur un hébergement hors processus effectué par le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02523-183">The x86 runtime is appropriate for [framework-dependent deployments](/dotnet/core/deploying/#framework-dependent-deployments-fdd) that rely on out-of-process hosting by the ASP.NET Core Module.</span></span>
1. <span data-ttu-id="02523-184">Sélectionnez **OK** pour accepter les conditions légales.</span><span class="sxs-lookup"><span data-stu-id="02523-184">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="02523-185">Sélectionnez **OK** pour installer l’extension.</span><span class="sxs-lookup"><span data-stu-id="02523-185">Select **OK** to install the extension.</span></span>

<span data-ttu-id="02523-186">Une fois l’opération effectuée, la dernière préversion de .NET Core est installée.</span><span class="sxs-lookup"><span data-stu-id="02523-186">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="02523-187">Vérifiez l’installation :</span><span class="sxs-lookup"><span data-stu-id="02523-187">Verify the installation:</span></span>

1. <span data-ttu-id="02523-188">Sélectionnez **Outils avancés** sous **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="02523-188">Select **Advanced Tools** under **DEVELOPMENT TOOLS**.</span></span>
1. <span data-ttu-id="02523-189">Sélectionnez **OK** dans le panneau **Outils avancés**.</span><span class="sxs-lookup"><span data-stu-id="02523-189">Select **Go** on the **Advanced Tools** blade.</span></span>
1. <span data-ttu-id="02523-190">Sélectionnez l’élément de menu **Console de débogage** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="02523-190">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="02523-191">À l’invite PowerShell, exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="02523-191">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="02523-192">Remplacez `<x.y>` par la version du runtime ASP.NET Core dans la commande :</span><span class="sxs-lookup"><span data-stu-id="02523-192">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   <span data-ttu-id="02523-193">Si le runtime de la préversion installée est ASP.NET Core 2.2, la commande est la suivante :</span><span class="sxs-lookup"><span data-stu-id="02523-193">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   <span data-ttu-id="02523-194">La commande retourne `True` quand le runtime de la préversion x64 est installée.</span><span class="sxs-lookup"><span data-stu-id="02523-194">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="02523-195">L’architecture de plateforme (x86/x64) d’une application App Services est définie dans le panneau **Paramètres de l’application** sous **Paramètres généraux** pour les applications hébergées sur une instance de calcul de série A ou supérieure.</span><span class="sxs-lookup"><span data-stu-id="02523-195">The platform architecture (x86/x64) of an App Services app is set in the **Application Settings** blade under **General Settings** for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="02523-196">Si l’application s’exécute en mode in-process et si la plateforme est configurée pour une architecture 64 bits (x64), le module ASP.NET Core utilise le runtime de la préversion 64 bits, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="02523-196">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="02523-197">Installez l’extension **ASP.NET Core &lt;x.y&gt; (x64) Runtime** (par exemple **ASP.NET Core 2.2 (x64) Runtime**).</span><span class="sxs-lookup"><span data-stu-id="02523-197">Install the **ASP.NET Core &lt;x.y&gt; (x64) Runtime** extension (for example, **ASP.NET Core 2.2 (x64) Runtime**).</span></span>
>
> <span data-ttu-id="02523-198">Après avoir installé le runtime de la préversion x64, exécutez la commande suivante dans la fenêtre Commande Kudu PowerShell pour vérifier l’installation.</span><span class="sxs-lookup"><span data-stu-id="02523-198">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="02523-199">Remplacez `<x.y>` par la version du runtime ASP.NET Core dans la commande :</span><span class="sxs-lookup"><span data-stu-id="02523-199">Substitute the ASP.NET Core runtime version for `<x.y>` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> <span data-ttu-id="02523-200">Si le runtime de la préversion installée est ASP.NET Core 2.2, la commande est la suivante :</span><span class="sxs-lookup"><span data-stu-id="02523-200">If the installed preview runtime is for ASP.NET Core 2.2, the command is:</span></span>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> <span data-ttu-id="02523-201">La commande retourne `True` quand le runtime de la préversion x64 est installée.</span><span class="sxs-lookup"><span data-stu-id="02523-201">The command returns `True` when the x64 preview runtime is installed.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="02523-202">Les **extensions ASP.NET Core** permettent d’activer des fonctionnalités supplémentaires pour ASP.NET Core sur Azure App Services, par exemple la journalisation Azure.</span><span class="sxs-lookup"><span data-stu-id="02523-202">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="02523-203">L’extension est installée automatiquement quand vous effectuez le déploiement à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02523-203">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="02523-204">Si l’extension n’est pas installée, installez-la pour l’application.</span><span class="sxs-lookup"><span data-stu-id="02523-204">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="02523-205">**Utiliser l’extension de site de la version Preview avec un modèle ARM**</span><span class="sxs-lookup"><span data-stu-id="02523-205">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="02523-206">Si un modèle ARM est utilisé pour créer et déployer des applications, le type de ressource `siteextensions` peut être utilisé pour ajouter l’extension de site à une application web.</span><span class="sxs-lookup"><span data-stu-id="02523-206">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="02523-207">Exemple :</span><span class="sxs-lookup"><span data-stu-id="02523-207">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<!--
### Deploy the app self-contained

A [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) can be deployed that carries the preview runtime in the deployment. When deploying a self-contained app:

* The site doesn't need to be prepared.
* The app must be published differently than when publishing for a framework-dependent deployment with the shared runtime and host on the server.

Self-contained apps are an option for all ASP.NET Core apps.
-->

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="02523-208">Utiliser Docker avec Web Apps pour conteneurs</span><span class="sxs-lookup"><span data-stu-id="02523-208">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="02523-209">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contient les dernières images de Docker en préversion.</span><span class="sxs-lookup"><span data-stu-id="02523-209">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="02523-210">Les images peuvent être utilisées comme images de base.</span><span class="sxs-lookup"><span data-stu-id="02523-210">The images can be used as a base image.</span></span> <span data-ttu-id="02523-211">Utilisez l’image pour effectuer un déploiement sur Web Apps pour conteneurs normalement.</span><span class="sxs-lookup"><span data-stu-id="02523-211">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02523-212">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="02523-212">Additional resources</span></span>

* [<span data-ttu-id="02523-213">Vue d’ensemble de Web Apps (vidéo de 5 minutes)</span><span class="sxs-lookup"><span data-stu-id="02523-213">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="02523-214">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span><span class="sxs-lookup"><span data-stu-id="02523-214">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="02523-215">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span><span class="sxs-lookup"><span data-stu-id="02523-215">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="02523-216">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="02523-216">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="02523-217">Azure App Service sur Windows Server utilise [IIS (Internet Information Services)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="02523-217">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="02523-218">Les rubriques suivantes se rapportent à la technologie IIS sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="02523-218">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="02523-219">Bibliothèque Microsoft TechNet : Windows Server</span><span class="sxs-lookup"><span data-stu-id="02523-219">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
