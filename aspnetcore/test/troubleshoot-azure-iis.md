---
title: Résoudre les problèmes de ASP.NET Core sur Azure App Service et IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Azure App Service et Internet Information Services (IIS) des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2020
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 071dba9e936351e201b7582b3d0667cd6fac54bb
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294617"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="a6264-103">Résoudre les problèmes de ASP.NET Core sur Azure App Service et IIS</span><span class="sxs-lookup"><span data-stu-id="a6264-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="a6264-104">Par [Luke Latham](https://github.com/guardrex) et [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="a6264-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="a6264-105">Cet article fournit des informations sur les erreurs de démarrage d’application courantes et des instructions sur la façon de diagnostiquer les erreurs quand une application est déployée sur Azure App Service ou IIS :</span><span class="sxs-lookup"><span data-stu-id="a6264-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="a6264-106">Erreurs de démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="a6264-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="a6264-107">Explique les scénarios courants de code d’état HTTP de démarrage.</span><span class="sxs-lookup"><span data-stu-id="a6264-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="a6264-108">Résoudre les problèmes sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a6264-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="a6264-109">Fournit des conseils de dépannage pour les applications déployées sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a6264-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="a6264-110">Résoudre les problèmes sur IIS</span><span class="sxs-lookup"><span data-stu-id="a6264-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="a6264-111">Fournit des conseils de dépannage pour les applications déployées sur IIS ou exécutées localement sur IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a6264-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="a6264-112">Ce guide s’applique aux déploiements Windows Server et Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="a6264-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="a6264-113">Effacer les caches de package</span><span class="sxs-lookup"><span data-stu-id="a6264-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="a6264-114">Explique ce qu’il faut faire quand des packages incohérents interrompent une application lors des mises à niveau majeures ou de la modification des versions de package.</span><span class="sxs-lookup"><span data-stu-id="a6264-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="a6264-115">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a6264-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="a6264-116">Répertorie des rubriques supplémentaires sur la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="a6264-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="a6264-117">Erreurs de démarrage des applications</span><span class="sxs-lookup"><span data-stu-id="a6264-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a6264-118">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="a6264-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="a6264-119">Un *échec de processus 502,5* ou un *échec de démarrage de 500,30* qui se produit lorsque le débogage local peut être diagnostiqué à l’aide des conseils de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="a6264-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a6264-120">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="a6264-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="a6264-121">Un *échec de processus 502,5* qui se produit lorsque le débogage local peut être diagnostiqué à l’aide des conseils de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="a6264-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="a6264-122">403,14 interdit</span><span class="sxs-lookup"><span data-stu-id="a6264-122">403.14 Forbidden</span></span>

<span data-ttu-id="a6264-123">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-123">The app fails to start.</span></span> <span data-ttu-id="a6264-124">L’erreur suivante est enregistrée :</span><span class="sxs-lookup"><span data-stu-id="a6264-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="a6264-125">L’erreur est généralement causée par un déploiement rompu sur le système d’hébergement, qui comprend l’un des scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="a6264-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="a6264-126">L’application est déployée dans le mauvais dossier sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a6264-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="a6264-127">Le processus de déploiement n’a pas réussi à déplacer tous les fichiers et dossiers de l’application vers le dossier de déploiement sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a6264-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="a6264-128">Le fichier *Web. config* est manquant dans le déploiement ou le contenu du fichier *Web. config* est incorrect.</span><span class="sxs-lookup"><span data-stu-id="a6264-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="a6264-129">Effectuez les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="a6264-129">Perform the following steps:</span></span>

1. <span data-ttu-id="a6264-130">Supprimez tous les fichiers et dossiers du dossier de déploiement sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="a6264-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="a6264-131">Redéployez le contenu du dossier de *publication* de l’application sur le système d’hébergement à l’aide de votre méthode de déploiement normale, telle que Visual Studio, PowerShell ou le déploiement manuel :</span><span class="sxs-lookup"><span data-stu-id="a6264-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="a6264-132">Vérifiez que le fichier *Web. config* est présent dans le déploiement et que son contenu est correct.</span><span class="sxs-lookup"><span data-stu-id="a6264-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="a6264-133">Lors de l’hébergement sur Azure App Service, vérifiez que l’application est déployée dans le dossier `D:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="a6264-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="a6264-134">Lorsque l’application est hébergée par IIS, vérifiez que l’application est déployée sur le **chemin d’accès physique** IIS indiqué dans les **paramètres de base**du gestionnaire des **services Internet**.</span><span class="sxs-lookup"><span data-stu-id="a6264-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="a6264-135">Confirmez que tous les fichiers et dossiers de l’application sont déployés en comparant le déploiement sur le système d’hébergement au contenu du dossier de *publication* du projet.</span><span class="sxs-lookup"><span data-stu-id="a6264-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="a6264-136">Pour plus d’informations sur la disposition d’une application ASP.NET Core publiée, consultez <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="a6264-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="a6264-137">Pour plus d’informations sur le fichier *Web. config* , consultez <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="a6264-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="a6264-138">Erreur de serveur interne 500</span><span class="sxs-lookup"><span data-stu-id="a6264-138">500 Internal Server Error</span></span>

<span data-ttu-id="a6264-139">L’application démarre, mais une erreur empêche le serveur de répondre à la requête.</span><span class="sxs-lookup"><span data-stu-id="a6264-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="a6264-140">Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="a6264-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="a6264-141">La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="a6264-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="a6264-142">Le Journal des événements de l’application indique généralement qu’elle a démarré normalement.</span><span class="sxs-lookup"><span data-stu-id="a6264-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="a6264-143">Du point de vue du serveur, c’est exact.</span><span class="sxs-lookup"><span data-stu-id="a6264-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="a6264-144">L’application a démarré, mais elle ne peut pas générer de réponse valide.</span><span class="sxs-lookup"><span data-stu-id="a6264-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="a6264-145">Exécutez l’application à partir d’une invite de commandes sur le serveur ou activez le journal stdout du module ASP.NET Core pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="a6264-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="a6264-146">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="a6264-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="a6264-147">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-147">The worker process fails.</span></span> <span data-ttu-id="a6264-148">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-148">The app doesn't start.</span></span>

<span data-ttu-id="a6264-149">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) ne parvient pas à trouver le CLR .net Core et à rechercher le gestionnaire de demandes in-process (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="a6264-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="a6264-150">Vérifiez les points suivants :</span><span class="sxs-lookup"><span data-stu-id="a6264-150">Check that:</span></span>

* <span data-ttu-id="a6264-151">l’application cible le package NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) ou le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ;</span><span class="sxs-lookup"><span data-stu-id="a6264-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="a6264-152">la version du framework partagé ASP.NET Core que l’application cible est installée sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="a6264-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="a6264-153">500.0 - Échec de chargement du gestionnaire out-of-process</span><span class="sxs-lookup"><span data-stu-id="a6264-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="a6264-154">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-154">The worker process fails.</span></span> <span data-ttu-id="a6264-155">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-155">The app doesn't start.</span></span>

