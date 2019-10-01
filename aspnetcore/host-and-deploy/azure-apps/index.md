---
title: Déployer des applications ASP.NET Core sur Azure App Service
author: guardrex
description: Cet article contient des liens vers des ressources d’hébergement et de déploiement Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/28/2019
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 7489868fac513948cbe6f48391e7260a34b2175e
ms.sourcegitcommit: dc96d76f6b231de59586fcbb989a7fb5106d26a8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703751"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a><span data-ttu-id="ed7ab-103">Déployer des applications ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ed7ab-103">Deploy ASP.NET Core apps to Azure App Service</span></span>

<span data-ttu-id="ed7ab-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) est un [service de plateforme de cloud computing Microsoft](https://azure.microsoft.com/) qui permet d’héberger des applications web, notamment ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="ed7ab-105">Ressources utiles</span><span class="sxs-lookup"><span data-stu-id="ed7ab-105">Useful resources</span></span>

<span data-ttu-id="ed7ab-106">[App Service Documentation](/azure/app-service/) héberge la documentation, les tutoriels, les exemples, les guides pratiques et d’autres ressources Azure.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-106">[App Service Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="ed7ab-107">Voici deux didacticiels importants qui abordent l’hébergement d’applications ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="ed7ab-108">Créer une application web ASP.NET Core dans Azure</span><span class="sxs-lookup"><span data-stu-id="ed7ab-108">Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="ed7ab-109">Utilisez Visual Studio pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Windows.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="ed7ab-110">Créer une application ASP.NET Core dans App Service sur Linux</span><span class="sxs-lookup"><span data-stu-id="ed7ab-110">Create an ASP.NET Core app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="ed7ab-111">Utilisez la ligne de commande pour créer et déployer une application web ASP.NET Core dans Azure App Service sur Linux.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="ed7ab-112">Consultez le [tableau de bord ASP.net Core sur app service](https://aspnetcoreon.azurewebsites.net/) pour connaître la version de ASP.net Core disponible sur Azure App service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-112">See the [ASP.NET Core on App Service Dashboard](https://aspnetcoreon.azurewebsites.net/) for the version of ASP.NET Core available on Azure App service.</span></span>

<span data-ttu-id="ed7ab-113">Les articles suivants sont disponibles dans la documentation d’ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-113">The following articles are available in ASP.NET Core documentation:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="ed7ab-114">Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
<span data-ttu-id="ed7ab-115">Découvrez comment créer une application web ASP.NET Core à l’aide de Visual Studio et comment la déployer sur Azure App Service en utilisant Git pour le déploiement continu.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-115">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="ed7ab-116">Créer votre premier pipeline</span><span class="sxs-lookup"><span data-stu-id="ed7ab-116">Create your first pipeline</span></span>](/azure/devops/pipelines/get-started-yaml)  
<span data-ttu-id="ed7ab-117">Configurez une build CI pour une application ASP.NET Core, puis créez une version de déploiement continu sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-117">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="ed7ab-118">Bac à sable Azure Web App</span><span class="sxs-lookup"><span data-stu-id="ed7ab-118">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="ed7ab-119">Découvrez les limitations d’exécution du runtime Azure App Service appliquées par la plateforme Azure Apps.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-119">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

<xref:test/troubleshoot>  
<span data-ttu-id="ed7ab-120">Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-120">Understand and troubleshoot warnings and errors with ASP.NET Core projects.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="ed7ab-121">Configuration d’application</span><span class="sxs-lookup"><span data-stu-id="ed7ab-121">Application configuration</span></span>

### <a name="platform"></a><span data-ttu-id="ed7ab-122">Plateforme</span><span class="sxs-lookup"><span data-stu-id="ed7ab-122">Platform</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ed7ab-123">Des runtimes pour les applications 64 bits (x64) et 32 bits (x 86) sont présents sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-123">Runtimes for 64-bit (x64) and 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="ed7ab-124">Le [SDK .NET Core](/dotnet/core/sdk) disponible sur App Service est 32 bits, mais vous pouvez déployer des applications 64 bits crées localement en utilisant la console [Kudu](https://github.com/projectkudu/kudu/wiki) ou le processus de publication de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-124">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit, but you can deploy 64-bit apps built locally using the [Kudu](https://github.com/projectkudu/kudu/wiki) console or the publish process in Visual Studio.</span></span> <span data-ttu-id="ed7ab-125">Pour plus d’informations, consultez la section [Publier et déployer l’application](#publish-and-deploy-the-app).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-125">For more information, see the [Publish and deploy the app](#publish-and-deploy-the-app) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ed7ab-126">Concernant les applications avec des dépendances natives, des runtimes pour les applications 32 bits (x86) sont présents sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-126">For apps with native dependencies, runtimes for 32-bit (x86) apps are present on Azure App Service.</span></span> <span data-ttu-id="ed7ab-127">Le [SDK .NET Core](/dotnet/core/sdk) disponible sur App Service est 32 bits.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-127">The [.NET Core SDK](/dotnet/core/sdk) available on App Service is 32-bit.</span></span>

::: moniker-end

<span data-ttu-id="ed7ab-128">Pour plus d’informations sur les composants et les méthodes de distribution du framework .NET Core, comme des informations sur le runtime .NET Core et le SDK .NET Core, consultez [À propos de .NET Core : Composition](/dotnet/core/about#composition).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-128">For more information on .NET Core framework components and distribution methods, such as information on the .NET Core runtime and the .NET Core SDK, see [About .NET Core: Composition](/dotnet/core/about#composition).</span></span>

### <a name="packages"></a><span data-ttu-id="ed7ab-129">Packages</span><span class="sxs-lookup"><span data-stu-id="ed7ab-129">Packages</span></span>

<span data-ttu-id="ed7ab-130">Ajoutez les packages NuGet suivants pour fournir des fonctionnalités de journalisation automatique aux applications déployées sur Azure App Service :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-130">Include the following NuGet packages to provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="ed7ab-131">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) utilise [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) pour intégrer la fonction d’éclairage ASP.NET Core dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-131">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) to provide ASP.NET Core light-up integration with Azure App Service.</span></span> <span data-ttu-id="ed7ab-132">Les fonctionnalités de journalisation ajoutées sont fournies par le package `Microsoft.AspNetCore.AzureAppServicesIntegration`.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-132">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="ed7ab-133">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) exécute [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) pour ajouter des fournisseurs de journalisation de diagnostics Azure App Service dans le package `Microsoft.Extensions.Logging.AzureAppServices`.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-133">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="ed7ab-134">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) fournit des implémentations de journaliseur pour prendre en charge les journaux de diagnostics et les fonctionnalités de streaming de journal Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-134">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