<span data-ttu-id="a6264-156">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) ne parvient pas à trouver le gestionnaire de demandes d’hébergement hors processus.</span><span class="sxs-lookup"><span data-stu-id="a6264-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="a6264-157">Vérifiez que le fichier *aspnetcorev2_outofprocess.dll* est présent dans un sous-dossier en regard de *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="a6264-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="a6264-158">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="a6264-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="a6264-159">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-159">The worker process fails.</span></span> <span data-ttu-id="a6264-160">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-160">The app doesn't start.</span></span>

<span data-ttu-id="a6264-161">Une erreur inconnue s’est produite lors du chargement des composants du [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="a6264-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="a6264-162">Effectuez l’une des actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="a6264-162">Take one of the following actions:</span></span>

* <span data-ttu-id="a6264-163">Contactez le [Support Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) (sélectionnez **Outils de développement**, puis **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="a6264-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="a6264-164">Posez une question sur Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="a6264-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="a6264-165">Signalez un problème sur notre [dépôt GitHub](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="a6264-165">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="a6264-166">500.30 - Échec du démarrage in-process</span><span class="sxs-lookup"><span data-stu-id="a6264-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="a6264-167">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-167">The worker process fails.</span></span> <span data-ttu-id="a6264-168">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-168">The app doesn't start.</span></span>

<span data-ttu-id="a6264-169">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) tente de démarrer le CLR .net Core in-process, mais il ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="a6264-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="a6264-170">La cause d’un échec de démarrage d’un processus peut généralement être déterminée à partir des entrées dans le journal des événements d’application et le journal stdout du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6264-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="a6264-171">Conditions d’échec courantes :</span><span class="sxs-lookup"><span data-stu-id="a6264-171">Common failure conditions:</span></span>

* <span data-ttu-id="a6264-172">L’application n’est pas configurée en raison du ciblage d’une version du Framework partagé ASP.NET Core qui n’est pas présent.</span><span class="sxs-lookup"><span data-stu-id="a6264-172">The app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="a6264-173">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="a6264-173">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>
* <span data-ttu-id="a6264-174">À l’aide de Azure Key Vault, le manque d’autorisations au Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a6264-174">Using Azure Key Vault, lack of permissions to the Key Vault.</span></span> <span data-ttu-id="a6264-175">Vérifiez les stratégies d’accès dans le Key Vault ciblé pour vous assurer que les autorisations appropriées sont accordées.</span><span class="sxs-lookup"><span data-stu-id="a6264-175">Check the access policies in the targeted Key Vault to ensure that the correct permissions are granted.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="a6264-176">500.31 - Échec de la recherche de dépendances natives par ANCM</span><span class="sxs-lookup"><span data-stu-id="a6264-176">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="a6264-177">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-177">The worker process fails.</span></span> <span data-ttu-id="a6264-178">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-178">The app doesn't start.</span></span>

<span data-ttu-id="a6264-179">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) tente de démarrer le Runtime .net Core in-process, mais il ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="a6264-179">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="a6264-180">La cause la plus courante de cet échec de démarrage est liée à la non-installation du runtime `Microsoft.NETCore.App` ou `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="a6264-180">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="a6264-181">Si l’application est déployée pour cibler ASP.NET Core 3.0, et si cette version n’existe pas sur la machine, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="a6264-181">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="a6264-182">Voici un exemple de message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="a6264-182">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="a6264-183">Le message d’erreur liste toutes les versions installées de .NET Core ainsi que la version demandée par l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-183">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="a6264-184">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="a6264-184">To fix this error, either:</span></span>

* <span data-ttu-id="a6264-185">Installez la version appropriée de .NET Core sur la machine.</span><span class="sxs-lookup"><span data-stu-id="a6264-185">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="a6264-186">Changez l’application pour cibler une version de .NET Core présente sur la machine.</span><span class="sxs-lookup"><span data-stu-id="a6264-186">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="a6264-187">Publiez l’application sous forme de [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="a6264-187">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="a6264-188">Durant l’exécution en phase de développement (la variable d’environnement `ASPNETCORE_ENVIRONMENT` a la valeur `Development`), l’erreur spécifique est écrite dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="a6264-188">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="a6264-189">La cause de l’échec du démarrage d’un processus se trouve également dans le journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-189">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="a6264-190">500.32 - Échec du chargement de la dll par ANCM</span><span class="sxs-lookup"><span data-stu-id="a6264-190">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="a6264-191">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-191">The worker process fails.</span></span> <span data-ttu-id="a6264-192">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-192">The app doesn't start.</span></span>

<span data-ttu-id="a6264-193">Le plus souvent, cette erreur provient du fait que l’application est publiée pour une architecture de processeur incompatible.</span><span class="sxs-lookup"><span data-stu-id="a6264-193">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="a6264-194">Si le processus de travail s’exécute en tant qu’application 32 bits et si l’application a été publiée pour cibler une architecture 64 bits, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="a6264-194">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="a6264-195">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="a6264-195">To fix this error, either:</span></span>

* <span data-ttu-id="a6264-196">Republiez l’application pour la même architecture de processeur que celle du processus de travail.</span><span class="sxs-lookup"><span data-stu-id="a6264-196">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="a6264-197">Publiez l’application sous forme de [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="a6264-197">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="a6264-198">500.33 - Échec du chargement du gestionnaire de requêtes ANCM</span><span class="sxs-lookup"><span data-stu-id="a6264-198">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="a6264-199">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-199">The worker process fails.</span></span> <span data-ttu-id="a6264-200">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-200">The app doesn't start.</span></span>

<span data-ttu-id="a6264-201">L’application n’a pas référencé le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="a6264-201">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="a6264-202">Seules les applications ciblant l’infrastructure `Microsoft.AspNetCore.App` peuvent être hébergées par le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a6264-202">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="a6264-203">Pour corriger cette erreur, vérifiez que l’application cible le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="a6264-203">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="a6264-204">Examinez le fichier `.runtimeconfig.json` pour vérifier le framework ciblé par l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-204">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="a6264-205">500.34 - Modèles d’hébergement mixtes ANCM non pris en charge</span><span class="sxs-lookup"><span data-stu-id="a6264-205">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="a6264-206">Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="a6264-206">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="a6264-207">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="a6264-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="a6264-208">500.35 - Applications in-process multiples dans le même processus ANCM</span><span class="sxs-lookup"><span data-stu-id="a6264-208">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="a6264-209">Le processus de travail ne peut pas exécuter plusieurs applications in-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="a6264-209">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="a6264-210">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="a6264-210">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="a6264-211">500.36 - Échec de chargement du gestionnaire out-of-process ANCM</span><span class="sxs-lookup"><span data-stu-id="a6264-211">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="a6264-212">Le gestionnaire de requêtes out-of-process, *aspnetcorev2_outofprocess.dll*, ne se trouve pas aux côtés du fichier *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="a6264-212">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="a6264-213">Cela indique une installation endommagée du [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a6264-213">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="a6264-214">Pour corriger cette erreur, réparez l’installation du [bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (pour IIS) ou Visual Studio (pour IIS Express).</span><span class="sxs-lookup"><span data-stu-id="a6264-214">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="a6264-215">500.37 - Échec du démarrage d’ANCM dans le délai imparti</span><span class="sxs-lookup"><span data-stu-id="a6264-215">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="a6264-216">ANCM n’a pas pu démarrer dans le délai imparti spécifié.</span><span class="sxs-lookup"><span data-stu-id="a6264-216">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="a6264-217">Par défaut, le délai d’expiration est de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="a6264-217">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="a6264-218">Cette erreur peut se produire quand un grand nombre d’applications démarrent sur la même machine.</span><span class="sxs-lookup"><span data-stu-id="a6264-218">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="a6264-219">Recherchez les pics d’utilisation du processeur/de la mémoire sur le serveur durant le démarrage.</span><span class="sxs-lookup"><span data-stu-id="a6264-219">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="a6264-220">Vous devrez peut-être décaler le processus de démarrage de plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="a6264-220">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="a6264-221">Échec de processus 502.5</span><span class="sxs-lookup"><span data-stu-id="a6264-221">502.5 Process Failure</span></span>

<span data-ttu-id="a6264-222">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a6264-222">The worker process fails.</span></span> <span data-ttu-id="a6264-223">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a6264-223">The app doesn't start.</span></span>

<span data-ttu-id="a6264-224">Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tente, en vain, de démarrer le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="a6264-224">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="a6264-225">La cause d’un échec de démarrage d’un processus peut généralement être déterminée à partir des entrées dans le journal des événements d’application et le journal stdout du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6264-225">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="a6264-226">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="a6264-226">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="a6264-227">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="a6264-227">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="a6264-228">Le *framework partagé* est le jeu d’assemblys (fichiers *.dll*) qui sont installés sur l’ordinateur et référencés par un métapaquet comme `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="a6264-228">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="a6264-229">La référence de métapaquet peut spécifier une version minimale requise.</span><span class="sxs-lookup"><span data-stu-id="a6264-229">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="a6264-230">Pour plus d’informations, consultez [Le framework partagé](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="a6264-230">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="a6264-231">La page d’erreur d’un *échec de processus 502.5* est retournée quand une erreur de configuration d’hébergement ou d’application entraîne l’échec du processus de travail :</span><span class="sxs-lookup"><span data-stu-id="a6264-231">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="a6264-232">Échec du démarrage de l’application (code d’erreur « 0x800700c1 »)</span><span class="sxs-lookup"><span data-stu-id="a6264-232">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="a6264-233">L’application n’a pas pu démarrer, car l’assembly de l’application ( *.dll*) n’a pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="a6264-233">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="a6264-234">Cette erreur se produit lorsqu’il existe une incompatibilité au niveau du nombre de bits entre l’application publiée et le processus w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="a6264-234">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="a6264-235">Vérifiez que le paramètre 32 bits du pool d’applications est correct :</span><span class="sxs-lookup"><span data-stu-id="a6264-235">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="a6264-236">Sélectionnez le pool d’applications parmi les **Pools d’applications** IIS Manager.</span><span class="sxs-lookup"><span data-stu-id="a6264-236">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="a6264-237">Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="a6264-237">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="a6264-238">Définissez **Activer les applications 32 bits** :</span><span class="sxs-lookup"><span data-stu-id="a6264-238">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="a6264-239">Si vous déployez une application 32 bits (x86), définissez la valeur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="a6264-239">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="a6264-240">Si vous déployez une application 64 bits (x64), définissez la valeur sur `False`.</span><span class="sxs-lookup"><span data-stu-id="a6264-240">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="a6264-241">Confirmez qu’il n’existe pas de conflit entre une `<Platform>` propriété MSBuild dans le fichier projet et le nombre de bits publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-241">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="a6264-242">Réinitialisation de la connexion</span><span class="sxs-lookup"><span data-stu-id="a6264-242">Connection reset</span></span>

<span data-ttu-id="a6264-243">Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**.</span><span class="sxs-lookup"><span data-stu-id="a6264-243">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="a6264-244">C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="a6264-244">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="a6264-245">Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client.</span><span class="sxs-lookup"><span data-stu-id="a6264-245">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="a6264-246">La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.</span><span class="sxs-lookup"><span data-stu-id="a6264-246">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="a6264-247">Limites de démarrage par défaut</span><span class="sxs-lookup"><span data-stu-id="a6264-247">Default startup limits</span></span>

<span data-ttu-id="a6264-248">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) est configuré avec un *startupTimeLimit* par défaut de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="a6264-248">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="a6264-249">Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus.</span><span class="sxs-lookup"><span data-stu-id="a6264-249">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="a6264-250">Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="a6264-250">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="a6264-251">Résoudre les problèmes sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a6264-251">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="a6264-252">Journal des événements d’application (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="a6264-252">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="a6264-253">Pour accéder au Journal des événements de l’application, utilisez le panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure :</span><span class="sxs-lookup"><span data-stu-id="a6264-253">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="a6264-254">Dans le Portail Azure, ouvrez l’application dans **App Services**.</span><span class="sxs-lookup"><span data-stu-id="a6264-254">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="a6264-255">Sélectionnez **Diagnostiquer et résoudre les problèmes**.</span><span class="sxs-lookup"><span data-stu-id="a6264-255">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="a6264-256">Sélectionnez le titre **Outils de diagnostic**.</span><span class="sxs-lookup"><span data-stu-id="a6264-256">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="a6264-257">Sous **Outils de support**, sélectionnez le bouton **Événements d’application**.</span><span class="sxs-lookup"><span data-stu-id="a6264-257">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="a6264-258">Examinez la dernière erreur fournie par l’entrée *IIS AspNetCoreModule* ou *IIS AspNetCoreModule V2* dans la colonne **Source**.</span><span class="sxs-lookup"><span data-stu-id="a6264-258">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="a6264-259">En dehors de la solution consistant à utiliser le panneau **Diagnostiquer et résoudre les problèmes**, vous pouvez examiner directement le fichier Journal des événements de l’application avec [Kudu](https://github.com/projectkudu/kudu/wiki) :</span><span class="sxs-lookup"><span data-stu-id="a6264-259">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="a6264-260">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="a6264-260">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="a6264-261">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="a6264-261">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="a6264-262">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a6264-262">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="a6264-263">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="a6264-263">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="a6264-264">Ouvrez le dossier **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="a6264-264">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="a6264-265">Sélectionnez l’icône en forme de crayon à côté du fichier *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="a6264-265">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="a6264-266">Examinez le journal.</span><span class="sxs-lookup"><span data-stu-id="a6264-266">Examine the log.</span></span> <span data-ttu-id="a6264-267">Allez en bas du journal pour voir les événements les plus récents.</span><span class="sxs-lookup"><span data-stu-id="a6264-267">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="a6264-268">Exécuter l’application dans la console Kudu</span><span class="sxs-lookup"><span data-stu-id="a6264-268">Run the app in the Kudu console</span></span>

<span data-ttu-id="a6264-269">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-269">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="a6264-270">Vous pouvez exécuter l’application dans la console d’exécution à distance [Kudu](https://github.com/projectkudu/kudu/wiki) pour détecter l’erreur :</span><span class="sxs-lookup"><span data-stu-id="a6264-270">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="a6264-271">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="a6264-271">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="a6264-272">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="a6264-272">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="a6264-273">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a6264-273">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="a6264-274">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="a6264-274">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="a6264-275">Tester une application 32 bits (x86)</span><span class="sxs-lookup"><span data-stu-id="a6264-275">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="a6264-276">**Version actuelle**</span><span class="sxs-lookup"><span data-stu-id="a6264-276">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="a6264-277">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="a6264-277">Run the app:</span></span>
   * <span data-ttu-id="a6264-278">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="a6264-278">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="a6264-279">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="a6264-279">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="a6264-280">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="a6264-280">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="a6264-281">**Déploiement dépendant du Framework s’exécutant sur une version préliminaire**</span><span class="sxs-lookup"><span data-stu-id="a6264-281">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="a6264-282">*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x86) requise.*</span><span class="sxs-lookup"><span data-stu-id="a6264-282">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="a6264-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` est la version du runtime).</span><span class="sxs-lookup"><span data-stu-id="a6264-283">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="a6264-284">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="a6264-284">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="a6264-285">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="a6264-285">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="a6264-286">Tester une application 64 bits (x64)</span><span class="sxs-lookup"><span data-stu-id="a6264-286">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="a6264-287">**Version actuelle**</span><span class="sxs-lookup"><span data-stu-id="a6264-287">**Current release**</span></span>

* <span data-ttu-id="a6264-288">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) 64 bits (x64) :</span><span class="sxs-lookup"><span data-stu-id="a6264-288">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="a6264-289">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="a6264-289">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="a6264-290">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="a6264-290">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="a6264-291">Exécutez l'application : `{ASSEMBLY NAME}.exe`.</span><span class="sxs-lookup"><span data-stu-id="a6264-291">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="a6264-292">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="a6264-292">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="a6264-293">**Déploiement dépendant du Framework s’exécutant sur une version préliminaire**</span><span class="sxs-lookup"><span data-stu-id="a6264-293">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="a6264-294">*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x64) requise.*</span><span class="sxs-lookup"><span data-stu-id="a6264-294">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="a6264-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` est la version du runtime).</span><span class="sxs-lookup"><span data-stu-id="a6264-295">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="a6264-296">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="a6264-296">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="a6264-297">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="a6264-297">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="a6264-298">Journal stdout du module ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="a6264-298">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="a6264-299">Le journal stdout du module ASP.NET Core enregistre souvent des messages d’erreur utiles et absents du Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-299">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="a6264-300">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="a6264-300">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="a6264-301">Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="a6264-301">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="a6264-302">Sous **SÉLECTIONNER UNE CATÉGORIE DE PROBLÈME**, sélectionnez le bouton **Application web en panne**.</span><span class="sxs-lookup"><span data-stu-id="a6264-302">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="a6264-303">Sous **Solutions suggérées** > **Activer la redirection de journaux stdout**, sélectionnez le bouton **Ouvrir la console Kudu pour modifier web.config**.</span><span class="sxs-lookup"><span data-stu-id="a6264-303">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="a6264-304">Dans la **Console de diagnostic** Kudu, ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="a6264-304">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="a6264-305">Faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.</span><span class="sxs-lookup"><span data-stu-id="a6264-305">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="a6264-306">Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a6264-306">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="a6264-307">Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="a6264-307">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="a6264-308">Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="a6264-308">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="a6264-309">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-309">Make a request to the app.</span></span>
1. <span data-ttu-id="a6264-310">Revenez sur le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="a6264-310">Return to the Azure portal.</span></span> <span data-ttu-id="a6264-311">Sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="a6264-311">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="a6264-312">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="a6264-312">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="a6264-313">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a6264-313">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="a6264-314">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="a6264-314">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="a6264-315">Sélectionnez le dossier **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="a6264-315">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="a6264-316">Inspectez la colonne **Modifié** et sélectionnez l’icône en forme de crayon pour modifier le journal stdout avec la date de dernière modification.</span><span class="sxs-lookup"><span data-stu-id="a6264-316">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="a6264-317">Lorsque le fichier journal s’ouvre, l’erreur s’affiche.</span><span class="sxs-lookup"><span data-stu-id="a6264-317">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="a6264-318">Désactivez la journalisation stdout, une fois les problèmes résolus :</span><span class="sxs-lookup"><span data-stu-id="a6264-318">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="a6264-319">Dans la **Console de diagnostic** Kudu, revenez au chemin d’accès **site** > **wwwroot** pour faire apparaître le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a6264-319">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="a6264-320">Ouvrez à nouveau le fichier **web.config** en sélectionnant l’icône en forme de crayon.</span><span class="sxs-lookup"><span data-stu-id="a6264-320">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="a6264-321">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="a6264-321">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="a6264-322">Sélectionnez **Enregistrer** pour enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="a6264-322">Select **Save** to save the file.</span></span>