<span data-ttu-id="ed7ab-135">Les packages précédents ne sont pas disponibles dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-135">The preceding packages aren't available from the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="ed7ab-136">Les applications qui ciblent le .NET Framework ou référencent le métapackage `Microsoft.AspNetCore.App` doivent référencer explicitement les packages individuels dans le fichier projet de l’application.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-136">Apps that target .NET Framework or reference the `Microsoft.AspNetCore.App` metapackage must explicitly reference the individual packages in the app's project file.</span></span>

## <a name="override-app-configuration-using-the-azure-portal"></a><span data-ttu-id="ed7ab-137">Remplacer la configuration de l’application à l’aide du Portail Azure</span><span class="sxs-lookup"><span data-stu-id="ed7ab-137">Override app configuration using the Azure Portal</span></span>

<span data-ttu-id="ed7ab-138">Les paramètres d’application dans le portail Azure vous permettent de définir des variables d’environnement pour l’application.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-138">App settings in the Azure Portal permit you to set environment variables for the app.</span></span> <span data-ttu-id="ed7ab-139">Celles-ci peuvent être utilisées par le [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-139">Environment variables can be consumed by the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

<span data-ttu-id="ed7ab-140">Quand un paramètre d’application est créé ou modifié dans le portail Azure et que le bouton **Enregistrer** est sélectionné, Azure App redémarre.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-140">When an app setting is created or modified in the Azure Portal and the **Save** button is selected, the Azure App is restarted.</span></span> <span data-ttu-id="ed7ab-141">La variable d’environnement est à la disposition de l’application après le redémarrage du service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-141">The environment variable is available to the app after the service restarts.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ed7ab-142">Quand une application utilise [l’Hôte générique](xref:fundamentals/host/generic-host), les variables d’environnement ne sont par défaut pas chargées dans une configuration de l’application ; le fournisseur de configuration doit être ajouté par le développeur.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-142">When an app uses the [Generic Host](xref:fundamentals/host/generic-host), environment variables aren't loaded into an app's configuration by default and the configuration provider must be added by the developer.</span></span> <span data-ttu-id="ed7ab-143">Ce dernier détermine le préfixe de variable d’environnement lors de l’ajout du fournisseur de configuration.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-143">The developer determines the environment variable prefix when the configuration provider is added.</span></span> <span data-ttu-id="ed7ab-144">Pour plus d’informations, voir <xref:fundamentals/host/generic-host> et [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-144">For more information, see <xref:fundamentals/host/generic-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ed7ab-145">Quand une application génère l’hôte via [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), les variables d’environnement qui permettent de configurer l’hôte utilisent le préfixe `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-145">When an app builds the host using [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), environment variables that configure the host use the `ASPNETCORE_` prefix.</span></span> <span data-ttu-id="ed7ab-146">Pour plus d’informations, voir <xref:fundamentals/host/web-host> et [Fournisseur de configuration des variables d’environnement](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-146">For more information, see <xref:fundamentals/host/web-host> and the [Environment Variables Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span>

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ed7ab-147">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="ed7ab-147">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ed7ab-148">Le [middleware d’intégration IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), qui configure le middleware des en-têtes transférés lors de l’hébergement [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), et le module ASP.NET Core sont configurés pour transférer le schéma (HTTP/HTTPS) et l’adresse IP distante d’où provient la requête.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-148">The [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components), which configures Forwarded Headers Middleware when hosting [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="ed7ab-149">Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-149">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="ed7ab-150">Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-150">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="ed7ab-151">Surveillance et journalisation</span><span class="sxs-lookup"><span data-stu-id="ed7ab-151">Monitoring and logging</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ed7ab-152">Les applications ASP.NET Core déployées sur App Service reçoivent automatiquement une extension App Service, **Intégration de journalisation ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-152">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Integration**.</span></span> <span data-ttu-id="ed7ab-153">L’extension permet d’intégrer la journalisation dans les applications ASP.NET Core sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-153">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ed7ab-154">Les applications ASP.NET Core déployées sur App Service reçoivent automatiquement une extension App Service, **Extensions de journalisation ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-154">ASP.NET Core apps deployed to App Service automatically receive an App Service extension, **ASP.NET Core Logging Extensions**.</span></span> <span data-ttu-id="ed7ab-155">L’extension permet d’intégrer la journalisation dans les applications ASP.NET Core sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-155">The extension enables logging integration for ASP.NET Core apps on Azure App Service.</span></span>

::: moniker-end

<span data-ttu-id="ed7ab-156">Pour des informations de surveillance, de journalisation et de dépannage, consultez les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-156">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="ed7ab-157">Superviser des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ed7ab-157">Monitor apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="ed7ab-158">Découvrez comment consulter les quotas et les métriques des applications et des plans App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-158">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="ed7ab-159">Activer la journalisation des diagnostics pour les applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ed7ab-159">Enable diagnostics logging for apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="ed7ab-160">Découvrez comment activer et accéder à la journalisation des diagnostics pour les codes d’état HTTP, les requêtes en échec et les activités de serveur web.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-160">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="ed7ab-161">Découvrez les approches courantes permettant de gérer les erreurs dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-161">Understand common approaches to handling errors in ASP.NET Core apps.</span></span>

<xref:test/troubleshoot-azure-iis>  
<span data-ttu-id="ed7ab-162">Découvrez comment diagnostiquer les problèmes de déploiements Azure App Service avec les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-162">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

<xref:host-and-deploy/azure-iis-errors-reference>  
<span data-ttu-id="ed7ab-163">Découvrez les erreurs de configuration de déploiement courantes dans les applications hébergées par Azure App Service/IIS, ainsi que des conseils de dépannage.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-163">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="ed7ab-164">Porte-clés de Protection des données et emplacements de déploiement</span><span class="sxs-lookup"><span data-stu-id="ed7ab-164">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="ed7ab-165">Les [clés de Protection des données](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sont persistantes dans le dossier *%HOME%\ASP.NET\DataProtection-Keys*.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-165">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="ed7ab-166">Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-166">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="ed7ab-167">Les clés ne sont pas protégées au repos.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-167">Keys aren't protected at rest.</span></span> <span data-ttu-id="ed7ab-168">Ce dossier fournit le porte-clés à toutes les instances d’une application dans un seul emplacement de déploiement.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-168">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="ed7ab-169">Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-169">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="ed7ab-170">Lors d’une permutation entre les emplacements de déploiement, aucun système utilisant la protection des données ne peut déchiffrer les données stockées à l’aide du porte-clés au sein de l’emplacement précédent.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-170">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="ed7ab-171">L’intergiciel (middleware) ASP.NET Cookie utilise la protection des données pour protéger ses cookies.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-171">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="ed7ab-172">Cela entraîne la déconnexion des utilisateurs des applications qui utilisent l’intergiciel ASP.NET Cookie standard.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-172">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="ed7ab-173">Pour une solution de porte-clés indépendante de l’emplacement, utilisez un fournisseur de porte-clés externe, tel que :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-173">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="ed7ab-174">Stockage Blob Azure</span><span class="sxs-lookup"><span data-stu-id="ed7ab-174">Azure Blob Storage</span></span>
* <span data-ttu-id="ed7ab-175">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ed7ab-175">Azure Key Vault</span></span>
* <span data-ttu-id="ed7ab-176">Magasin SQL</span><span class="sxs-lookup"><span data-stu-id="ed7ab-176">SQL store</span></span>
* <span data-ttu-id="ed7ab-177">Cache Redis</span><span class="sxs-lookup"><span data-stu-id="ed7ab-177">Redis cache</span></span>

<span data-ttu-id="ed7ab-178">Pour plus d'informations, consultez <xref:security/data-protection/implementation/key-storage-providers>.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-178">For more information, see <xref:security/data-protection/implementation/key-storage-providers>.</span></span>
<a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>
<!-- revert this after 3.0 supported
## Deploy ASP.NET Core preview release to Azure App Service

Use one of the following approaches if the app relies on a preview release of .NET Core:

* [Install the preview site extension](#install-the-preview-site-extension).
* [Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).
* [Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).
-->
## <a name="deploy-aspnet-core-30-to-azure-app-service"></a><span data-ttu-id="ed7ab-179">Déployer ASP.NET Core 3,0 sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ed7ab-179">Deploy ASP.NET Core 3.0 to Azure App Service</span></span>

<span data-ttu-id="ed7ab-180">Nous espérons que ASP.NET Core 3,0 disponible sur Azure App Service prochainement.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-180">We hope to have ASP.NET Core 3.0 available on Azure App Service soon.</span></span>

<span data-ttu-id="ed7ab-181">Utilisez l’une des approches suivantes si l’application s’appuie sur .NET Core 3,0 :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-181">Use one of the following approaches if the app relies on .NET Core 3.0:</span></span>

* <span data-ttu-id="ed7ab-182">[Installer l’extension de site de préversion](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-182">[Install the preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="ed7ab-183">[Déployer une application en préversion autonome](#deploy-a-self-contained-preview-app).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-183">[Deploy a self-contained preview app](#deploy-a-self-contained-preview-app).</span></span>
* <span data-ttu-id="ed7ab-184">[Utiliser Docker avec Web Apps pour conteneurs](#use-docker-with-web-apps-for-containers).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-184">[Use Docker with Web Apps for containers](#use-docker-with-web-apps-for-containers).</span></span>

### <a name="install-the-preview-site-extension"></a><span data-ttu-id="ed7ab-185">Installer l’extension de site de version Preview</span><span class="sxs-lookup"><span data-stu-id="ed7ab-185">Install the preview site extension</span></span>

<span data-ttu-id="ed7ab-186">Si un problème se produit avec l’extension de site en préversion, ouvrez un [problème aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/issues).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-186">If a problem occurs using the preview site extension, open an [aspnet/AspNetCore issue](https://github.com/aspnet/AspNetCore/issues).</span></span>

1. <span data-ttu-id="ed7ab-187">Dans le portail Azure, accédez à App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-187">From the Azure Portal, navigate to the App Service.</span></span>
1. <span data-ttu-id="ed7ab-188">Sélectionnez l’application web.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-188">Select the web app.</span></span>
1. <span data-ttu-id="ed7ab-189">Tapez « ex » dans la zone de recherche pour filtrer sur les « Extensions » ou faites défiler la liste outils de gestion.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-189">Type "ex" in the search box to filter for "Extensions" or scroll down the list of management tools.</span></span>
1. <span data-ttu-id="ed7ab-190">Sélectionner **Extensions**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-190">Select **Extensions**.</span></span>
1. <span data-ttu-id="ed7ab-191">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-191">Select **Add**.</span></span>
1. <span data-ttu-id="ed7ab-192">Sélectionnez l’extension **ASP.NET Core {X.Y} ({x64|x86}) Runtime** dans la liste, où `{X.Y}` correspond à la préversion d’ASP.NET Core et `{x64|x86}` spécifie la plateforme.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-192">Select the **ASP.NET Core {X.Y} ({x64|x86}) Runtime** extension from the list, where `{X.Y}` is the ASP.NET Core preview version and `{x64|x86}` specifies the platform.</span></span>
1. <span data-ttu-id="ed7ab-193">Sélectionnez **OK** pour accepter les conditions légales.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-193">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="ed7ab-194">Sélectionnez **OK** pour installer l’extension.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-194">Select **OK** to install the extension.</span></span>

<span data-ttu-id="ed7ab-195">Une fois l’opération effectuée, la dernière préversion de .NET Core est installée.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-195">When the operation completes, the latest .NET Core preview is installed.</span></span> <span data-ttu-id="ed7ab-196">Vérifiez l’installation :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-196">Verify the installation:</span></span>

1. <span data-ttu-id="ed7ab-197">Sélectionnez **Outils avancés**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-197">Select **Advanced Tools**.</span></span>
1. <span data-ttu-id="ed7ab-198">Sélectionnez **Go** dans **Outils avancés**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-198">Select **Go** in **Advanced Tools**.</span></span>
1. <span data-ttu-id="ed7ab-199">Sélectionnez l’élément de menu **Console de débogage** > **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-199">Select the **Debug console** > **PowerShell** menu item.</span></span>
1. <span data-ttu-id="ed7ab-200">À l’invite PowerShell, exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-200">At the PowerShell prompt, execute the following command.</span></span> <span data-ttu-id="ed7ab-201">Remplacez `{X.Y}` par la version du runtime ASP.NET Core et `{PLATFORM}` par la plateforme dans la commande :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-201">Substitute the ASP.NET Core runtime version for `{X.Y}` and the platform for `{PLATFORM}` in the command:</span></span>

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.{PLATFORM}\
   ```

   <span data-ttu-id="ed7ab-202">La commande retourne `True` quand le runtime de la préversion x64 est installée.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-202">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="ed7ab-203">L’architecture de plateforme (x86/x64) d’une application App Services est définie dans les paramètres de l’application dans le portail Azure pour les applications hébergées sur une instance de calcul de série A ou supérieure.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-203">The platform architecture (x86/x64) of an App Services app is set in the app's settings in the Azure Portal for apps that are hosted on an A-series compute or better hosting tier.</span></span> <span data-ttu-id="ed7ab-204">Si l’application s’exécute en mode in-process et si la plateforme est configurée pour une architecture 64 bits (x64), le module ASP.NET Core utilise le runtime de la préversion 64 bits, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-204">If the app is run in in-process mode and the platform architecture is configured for 64-bit (x64), the ASP.NET Core Module uses the 64-bit preview runtime, if present.</span></span> <span data-ttu-id="ed7ab-205">Installez l’extension **ASP.NET Core {X.Y} (x64) Runtime**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-205">Install the **ASP.NET Core {X.Y} (x64) Runtime** extension.</span></span>
>
> <span data-ttu-id="ed7ab-206">Après avoir installé le runtime de la préversion x64, exécutez la commande suivante dans la fenêtre Commande Kudu PowerShell pour vérifier l’installation.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-206">After installing the x64 preview runtime, run the following command in the Kudu PowerShell command window to verify the installation.</span></span> <span data-ttu-id="ed7ab-207">Remplacez `{X.Y}` par la version du runtime ASP.NET Core dans la commande :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-207">Substitute the ASP.NET Core runtime version for `{X.Y}` in the command:</span></span>
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64\
> ```
>
> <span data-ttu-id="ed7ab-208">La commande retourne `True` quand le runtime de la préversion x64 est installée.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-208">The command returns `True` when the x64 preview runtime is installed.</span></span>

> [!NOTE]
> <span data-ttu-id="ed7ab-209">Les **extensions ASP.NET Core** permettent d’activer des fonctionnalités supplémentaires pour ASP.NET Core sur Azure App Services, par exemple la journalisation Azure.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-209">**ASP.NET Core Extensions** enables additional functionality for ASP.NET Core on Azure App Services, such as enabling Azure logging.</span></span> <span data-ttu-id="ed7ab-210">L’extension est installée automatiquement quand vous effectuez le déploiement à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-210">The extension is installed automatically when deploying from Visual Studio.</span></span> <span data-ttu-id="ed7ab-211">Si l’extension n’est pas installée, installez-la pour l’application.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-211">If the extension isn't installed, install it for the app.</span></span>

<span data-ttu-id="ed7ab-212">**Utiliser l’extension de site de la version Preview avec un modèle ARM**</span><span class="sxs-lookup"><span data-stu-id="ed7ab-212">**Use the preview site extension with an ARM template**</span></span>

<span data-ttu-id="ed7ab-213">Si un modèle ARM est utilisé pour créer et déployer des applications, le type de ressource `siteextensions` peut être utilisé pour ajouter l’extension de site à une application web.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-213">If an ARM template is used to create and deploy apps, the `siteextensions` resource type can be used to add the site extension to a web app.</span></span> <span data-ttu-id="ed7ab-214">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-214">For example:</span></span>

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-a-self-contained-preview-app"></a><span data-ttu-id="ed7ab-215">Déployer une application en préversion autonome</span><span class="sxs-lookup"><span data-stu-id="ed7ab-215">Deploy a self-contained preview app</span></span>

<span data-ttu-id="ed7ab-216">Un [déploiement autonome (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) qui cible une préversion runtime exécute le runtime de la préversion dans le déploiement.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-216">A [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) that targets a preview runtime carries the preview runtime in the deployment.</span></span>

<span data-ttu-id="ed7ab-217">Pendant le déploiement d’une application autonome :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-217">When deploying a self-contained app:</span></span>

* <span data-ttu-id="ed7ab-218">Le site dans Azure App Service ne requiert pas l’[extension du site de préversion](#install-the-preview-site-extension).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-218">The site in Azure App Service doesn't require the [preview site extension](#install-the-preview-site-extension).</span></span>
* <span data-ttu-id="ed7ab-219">L’application doit être publiée suivant une autre approche que dans le cadre de la publication d’un [déploiement en fonction de l’infrastructure (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-219">The app must be published following a different approach than when publishing for a [framework-dependent deployment (FDD)](/dotnet/core/deploying#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="ed7ab-220">Suivez les instructions de la section [Déployer l’application autonome](#deploy-the-app-self-contained).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-220">Follow the guidance in the [Deploy the app self-contained](#deploy-the-app-self-contained) section.</span></span>

### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="ed7ab-221">Utiliser Docker avec Web Apps pour conteneurs</span><span class="sxs-lookup"><span data-stu-id="ed7ab-221">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="ed7ab-222">[Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contient les dernières images de Docker en préversion.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-222">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest preview Docker images.</span></span> <span data-ttu-id="ed7ab-223">Les images peuvent être utilisées comme images de base.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-223">The images can be used as a base image.</span></span> <span data-ttu-id="ed7ab-224">Utilisez l’image pour effectuer un déploiement sur Web Apps pour conteneurs normalement.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-224">Use the image and deploy to Web Apps for Containers normally.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="ed7ab-225">Publier et déployer l’application</span><span class="sxs-lookup"><span data-stu-id="ed7ab-225">Publish and deploy the app</span></span>

### <a name="deploy-the-app-framework-dependent"></a><span data-ttu-id="ed7ab-226">Déployer l’application en fonction du framework</span><span class="sxs-lookup"><span data-stu-id="ed7ab-226">Deploy the app framework-dependent</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ed7ab-227">Pour un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) 64 bits :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-227">For a 64-bit [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

* <span data-ttu-id="ed7ab-228">Utilisez un SDK .NET Core 64 bits pour générer une application 64 bits.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-228">Use a 64-bit .NET Core SDK to build a 64-bit app.</span></span>
* <span data-ttu-id="ed7ab-229">Définissez **Plateforme** sur **64 bits** dans **Configuration** > **Paramètres généraux** d’App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-229">Set the **Platform** to **64 Bit** in the App Service's **Configuration** > **General settings**.</span></span> <span data-ttu-id="ed7ab-230">L’application doit utiliser un plan de service De base ou supérieur pour permettre le choix du nombre de bits de la plateforme.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-230">The app must use a Basic or higher service plan to enable the choice of platform bitness.</span></span>

::: moniker-end

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed7ab-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed7ab-231">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ed7ab-232">Sélectionnez **Générer** > **Publier {nom de l’application}** dans la barre d’outils de Visual Studio, ou cliquez avec le bouton droit sur le projet dans l’**Explorateur de solutions**, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-232">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="ed7ab-233">Dans la boîte de dialogue **Choisir une cible de publication**, confirmez qu’**App Service** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-233">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="ed7ab-234">Sélectionnez **Avancé**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-234">Select **Advanced**.</span></span> <span data-ttu-id="ed7ab-235">La boîte de dialogue **Publier** s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-235">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="ed7ab-236">Dans la boîte de dialogue **Publier** :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-236">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="ed7ab-237">Confirmez que la configuration **Mise en production** est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-237">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="ed7ab-238">Ouvrez la liste déroulante **Mode de déploiement** et sélectionnez **Dépendant du framework**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-238">Open the **Deployment Mode** drop-down list and select **Framework-Dependent**.</span></span>
   * <span data-ttu-id="ed7ab-239">Sélectionnez **Portable** comme **Runtime cible**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-239">Select **Portable** as the **Target Runtime**.</span></span>
   * <span data-ttu-id="ed7ab-240">Si vous devez supprimer des fichiers supplémentaires lors du déploiement, ouvrez **Options de publication de fichiers** et sélectionnez la case à cocher pour supprimer des fichiers supplémentaires à la destination.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-240">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="ed7ab-241">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-241">Select **Save**.</span></span>
1. <span data-ttu-id="ed7ab-242">Créez un nouveau site ou mettez à jour un site existant en suivant les autres invites de l'Assistant de publication.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-242">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed7ab-243">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed7ab-243">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="ed7ab-244">Dans le fichier projet, ne spécifiez pas d’[Identificateur d’exécution (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-244">In the project file, don't specify a [Runtime Identifier (RID)](/dotnet/core/rid-catalog).</span></span>

1. <span data-ttu-id="ed7ab-245">A partir d’un interpréteur de commandes, publiez l’application en configuration Release avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-245">From a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="ed7ab-246">Dans l’exemple suivant, l’application est publiée en tant qu’application dépendante du framework :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-246">In the following example, the app is published as a framework-dependent app:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="ed7ab-247">Déplacez le contenu du répertoire *bin/Release/{TARGET FRAMEWORK}/publish* vers le site dans App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-247">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="ed7ab-248">Si vous faites glisser le contenu du dossier *publish* depuis votre disque dur local ou votre partage réseau directement vers App service dans la console [Kudu](https://github.com/projectkudu/kudu/wiki), faites glisser les fichiers vers le dossier `D:\home\site\wwwroot` dans la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-248">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the [Kudu](https://github.com/projectkudu/kudu/wiki) console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="ed7ab-249">Déployer l’application autonome</span><span class="sxs-lookup"><span data-stu-id="ed7ab-249">Deploy the app self-contained</span></span>

<span data-ttu-id="ed7ab-250">Utilisez Visual Studio ou les outils de l’interface CLI pour un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-250">Use Visual Studio or the command-line interface (CLI) tools for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ed7ab-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed7ab-251">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ed7ab-252">Sélectionnez **Générer** > **Publier {nom de l’application}** dans la barre d’outils de Visual Studio, ou cliquez avec le bouton droit sur le projet dans l’**Explorateur de solutions**, puis sélectionnez **Publier**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-252">Select **Build** > **Publish {Application Name}** from the Visual Studio toolbar or right-click the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="ed7ab-253">Dans la boîte de dialogue **Choisir une cible de publication**, confirmez qu’**App Service** est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-253">In the **Pick a publish target** dialog, confirm that **App Service** is selected.</span></span>
1. <span data-ttu-id="ed7ab-254">Sélectionnez **Avancé**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-254">Select **Advanced**.</span></span> <span data-ttu-id="ed7ab-255">La boîte de dialogue **Publier** s’ouvre.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-255">The **Publish** dialog opens.</span></span>
1. <span data-ttu-id="ed7ab-256">Dans la boîte de dialogue **Publier** :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-256">In the **Publish** dialog:</span></span>
   * <span data-ttu-id="ed7ab-257">Confirmez que la configuration **Mise en production** est sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-257">Confirm that the **Release** configuration is selected.</span></span>
   * <span data-ttu-id="ed7ab-258">Ouvrez la liste déroulante **Mode de déploiement** et sélectionnez **Autonome**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-258">Open the **Deployment Mode** drop-down list and select **Self-Contained**.</span></span>
   * <span data-ttu-id="ed7ab-259">Sélectionnez le runtime cible à partir de la liste déroulante **Runtime cible**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-259">Select the target runtime from the **Target Runtime** drop-down list.</span></span> <span data-ttu-id="ed7ab-260">La valeur par défaut est `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-260">The default is `win-x86`.</span></span>
   * <span data-ttu-id="ed7ab-261">Si vous devez supprimer des fichiers supplémentaires lors du déploiement, ouvrez **Options de publication de fichiers** et sélectionnez la case à cocher pour supprimer des fichiers supplémentaires à la destination.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-261">If you need to remove additional files upon deployment, open **File Publish Options** and select the check box to remove additional files at the destination.</span></span>
   * <span data-ttu-id="ed7ab-262">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-262">Select **Save**.</span></span>
1. <span data-ttu-id="ed7ab-263">Créez un nouveau site ou mettez à jour un site existant en suivant les autres invites de l'Assistant de publication.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-263">Create a new site or update an existing site by following the remaining prompts of the publish wizard.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ed7ab-264">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed7ab-264">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="ed7ab-265">Dans le fichier projet, spécifiez un ou plusieurs [identificateurs de runtime (RID)](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-265">In the project file, specify one or more [Runtime Identifiers (RIDs)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="ed7ab-266">Utilisez `<RuntimeIdentifier>` (singulier) pour un seul RID, ou `<RuntimeIdentifiers>` (pluriel) pour fournir une liste de RID délimitée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-266">Use `<RuntimeIdentifier>` (singular) for a single RID, or use `<RuntimeIdentifiers>` (plural) to provide a semicolon-delimited list of RIDs.</span></span> <span data-ttu-id="ed7ab-267">Dans l’exemple suivant, le RID `win-x86` est spécifié :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-267">In the following example, the `win-x86` RID is specified:</span></span>

   ```xml
   <PropertyGroup>
     <TargetFramework>{TARGET FRAMEWORK}</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```

1. <span data-ttu-id="ed7ab-268">A partir d'un interpréteur de commandes, publiez l'application dans la configuration Mise en production pour le runtime de l'hôte avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-268">From a command shell, publish the app in Release configuration for the host's runtime with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="ed7ab-269">Dans l’exemple suivant, l’application est publiée pour le RID `win-x86`.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-269">In the following example, the app is published for the `win-x86` RID.</span></span> <span data-ttu-id="ed7ab-270">Le RID fourni à l’option `--runtime` doit être fourni dans la propriété `<RuntimeIdentifier>` (ou `<RuntimeIdentifiers>`) du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-270">The RID supplied to the `--runtime` option must be provided in the `<RuntimeIdentifier>` (or `<RuntimeIdentifiers>`) property in the project file.</span></span>

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```

1. <span data-ttu-id="ed7ab-271">Déplacez le contenu du répertoire *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* vers le site dans App Service.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-271">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* directory to the site in App Service.</span></span> <span data-ttu-id="ed7ab-272">Si vous faites glisser le contenu du dossier *publish* depuis votre disque dur local ou votre partage réseau directement vers App service dans la console Kudu, faites glisser les fichiers vers le dossier `D:\home\site\wwwroot` dans la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-272">If dragging the *publish* folder contents from your local hard drive or network share directly to App Service in the Kudu console, drag the files to the `D:\home\site\wwwroot` folder in the Kudu console.</span></span>

---

## <a name="protocol-settings-https"></a><span data-ttu-id="ed7ab-273">Paramètres de protocole (HTTPS)</span><span class="sxs-lookup"><span data-stu-id="ed7ab-273">Protocol settings (HTTPS)</span></span>

<span data-ttu-id="ed7ab-274">Les liaisons de protocole sécurisées permettent de spécifier un certificat à utiliser pour répondre à des requêtes sur HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-274">Secure protocol bindings allow you specify a certificate to use when responding to requests over HTTPS.</span></span> <span data-ttu-id="ed7ab-275">La liaison nécessite un certificat privé valide ( *.pfx*) émis pour le nom d’hôte spécifique.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-275">Binding requires a valid private certificate (*.pfx*) issued for the specific hostname.</span></span> <span data-ttu-id="ed7ab-276">Pour plus d'informations, consultez le [Tutoriel : Lier un certificat SSL personnalisé existant à Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-276">For more information, see [Tutorial: Bind an existing custom SSL certificate to Azure App Service](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

## <a name="transform-webconfig"></a><span data-ttu-id="ed7ab-277">Transformer web.config</span><span class="sxs-lookup"><span data-stu-id="ed7ab-277">Transform web.config</span></span>

<span data-ttu-id="ed7ab-278">Si vous devez transformer *web.config* lors de la publication (par exemple, définir les variables d’environnement basées sur la configuration, le profil ou l’environnement), consultez <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="ed7ab-278">If you need to transform *web.config* on publish (for example, set environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed7ab-279">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ed7ab-279">Additional resources</span></span>

* [<span data-ttu-id="ed7ab-280">Vue d’ensemble d’App Service</span><span class="sxs-lookup"><span data-stu-id="ed7ab-280">App Service overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="ed7ab-281">Azure App Service : The Best Place to Host your .NET Apps (vidéo de présentation de 55 minutes)</span><span class="sxs-lookup"><span data-stu-id="ed7ab-281">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="ed7ab-282">Azure Friday : Azure App Service Diagnostic and Troubleshooting Experience (vidéo de 12 minutes)</span><span class="sxs-lookup"><span data-stu-id="ed7ab-282">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="ed7ab-283">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ed7ab-283">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

<span data-ttu-id="ed7ab-284">Azure App Service sur Windows Server utilise [IIS (Internet Information Services)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ed7ab-284">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="ed7ab-285">Les rubriques suivantes se rapportent à la technologie IIS sous-jacente :</span><span class="sxs-lookup"><span data-stu-id="ed7ab-285">The following topics pertain to the underlying IIS technology:</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [<span data-ttu-id="ed7ab-286">Windows Server - contenu d’administrateur informatique pour les versions actuelles et antérieures</span><span class="sxs-lookup"><span data-stu-id="ed7ab-286">Windows Server - IT administrator content for current and previous releases</span></span>](/windows-server/windows-server-versions)