<span data-ttu-id="a6264-323">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="a6264-323">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="a6264-324">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="a6264-324">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="a6264-325">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="a6264-325">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="a6264-326">N’utilisez la journalisation stdout que pour résoudre les problèmes de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-326">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="a6264-327">Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="a6264-327">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a6264-328">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a6264-328">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="a6264-329">Journal de débogage du module ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="a6264-329">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="a6264-330">Le journal de débogage du module ASP.NET Core fournit une journalisation supplémentaire, plus approfondie, à partir du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6264-330">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="a6264-331">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="a6264-331">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="a6264-332">Pour activer le journal de diagnostic amélioré, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a6264-332">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="a6264-333">Suivez les instructions indiquées dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) afin de configurer l’application pour une journalisation des diagnostics améliorée.</span><span class="sxs-lookup"><span data-stu-id="a6264-333">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="a6264-334">Redéployez l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-334">Redeploy the app.</span></span>
   * <span data-ttu-id="a6264-335">Ajoutez le `<handlerSettings>` présenté dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) au fichier *web.config* de l’application en production à l’aide de la console Kudu :</span><span class="sxs-lookup"><span data-stu-id="a6264-335">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="a6264-336">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="a6264-336">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="a6264-337">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="a6264-337">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="a6264-338">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a6264-338">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="a6264-339">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="a6264-339">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="a6264-340">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="a6264-340">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="a6264-341">Modifiez le fichier *web.config* en sélectionnant le bouton représentant un crayon.</span><span class="sxs-lookup"><span data-stu-id="a6264-341">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="a6264-342">Ajoutez la section `<handlerSettings>` comme indiqué dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="a6264-342">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="a6264-343">Sélectionnez le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="a6264-343">Select the **Save** button.</span></span>
1. <span data-ttu-id="a6264-344">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="a6264-344">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="a6264-345">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="a6264-345">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="a6264-346">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a6264-346">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="a6264-347">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="a6264-347">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="a6264-348">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="a6264-348">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="a6264-349">Si vous n’avez pas indiqué de chemin pour le fichier *aspnetcore-debug.log*, le fichier apparaît dans la liste.</span><span class="sxs-lookup"><span data-stu-id="a6264-349">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="a6264-350">Si vous avez indiqué un chemin, accédez à l’emplacement du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="a6264-350">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="a6264-351">Ouvrez le fichier journal à l’aide du bouton représentant un crayon à côté du nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="a6264-351">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="a6264-352">Désactivez la journalisation du débogage, une fois la résolution des problèmes effectuée :</span><span class="sxs-lookup"><span data-stu-id="a6264-352">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="a6264-353">Pour désactiver la journalisation de débogage améliorée, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="a6264-353">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="a6264-354">Supprimez `<handlerSettings>` du fichier *web.config* localement, puis redéployez l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-354">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="a6264-355">Utilisez la console Kudu pour modifier le fichier *web.config* et supprimer la section `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="a6264-355">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="a6264-356">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="a6264-356">Save the file.</span></span>

<span data-ttu-id="a6264-357">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="a6264-357">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="a6264-358">Si le journal de débogage n’est pas désactivé, cela peut entraîner une défaillance de l’application ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="a6264-358">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="a6264-359">Il n’existe aucune limite à la taille du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="a6264-359">There's no limit on log file size.</span></span> <span data-ttu-id="a6264-360">Utilisez uniquement la journalisation de débogage pour résoudre les problèmes de démarrage d’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-360">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="a6264-361">Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="a6264-361">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a6264-362">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a6264-362">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="a6264-363">Application lente ou suspendue (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="a6264-363">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="a6264-364">Si une application répond lentement ou se bloque sur une requête, voir les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="a6264-364">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="a6264-365">Résoudre les problèmes de performances d’une application web lente dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a6264-365">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="a6264-366">Utiliser l’extension de site Outil de diagnostic des incidents pour capturer une image mémoire des problèmes d’exception intermittente ou de performances sur Azure Web App</span><span class="sxs-lookup"><span data-stu-id="a6264-366">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="a6264-367">Panneaux Monitoring</span><span class="sxs-lookup"><span data-stu-id="a6264-367">Monitoring blades</span></span>

<span data-ttu-id="a6264-368">Les panneaux Monitoring offrent une expérience de résolution des erreurs différente des méthodes décrites précédemment dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="a6264-368">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="a6264-369">Ils peuvent servir à diagnostiquer les erreurs de la série 500.</span><span class="sxs-lookup"><span data-stu-id="a6264-369">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="a6264-370">Vérifiez que les extensions ASP.NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="a6264-370">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="a6264-371">Si ce n’est pas le cas, installez-les manuellement :</span><span class="sxs-lookup"><span data-stu-id="a6264-371">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="a6264-372">Dans la section du panneau **OUTILS DE DÉVELOPPEMENT**, sélectionnez le panneau **Extensions**.</span><span class="sxs-lookup"><span data-stu-id="a6264-372">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="a6264-373">Les **Extensions ASP.NET Core** devraient apparaître dans la liste.</span><span class="sxs-lookup"><span data-stu-id="a6264-373">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="a6264-374">Si les extensions ne sont pas installées, sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="a6264-374">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="a6264-375">Choisissez les **Extensions ASP.NET Core** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="a6264-375">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="a6264-376">Sélectionnez **OK** pour accepter les conditions légales.</span><span class="sxs-lookup"><span data-stu-id="a6264-376">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="a6264-377">Sélectionnez **OK** sur le panneau **Ajouter une extension**.</span><span class="sxs-lookup"><span data-stu-id="a6264-377">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="a6264-378">Un message contextuel d’information indique que les extensions sont bien installées.</span><span class="sxs-lookup"><span data-stu-id="a6264-378">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="a6264-379">Si la journalisation stdout n’est pas activée, suivez les étapes ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="a6264-379">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="a6264-380">Sur le Portail Azure, sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="a6264-380">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="a6264-381">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="a6264-381">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="a6264-382">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a6264-382">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="a6264-383">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="a6264-383">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="a6264-384">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot** et faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.</span><span class="sxs-lookup"><span data-stu-id="a6264-384">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="a6264-385">Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a6264-385">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="a6264-386">Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="a6264-386">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="a6264-387">Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="a6264-387">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="a6264-388">Maintenant, activez la journalisation des diagnostics :</span><span class="sxs-lookup"><span data-stu-id="a6264-388">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="a6264-389">Sur le Portail Azure, sélectionnez le panneau **Journaux de diagnostic**.</span><span class="sxs-lookup"><span data-stu-id="a6264-389">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="a6264-390">Basculez **Journalisation des applications (système de fichiers)** et **Messages d’erreur détaillés** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="a6264-390">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="a6264-391">Sélectionnez le bouton **Enregistrer** en haut du panneau.</span><span class="sxs-lookup"><span data-stu-id="a6264-391">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="a6264-392">Pour inclure le suivi des demandes ayant échoué, également appelé Mise en mémoire tampon des événements d’échec des demandes (FREB), basculez **Suivi des demandes ayant échoué** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="a6264-392">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="a6264-393">Sélectionnez le panneau **Flux du journal** situé juste sous le panneau **Journaux de diagnostic** sur le portail.</span><span class="sxs-lookup"><span data-stu-id="a6264-393">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="a6264-394">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-394">Make a request to the app.</span></span>
1. <span data-ttu-id="a6264-395">La cause de l’erreur est indiquée dans les données de flux de journal.</span><span class="sxs-lookup"><span data-stu-id="a6264-395">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="a6264-396">Veillez à désactiver la journalisation stdout une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="a6264-396">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="a6264-397">Pour afficher les journaux de suivi des demandes ayant échoué (journaux FREB) :</span><span class="sxs-lookup"><span data-stu-id="a6264-397">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="a6264-398">Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="a6264-398">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="a6264-399">Sélectionnez **Journaux de suivi des demandes ayant échoué** dans la zone **OUTILS DE PRISE EN CHARGE** de la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="a6264-399">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="a6264-400">Voir la [section Traces des demandes ayant échoué de la rubrique Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) et [FAQ sur les performances des applications web dans Azure : comment activer le suivi des demandes ayant échoué ?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="a6264-400">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="a6264-401">Pour plus d’informations, voir [Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="a6264-401">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="a6264-402">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="a6264-402">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="a6264-403">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="a6264-403">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="a6264-404">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="a6264-404">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a6264-405">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a6264-405">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="a6264-406">Résoudre les problèmes sur IIS</span><span class="sxs-lookup"><span data-stu-id="a6264-406">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="a6264-407">Journal des événements d’application (IIS)</span><span class="sxs-lookup"><span data-stu-id="a6264-407">Application Event Log (IIS)</span></span>

<span data-ttu-id="a6264-408">Accédez au Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="a6264-408">Access the Application Event Log:</span></span>

1. <span data-ttu-id="a6264-409">Ouvrez le menu Démarrer, recherchez *Observateur d’événements*, puis sélectionnez l’application **Observateur d’événements** .</span><span class="sxs-lookup"><span data-stu-id="a6264-409">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="a6264-410">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="a6264-410">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="a6264-411">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-411">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="a6264-412">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="a6264-412">Search for errors associated with the failing app.</span></span> <span data-ttu-id="a6264-413">Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.</span><span class="sxs-lookup"><span data-stu-id="a6264-413">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="a6264-414">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="a6264-414">Run the app at a command prompt</span></span>

<span data-ttu-id="a6264-415">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-415">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="a6264-416">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="a6264-416">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="a6264-417">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="a6264-417">Framework-dependent deployment</span></span>

<span data-ttu-id="a6264-418">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="a6264-418">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="a6264-419">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="a6264-419">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="a6264-420">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="a6264-420">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="a6264-421">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="a6264-421">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="a6264-422">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="a6264-422">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="a6264-423">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="a6264-423">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="a6264-424">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-424">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="a6264-425">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="a6264-425">Self-contained deployment</span></span>

<span data-ttu-id="a6264-426">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="a6264-426">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="a6264-427">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-427">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="a6264-428">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="a6264-428">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="a6264-429">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="a6264-429">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="a6264-430">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="a6264-430">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="a6264-431">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="a6264-431">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="a6264-432">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-432">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="a6264-433">Journal stdout du module ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="a6264-433">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="a6264-434">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="a6264-434">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="a6264-435">Accédez au dossier de déploiement du site sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="a6264-435">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="a6264-436">Si le dossier *logs* n’est pas présent, créez-le.</span><span class="sxs-lookup"><span data-stu-id="a6264-436">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="a6264-437">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="a6264-437">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="a6264-438">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a6264-438">Edit the *web.config* file.</span></span> <span data-ttu-id="a6264-439">Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="a6264-439">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="a6264-440">Dans le chemin, `stdout` est le préfixe du nom du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="a6264-440">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="a6264-441">Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé.</span><span class="sxs-lookup"><span data-stu-id="a6264-441">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="a6264-442">Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="a6264-442">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="a6264-443">Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="a6264-443">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="a6264-444">Enregistrez le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="a6264-444">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="a6264-445">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-445">Make a request to the app.</span></span>
1. <span data-ttu-id="a6264-446">Accédez au dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="a6264-446">Navigate to the *logs* folder.</span></span> <span data-ttu-id="a6264-447">Recherchez et ouvrez le journal stdout le plus récent.</span><span class="sxs-lookup"><span data-stu-id="a6264-447">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="a6264-448">Déterminez si le journal contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="a6264-448">Study the log for errors.</span></span>

<span data-ttu-id="a6264-449">Désactivez la journalisation stdout, une fois les problèmes résolus :</span><span class="sxs-lookup"><span data-stu-id="a6264-449">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="a6264-450">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a6264-450">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="a6264-451">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="a6264-451">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="a6264-452">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="a6264-452">Save the file.</span></span>

<span data-ttu-id="a6264-453">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="a6264-453">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="a6264-454">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="a6264-454">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="a6264-455">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="a6264-455">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="a6264-456">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="a6264-456">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a6264-457">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a6264-457">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="a6264-458">Journal de débogage du module ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="a6264-458">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="a6264-459">Ajoutez les paramètres de gestionnaire suivants au fichier *Web. config* de l’application pour activer ASP.net Core journal de débogage du module :</span><span class="sxs-lookup"><span data-stu-id="a6264-459">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="a6264-460">Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="a6264-460">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="a6264-461">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="a6264-461">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="a6264-462">Afficher la page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="a6264-462">Enable the Developer Exception Page</span></span>

<span data-ttu-id="a6264-463">La [variable d’environnement `ASPNETCORE_ENVIRONMENT` peut être ajoutée à Web. config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="a6264-463">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="a6264-464">Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="a6264-464">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="a6264-465">La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="a6264-465">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="a6264-466">Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="a6264-466">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="a6264-467">Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="a6264-467">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="a6264-468">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="a6264-468">Obtain data from an app</span></span>

<span data-ttu-id="a6264-469">Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal.</span><span class="sxs-lookup"><span data-stu-id="a6264-469">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="a6264-470">Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="a6264-470">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="a6264-471">Application lente ou bloquée (IIS)</span><span class="sxs-lookup"><span data-stu-id="a6264-471">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="a6264-472">Un *vidage sur incident* est un instantané de la mémoire du système et peut aider à déterminer la cause d’un incident d’application, d’un échec de démarrage ou d’une application lente.</span><span class="sxs-lookup"><span data-stu-id="a6264-472">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="a6264-473">L’application cesse de fonctionner ou rencontre une exception</span><span class="sxs-lookup"><span data-stu-id="a6264-473">App crashes or encounters an exception</span></span>

<span data-ttu-id="a6264-474">Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :</span><span class="sxs-lookup"><span data-stu-id="a6264-474">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="a6264-475">Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="a6264-475">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="a6264-476">Le pool d’application doit disposer des accès d’écriture dans le dossier.</span><span class="sxs-lookup"><span data-stu-id="a6264-476">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="a6264-477">Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="a6264-477">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="a6264-478">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="a6264-478">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="a6264-479">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="a6264-479">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="a6264-480">Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.</span><span class="sxs-lookup"><span data-stu-id="a6264-480">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="a6264-481">Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="a6264-481">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="a6264-482">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="a6264-482">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="a6264-483">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="a6264-483">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="a6264-484">Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement.</span><span class="sxs-lookup"><span data-stu-id="a6264-484">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="a6264-485">Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.</span><span class="sxs-lookup"><span data-stu-id="a6264-485">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="a6264-486">Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).</span><span class="sxs-lookup"><span data-stu-id="a6264-486">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="a6264-487">L’application se bloque, ne démarre pas ou s’exécute normalement</span><span class="sxs-lookup"><span data-stu-id="a6264-487">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="a6264-488">Quand une application *se bloque* (cesse de répondre mais ne se bloque pas), échoue pendant le démarrage ou s’exécute normalement, consultez [fichiers de vidage en mode utilisateur : choix de l’outil le mieux](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) adapté pour sélectionner un outil approprié pour produire le vidage.</span><span class="sxs-lookup"><span data-stu-id="a6264-488">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="a6264-489">Analyser le fichier dump</span><span class="sxs-lookup"><span data-stu-id="a6264-489">Analyze the dump</span></span>

<span data-ttu-id="a6264-490">Un fichier dump peut être analysé à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="a6264-490">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="a6264-491">Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).</span><span class="sxs-lookup"><span data-stu-id="a6264-491">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="a6264-492">Effacer les caches de package</span><span class="sxs-lookup"><span data-stu-id="a6264-492">Clear package caches</span></span>

<span data-ttu-id="a6264-493">Une application fonctionnelle peut échouer immédiatement après la mise à niveau de la kit SDK .NET Core sur l’ordinateur de développement ou la modification des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-493">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="a6264-494">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="a6264-494">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="a6264-495">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="a6264-495">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="a6264-496">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="a6264-496">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="a6264-497">Effacez les caches de package en exécutant [dotnet NuGet LOCALS tout--Clear](/dotnet/core/tools/dotnet-nuget-locals) dans une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="a6264-497">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="a6264-498">L’effacement des caches de package peut également être effectué à l’aide de l’outil [NuGet. exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="a6264-498">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="a6264-499">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="a6264-499">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="a6264-500">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="a6264-500">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="a6264-501">Supprimez tous les fichiers du dossier de déploiement sur le serveur avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="a6264-501">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6264-502">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a6264-502">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="a6264-503">Documentation Azure</span><span class="sxs-lookup"><span data-stu-id="a6264-503">Azure documentation</span></span>

* [<span data-ttu-id="a6264-504">Application Insights pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6264-504">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="a6264-505">Section débogage à distance des applications Web de dépanner une application Web dans Azure App Service à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6264-505">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="a6264-506">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a6264-506">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="a6264-507">Guide pratique pour surveiller des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a6264-507">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="a6264-508">Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6264-508">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="a6264-509">Résoudre les erreurs HTTP « 502 Passerelle incorrecte » et « 503 Service non disponible » dans des applications web Azure</span><span class="sxs-lookup"><span data-stu-id="a6264-509">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="a6264-510">Résoudre les problèmes de performances d’une application web lente dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="a6264-510">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="a6264-511">FAQ sur les performances des applications web dans Azure</span><span class="sxs-lookup"><span data-stu-id="a6264-511">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="a6264-512">Bac à sable des applications web Azure (limitations de l’exécution du runtime App Service)</span><span class="sxs-lookup"><span data-stu-id="a6264-512">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="a6264-513">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span><span class="sxs-lookup"><span data-stu-id="a6264-513">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="a6264-514">Documentation de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6264-514">Visual Studio documentation</span></span>

* [<span data-ttu-id="a6264-515">Débogage à distance ASP.NET Core sur IIS dans Azure dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a6264-515">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="a6264-516">Débogage à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a6264-516">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="a6264-517">Apprendre à déboguer avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6264-517">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="a6264-518">Documentation Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6264-518">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="a6264-519">Débogage avec Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6264-519">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
