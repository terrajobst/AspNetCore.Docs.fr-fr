---
title: Résoudre les problèmes de ASP.NET Core sur Azure App Service et IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Azure App Service et Internet Information Services (IIS) des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/20/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: b0f5d44f153a095a6108a12ee91f4cc46fe0a0de
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829008"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="4fa76-103">Résoudre les problèmes de ASP.NET Core sur Azure App Service et IIS</span><span class="sxs-lookup"><span data-stu-id="4fa76-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="4fa76-104">Par [Luke Latham](https://github.com/guardrex) et [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="4fa76-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="4fa76-105">Cet article fournit des informations sur les erreurs de démarrage d’application courantes et des instructions sur la façon de diagnostiquer les erreurs quand une application est déployée sur Azure App Service ou IIS :</span><span class="sxs-lookup"><span data-stu-id="4fa76-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="4fa76-106">Erreurs de démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="4fa76-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="4fa76-107">Explique les scénarios courants de code d’état HTTP de démarrage.</span><span class="sxs-lookup"><span data-stu-id="4fa76-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="4fa76-108">Résoudre les problèmes sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa76-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="4fa76-109">Fournit des conseils de dépannage pour les applications déployées sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4fa76-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="4fa76-110">Résoudre les problèmes sur IIS</span><span class="sxs-lookup"><span data-stu-id="4fa76-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="4fa76-111">Fournit des conseils de dépannage pour les applications déployées sur IIS ou exécutées localement sur IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4fa76-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="4fa76-112">Ce guide s’applique aux déploiements Windows Server et Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="4fa76-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="4fa76-113">Effacer les caches de package</span><span class="sxs-lookup"><span data-stu-id="4fa76-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="4fa76-114">Explique ce qu’il faut faire quand des packages incohérents interrompent une application lors des mises à niveau majeures ou de la modification des versions de package.</span><span class="sxs-lookup"><span data-stu-id="4fa76-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="4fa76-115">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4fa76-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="4fa76-116">Répertorie des rubriques supplémentaires sur la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="4fa76-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="4fa76-117">Erreurs de démarrage des applications</span><span class="sxs-lookup"><span data-stu-id="4fa76-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4fa76-118">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="4fa76-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="4fa76-119">Un *échec de processus 502,5* ou un *échec de démarrage de 500,30* qui se produit lorsque le débogage local peut être diagnostiqué à l’aide des conseils de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="4fa76-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="4fa76-120">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="4fa76-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="4fa76-121">Un *échec de processus 502,5* qui se produit lorsque le débogage local peut être diagnostiqué à l’aide des conseils de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="4fa76-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="40314-forbidden"></a><span data-ttu-id="4fa76-122">403,14 interdit</span><span class="sxs-lookup"><span data-stu-id="4fa76-122">403.14 Forbidden</span></span>

<span data-ttu-id="4fa76-123">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-123">The app fails to start.</span></span> <span data-ttu-id="4fa76-124">L’erreur suivante est enregistrée :</span><span class="sxs-lookup"><span data-stu-id="4fa76-124">The following error is logged:</span></span>

```
The Web server is configured to not list the contents of this directory.
```

<span data-ttu-id="4fa76-125">L’erreur est généralement causée par un déploiement rompu sur le système d’hébergement, qui comprend l’un des scénarios suivants :</span><span class="sxs-lookup"><span data-stu-id="4fa76-125">The error is usually caused by a broken deployment on the hosting system, which includes any of the following scenarios:</span></span>

* <span data-ttu-id="4fa76-126">L’application est déployée dans le mauvais dossier sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4fa76-126">The app is deployed to the wrong folder on the hosting system.</span></span>
* <span data-ttu-id="4fa76-127">Le processus de déploiement n’a pas réussi à déplacer tous les fichiers et dossiers de l’application vers le dossier de déploiement sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4fa76-127">The deployment process failed to move all of the app's files and folders to the deployment folder on the hosting system.</span></span>
* <span data-ttu-id="4fa76-128">Le fichier *Web. config* est manquant dans le déploiement ou le contenu du fichier *Web. config* est incorrect.</span><span class="sxs-lookup"><span data-stu-id="4fa76-128">The *web.config* file is missing from the deployment, or the *web.config* file contents are malformed.</span></span>

<span data-ttu-id="4fa76-129">Effectuez les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fa76-129">Perform the following steps:</span></span>

1. <span data-ttu-id="4fa76-130">Supprimez tous les fichiers et dossiers du dossier de déploiement sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4fa76-130">Delete all of the files and folders from the deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="4fa76-131">Redéployez le contenu du dossier de *publication* de l’application sur le système d’hébergement à l’aide de votre méthode de déploiement normale, telle que Visual Studio, PowerShell ou le déploiement manuel :</span><span class="sxs-lookup"><span data-stu-id="4fa76-131">Redeploy the contents of the app's *publish* folder to the hosting system using your normal method of deployment, such as Visual Studio, PowerShell, or manual deployment:</span></span>
   * <span data-ttu-id="4fa76-132">Vérifiez que le fichier *Web. config* est présent dans le déploiement et que son contenu est correct.</span><span class="sxs-lookup"><span data-stu-id="4fa76-132">Confirm that the *web.config* file is present in the deployment and that its contents are correct.</span></span>
   * <span data-ttu-id="4fa76-133">Lors de l’hébergement sur Azure App Service, vérifiez que l’application est déployée dans le dossier `D:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-133">When hosting on Azure App Service, confirm that the app is deployed to the `D:\home\site\wwwroot` folder.</span></span>
   * <span data-ttu-id="4fa76-134">Lorsque l’application est hébergée par IIS, vérifiez que l’application est déployée sur le **chemin d’accès physique** IIS indiqué dans les **paramètres de base**du gestionnaire des **services Internet**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-134">When the app is hosted by IIS, confirm that the app is deployed to the IIS **Physical path** shown in **IIS Manager**'s **Basic Settings**.</span></span>
1. <span data-ttu-id="4fa76-135">Confirmez que tous les fichiers et dossiers de l’application sont déployés en comparant le déploiement sur le système d’hébergement au contenu du dossier de *publication* du projet.</span><span class="sxs-lookup"><span data-stu-id="4fa76-135">Confirm that all of the app's files and folders are deployed by comparing the deployment on the hosting system to the contents of the project's *publish* folder.</span></span>

<span data-ttu-id="4fa76-136">Pour plus d’informations sur la disposition d’une application ASP.NET Core publiée, consultez <xref:host-and-deploy/directory-structure>.</span><span class="sxs-lookup"><span data-stu-id="4fa76-136">For more information on the layout of a published ASP.NET Core app, see <xref:host-and-deploy/directory-structure>.</span></span> <span data-ttu-id="4fa76-137">Pour plus d’informations sur le fichier *Web. config* , consultez <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="4fa76-137">For more information on the *web.config* file, see <xref:host-and-deploy/aspnet-core-module#configuration-with-webconfig>.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="4fa76-138">Erreur de serveur interne 500</span><span class="sxs-lookup"><span data-stu-id="4fa76-138">500 Internal Server Error</span></span>

<span data-ttu-id="4fa76-139">L’application démarre, mais une erreur empêche le serveur de répondre à la requête.</span><span class="sxs-lookup"><span data-stu-id="4fa76-139">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="4fa76-140">Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="4fa76-140">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="4fa76-141">La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-141">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="4fa76-142">Le Journal des événements de l’application indique généralement qu’elle a démarré normalement.</span><span class="sxs-lookup"><span data-stu-id="4fa76-142">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="4fa76-143">Du point de vue du serveur, c’est exact.</span><span class="sxs-lookup"><span data-stu-id="4fa76-143">From the server's perspective, that's correct.</span></span> <span data-ttu-id="4fa76-144">L’application a démarré, mais elle ne peut pas générer de réponse valide.</span><span class="sxs-lookup"><span data-stu-id="4fa76-144">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="4fa76-145">Exécutez l’application à partir d’une invite de commandes sur le serveur ou activez le journal stdout du module ASP.NET Core pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="4fa76-145">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="4fa76-146">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="4fa76-146">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="4fa76-147">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-147">The worker process fails.</span></span> <span data-ttu-id="4fa76-148">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-148">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-149">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) ne parvient pas à trouver le CLR .net Core et à rechercher le gestionnaire de demandes in-process (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="4fa76-149">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="4fa76-150">Vérifiez les points suivants :</span><span class="sxs-lookup"><span data-stu-id="4fa76-150">Check that:</span></span>

* <span data-ttu-id="4fa76-151">l’application cible le package NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) ou le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ;</span><span class="sxs-lookup"><span data-stu-id="4fa76-151">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="4fa76-152">la version du framework partagé ASP.NET Core que l’application cible est installée sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="4fa76-152">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="4fa76-153">500.0 - Échec de chargement du gestionnaire out-of-process</span><span class="sxs-lookup"><span data-stu-id="4fa76-153">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="4fa76-154">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-154">The worker process fails.</span></span> <span data-ttu-id="4fa76-155">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-155">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-156">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) ne parvient pas à trouver le gestionnaire de demandes d’hébergement hors processus.</span><span class="sxs-lookup"><span data-stu-id="4fa76-156">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="4fa76-157">Vérifiez que le fichier *aspnetcorev2_outofprocess.dll* est présent dans un sous-dossier en regard de *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-157">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="4fa76-158">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="4fa76-158">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="4fa76-159">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-159">The worker process fails.</span></span> <span data-ttu-id="4fa76-160">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-160">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-161">Une erreur inconnue s’est produite lors du chargement des composants du [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="4fa76-161">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="4fa76-162">Effectuez l’une des actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fa76-162">Take one of the following actions:</span></span>

* <span data-ttu-id="4fa76-163">Contactez le [Support Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) (sélectionnez **Outils de développement**, puis **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="4fa76-163">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="4fa76-164">Posez une question sur Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="4fa76-164">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="4fa76-165">Signalez un problème sur notre [dépôt GitHub](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="4fa76-165">File an issue on our [GitHub repository](https://github.com/dotnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="4fa76-166">500.30 - Échec du démarrage in-process</span><span class="sxs-lookup"><span data-stu-id="4fa76-166">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="4fa76-167">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-167">The worker process fails.</span></span> <span data-ttu-id="4fa76-168">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-168">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-169">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) tente de démarrer le CLR .net Core in-process, mais il ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="4fa76-169">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="4fa76-170">La cause d’un échec de démarrage d’un processus peut généralement être déterminée à partir des entrées dans le journal des événements d’application et le journal stdout du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4fa76-170">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="4fa76-171">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="4fa76-171">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="4fa76-172">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="4fa76-172">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="4fa76-173">500.31 - Échec de la recherche de dépendances natives par ANCM</span><span class="sxs-lookup"><span data-stu-id="4fa76-173">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="4fa76-174">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-174">The worker process fails.</span></span> <span data-ttu-id="4fa76-175">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-175">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-176">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) tente de démarrer le Runtime .net Core in-process, mais il ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="4fa76-176">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="4fa76-177">La cause la plus courante de cet échec de démarrage est liée à la non-installation du runtime `Microsoft.NETCore.App` ou `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-177">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="4fa76-178">Si l’application est déployée pour cibler ASP.NET Core 3.0, et si cette version n’existe pas sur la machine, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="4fa76-178">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="4fa76-179">Voici un exemple de message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="4fa76-179">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="4fa76-180">Le message d’erreur liste toutes les versions installées de .NET Core ainsi que la version demandée par l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-180">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="4fa76-181">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="4fa76-181">To fix this error, either:</span></span>

* <span data-ttu-id="4fa76-182">Installez la version appropriée de .NET Core sur la machine.</span><span class="sxs-lookup"><span data-stu-id="4fa76-182">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="4fa76-183">Changez l’application pour cibler une version de .NET Core présente sur la machine.</span><span class="sxs-lookup"><span data-stu-id="4fa76-183">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="4fa76-184">Publiez l’application sous forme de [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="4fa76-184">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="4fa76-185">Durant l’exécution en phase de développement (la variable d’environnement `ASPNETCORE_ENVIRONMENT` a la valeur `Development`), l’erreur spécifique est écrite dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="4fa76-185">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="4fa76-186">La cause de l’échec du démarrage d’un processus se trouve également dans le journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-186">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="4fa76-187">500.32 - Échec du chargement de la dll par ANCM</span><span class="sxs-lookup"><span data-stu-id="4fa76-187">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="4fa76-188">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-188">The worker process fails.</span></span> <span data-ttu-id="4fa76-189">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-189">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-190">Le plus souvent, cette erreur provient du fait que l’application est publiée pour une architecture de processeur incompatible.</span><span class="sxs-lookup"><span data-stu-id="4fa76-190">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="4fa76-191">Si le processus de travail s’exécute en tant qu’application 32 bits et si l’application a été publiée pour cibler une architecture 64 bits, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="4fa76-191">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="4fa76-192">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="4fa76-192">To fix this error, either:</span></span>

* <span data-ttu-id="4fa76-193">Republiez l’application pour la même architecture de processeur que celle du processus de travail.</span><span class="sxs-lookup"><span data-stu-id="4fa76-193">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="4fa76-194">Publiez l’application sous forme de [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="4fa76-194">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="4fa76-195">500.33 - Échec du chargement du gestionnaire de requêtes ANCM</span><span class="sxs-lookup"><span data-stu-id="4fa76-195">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="4fa76-196">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-196">The worker process fails.</span></span> <span data-ttu-id="4fa76-197">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-197">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-198">L’application n’a pas référencé le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-198">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="4fa76-199">Seules les applications ciblant l’infrastructure `Microsoft.AspNetCore.App` peuvent être hébergées par le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4fa76-199">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="4fa76-200">Pour corriger cette erreur, vérifiez que l’application cible le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-200">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="4fa76-201">Examinez le fichier `.runtimeconfig.json` pour vérifier le framework ciblé par l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-201">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="4fa76-202">500.34 - Modèles d’hébergement mixtes ANCM non pris en charge</span><span class="sxs-lookup"><span data-stu-id="4fa76-202">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="4fa76-203">Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="4fa76-203">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="4fa76-204">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="4fa76-204">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="4fa76-205">500.35 - Applications in-process multiples dans le même processus ANCM</span><span class="sxs-lookup"><span data-stu-id="4fa76-205">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="4fa76-206">Le processus de travail ne peut pas exécuter plusieurs applications in-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="4fa76-206">The worker process can't run multiple in-process apps in the same process.</span></span>

<span data-ttu-id="4fa76-207">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="4fa76-207">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="4fa76-208">500.36 - Échec de chargement du gestionnaire out-of-process ANCM</span><span class="sxs-lookup"><span data-stu-id="4fa76-208">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="4fa76-209">Le gestionnaire de requêtes out-of-process, *aspnetcorev2_outofprocess.dll*, ne se trouve pas aux côtés du fichier *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-209">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="4fa76-210">Cela indique une installation endommagée du [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="4fa76-210">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="4fa76-211">Pour corriger cette erreur, réparez l’installation du [bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (pour IIS) ou Visual Studio (pour IIS Express).</span><span class="sxs-lookup"><span data-stu-id="4fa76-211">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="4fa76-212">500.37 - Échec du démarrage d’ANCM dans le délai imparti</span><span class="sxs-lookup"><span data-stu-id="4fa76-212">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="4fa76-213">ANCM n’a pas pu démarrer dans le délai imparti spécifié.</span><span class="sxs-lookup"><span data-stu-id="4fa76-213">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="4fa76-214">Par défaut, le délai d’expiration est de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="4fa76-214">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="4fa76-215">Cette erreur peut se produire quand un grand nombre d’applications démarrent sur la même machine.</span><span class="sxs-lookup"><span data-stu-id="4fa76-215">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="4fa76-216">Recherchez les pics d’utilisation du processeur/de la mémoire sur le serveur durant le démarrage.</span><span class="sxs-lookup"><span data-stu-id="4fa76-216">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="4fa76-217">Vous devrez peut-être décaler le processus de démarrage de plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="4fa76-217">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="4fa76-218">Échec de processus 502.5</span><span class="sxs-lookup"><span data-stu-id="4fa76-218">502.5 Process Failure</span></span>

<span data-ttu-id="4fa76-219">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="4fa76-219">The worker process fails.</span></span> <span data-ttu-id="4fa76-220">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4fa76-220">The app doesn't start.</span></span>

<span data-ttu-id="4fa76-221">Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tente, en vain, de démarrer le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="4fa76-221">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="4fa76-222">La cause d’un échec de démarrage d’un processus peut généralement être déterminée à partir des entrées dans le journal des événements d’application et le journal stdout du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4fa76-222">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="4fa76-223">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="4fa76-223">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="4fa76-224">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="4fa76-224">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="4fa76-225">Le *framework partagé* est le jeu d’assemblys (fichiers *.dll*) qui sont installés sur l’ordinateur et référencés par un métapaquet comme `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-225">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="4fa76-226">La référence de métapaquet peut spécifier une version minimale requise.</span><span class="sxs-lookup"><span data-stu-id="4fa76-226">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="4fa76-227">Pour plus d’informations, consultez [Le framework partagé](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="4fa76-227">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="4fa76-228">La page d’erreur d’un *échec de processus 502.5* est retournée quand une erreur de configuration d’hébergement ou d’application entraîne l’échec du processus de travail :</span><span class="sxs-lookup"><span data-stu-id="4fa76-228">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="4fa76-229">Échec du démarrage de l’application (code d’erreur « 0x800700c1 »)</span><span class="sxs-lookup"><span data-stu-id="4fa76-229">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="4fa76-230">L’application n’a pas pu démarrer, car l’assembly de l’application ( *.dll*) n’a pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="4fa76-230">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="4fa76-231">Cette erreur se produit lorsqu’il existe une incompatibilité au niveau du nombre de bits entre l’application publiée et le processus w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="4fa76-231">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="4fa76-232">Vérifiez que le paramètre 32 bits du pool d’applications est correct :</span><span class="sxs-lookup"><span data-stu-id="4fa76-232">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="4fa76-233">Sélectionnez le pool d’applications parmi les **Pools d’applications** IIS Manager.</span><span class="sxs-lookup"><span data-stu-id="4fa76-233">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="4fa76-234">Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-234">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="4fa76-235">Définissez **Activer les applications 32 bits** :</span><span class="sxs-lookup"><span data-stu-id="4fa76-235">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="4fa76-236">Si vous déployez une application 32 bits (x86), définissez la valeur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-236">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="4fa76-237">Si vous déployez une application 64 bits (x64), définissez la valeur sur `False`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-237">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

<span data-ttu-id="4fa76-238">Confirmez qu’il n’existe pas de conflit entre une `<Platform>` propriété MSBuild dans le fichier projet et le nombre de bits publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-238">Confirm that there isn't a conflict between a `<Platform>` MSBuild property in the project file and the published bitness of the app.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="4fa76-239">Réinitialisation de la connexion</span><span class="sxs-lookup"><span data-stu-id="4fa76-239">Connection reset</span></span>

<span data-ttu-id="4fa76-240">Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-240">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="4fa76-241">C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="4fa76-241">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="4fa76-242">Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client.</span><span class="sxs-lookup"><span data-stu-id="4fa76-242">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="4fa76-243">La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.</span><span class="sxs-lookup"><span data-stu-id="4fa76-243">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="4fa76-244">Limites de démarrage par défaut</span><span class="sxs-lookup"><span data-stu-id="4fa76-244">Default startup limits</span></span>

<span data-ttu-id="4fa76-245">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) est configuré avec un *startupTimeLimit* par défaut de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="4fa76-245">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="4fa76-246">Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus.</span><span class="sxs-lookup"><span data-stu-id="4fa76-246">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="4fa76-247">Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="4fa76-247">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="4fa76-248">Résoudre les problèmes sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa76-248">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="4fa76-249">Journal des événements d’application (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4fa76-249">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="4fa76-250">Pour accéder au Journal des événements de l’application, utilisez le panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure :</span><span class="sxs-lookup"><span data-stu-id="4fa76-250">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="4fa76-251">Dans le Portail Azure, ouvrez l’application dans **App Services**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-251">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="4fa76-252">Sélectionnez **Diagnostiquer et résoudre les problèmes**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-252">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="4fa76-253">Sélectionnez le titre **Outils de diagnostic**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-253">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="4fa76-254">Sous **Outils de support**, sélectionnez le bouton **Événements d’application**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-254">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="4fa76-255">Examinez la dernière erreur fournie par l’entrée *IIS AspNetCoreModule* ou *IIS AspNetCoreModule V2* dans la colonne **Source**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-255">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="4fa76-256">En dehors de la solution consistant à utiliser le panneau **Diagnostiquer et résoudre les problèmes**, vous pouvez examiner directement le fichier Journal des événements de l’application avec [Kudu](https://github.com/projectkudu/kudu/wiki) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-256">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="4fa76-257">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-257">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4fa76-258">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="4fa76-258">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4fa76-259">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-259">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4fa76-260">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-260">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4fa76-261">Ouvrez le dossier **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-261">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="4fa76-262">Sélectionnez l’icône en forme de crayon à côté du fichier *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-262">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="4fa76-263">Examinez le journal.</span><span class="sxs-lookup"><span data-stu-id="4fa76-263">Examine the log.</span></span> <span data-ttu-id="4fa76-264">Allez en bas du journal pour voir les événements les plus récents.</span><span class="sxs-lookup"><span data-stu-id="4fa76-264">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="4fa76-265">Exécuter l’application dans la console Kudu</span><span class="sxs-lookup"><span data-stu-id="4fa76-265">Run the app in the Kudu console</span></span>

<span data-ttu-id="4fa76-266">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-266">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="4fa76-267">Vous pouvez exécuter l’application dans la console d’exécution à distance [Kudu](https://github.com/projectkudu/kudu/wiki) pour détecter l’erreur :</span><span class="sxs-lookup"><span data-stu-id="4fa76-267">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="4fa76-268">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-268">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4fa76-269">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="4fa76-269">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4fa76-270">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-270">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4fa76-271">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-271">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="4fa76-272">Tester une application 32 bits (x86)</span><span class="sxs-lookup"><span data-stu-id="4fa76-272">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="4fa76-273">**Version actuelle**</span><span class="sxs-lookup"><span data-stu-id="4fa76-273">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="4fa76-274">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="4fa76-274">Run the app:</span></span>
   * <span data-ttu-id="4fa76-275">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-275">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```dotnetcli
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="4fa76-276">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-276">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="4fa76-277">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="4fa76-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="4fa76-278">**Déploiement dépendant du Framework s’exécutant sur une version préliminaire**</span><span class="sxs-lookup"><span data-stu-id="4fa76-278">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="4fa76-279">*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x86) requise.*</span><span class="sxs-lookup"><span data-stu-id="4fa76-279">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="4fa76-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` est la version du runtime).</span><span class="sxs-lookup"><span data-stu-id="4fa76-280">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="4fa76-281">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-281">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="4fa76-282">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="4fa76-282">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="4fa76-283">Tester une application 64 bits (x64)</span><span class="sxs-lookup"><span data-stu-id="4fa76-283">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="4fa76-284">**Version actuelle**</span><span class="sxs-lookup"><span data-stu-id="4fa76-284">**Current release**</span></span>

* <span data-ttu-id="4fa76-285">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) 64 bits (x64) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-285">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="4fa76-286">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-286">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="4fa76-287">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-287">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="4fa76-288">Exécutez l'application : `{ASSEMBLY NAME}.exe`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-288">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="4fa76-289">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="4fa76-289">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="4fa76-290">**Déploiement dépendant du Framework s’exécutant sur une version préliminaire**</span><span class="sxs-lookup"><span data-stu-id="4fa76-290">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="4fa76-291">*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x64) requise.*</span><span class="sxs-lookup"><span data-stu-id="4fa76-291">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="4fa76-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` est la version du runtime).</span><span class="sxs-lookup"><span data-stu-id="4fa76-292">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="4fa76-293">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-293">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="4fa76-294">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="4fa76-294">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="4fa76-295">Journal stdout du module ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4fa76-295">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="4fa76-296">Le journal stdout du module ASP.NET Core enregistre souvent des messages d’erreur utiles et absents du Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-296">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="4fa76-297">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="4fa76-297">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="4fa76-298">Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="4fa76-298">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="4fa76-299">Sous **SÉLECTIONNER UNE CATÉGORIE DE PROBLÈME**, sélectionnez le bouton **Application web en panne**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-299">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="4fa76-300">Sous **Solutions suggérées** > **Activer la redirection de journaux stdout**, sélectionnez le bouton **Ouvrir la console Kudu pour modifier web.config**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-300">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="4fa76-301">Dans la **Console de diagnostic** Kudu, ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-301">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="4fa76-302">Faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.</span><span class="sxs-lookup"><span data-stu-id="4fa76-302">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="4fa76-303">Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-303">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="4fa76-304">Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-304">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="4fa76-305">Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="4fa76-305">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="4fa76-306">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-306">Make a request to the app.</span></span>
1. <span data-ttu-id="4fa76-307">Revenez sur le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="4fa76-307">Return to the Azure portal.</span></span> <span data-ttu-id="4fa76-308">Sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-308">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="4fa76-309">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="4fa76-309">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4fa76-310">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-310">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4fa76-311">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-311">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4fa76-312">Sélectionnez le dossier **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-312">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="4fa76-313">Inspectez la colonne **Modifié** et sélectionnez l’icône en forme de crayon pour modifier le journal stdout avec la date de dernière modification.</span><span class="sxs-lookup"><span data-stu-id="4fa76-313">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="4fa76-314">Lorsque le fichier journal s’ouvre, l’erreur s’affiche.</span><span class="sxs-lookup"><span data-stu-id="4fa76-314">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="4fa76-315">Désactivez la journalisation stdout, une fois les problèmes résolus :</span><span class="sxs-lookup"><span data-stu-id="4fa76-315">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="4fa76-316">Dans la **Console de diagnostic** Kudu, revenez au chemin d’accès **site** > **wwwroot** pour faire apparaître le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-316">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="4fa76-317">Ouvrez à nouveau le fichier **web.config** en sélectionnant l’icône en forme de crayon.</span><span class="sxs-lookup"><span data-stu-id="4fa76-317">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="4fa76-318">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-318">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="4fa76-319">Sélectionnez **Enregistrer** pour enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="4fa76-319">Select **Save** to save the file.</span></span>

<span data-ttu-id="4fa76-320">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="4fa76-320">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="4fa76-321">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="4fa76-321">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="4fa76-322">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="4fa76-322">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="4fa76-323">N’utilisez la journalisation stdout que pour résoudre les problèmes de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-323">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="4fa76-324">Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="4fa76-324">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4fa76-325">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4fa76-325">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="4fa76-326">Journal de débogage du module ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4fa76-326">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="4fa76-327">Le journal de débogage du module ASP.NET Core fournit une journalisation supplémentaire, plus approfondie, à partir du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4fa76-327">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="4fa76-328">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="4fa76-328">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="4fa76-329">Pour activer le journal de diagnostic amélioré, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fa76-329">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="4fa76-330">Suivez les instructions indiquées dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) afin de configurer l’application pour une journalisation des diagnostics améliorée.</span><span class="sxs-lookup"><span data-stu-id="4fa76-330">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="4fa76-331">Redéployez l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-331">Redeploy the app.</span></span>
   * <span data-ttu-id="4fa76-332">Ajoutez le `<handlerSettings>` présenté dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) au fichier *web.config* de l’application en production à l’aide de la console Kudu :</span><span class="sxs-lookup"><span data-stu-id="4fa76-332">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="4fa76-333">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-333">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4fa76-334">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="4fa76-334">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4fa76-335">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-335">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="4fa76-336">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-336">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="4fa76-337">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-337">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="4fa76-338">Modifiez le fichier *web.config* en sélectionnant le bouton représentant un crayon.</span><span class="sxs-lookup"><span data-stu-id="4fa76-338">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="4fa76-339">Ajoutez la section `<handlerSettings>` comme indiqué dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="4fa76-339">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="4fa76-340">Sélectionnez le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-340">Select the **Save** button.</span></span>
1. <span data-ttu-id="4fa76-341">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-341">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="4fa76-342">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="4fa76-342">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4fa76-343">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-343">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4fa76-344">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-344">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4fa76-345">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-345">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="4fa76-346">Si vous n’avez pas indiqué de chemin pour le fichier *aspnetcore-debug.log*, le fichier apparaît dans la liste.</span><span class="sxs-lookup"><span data-stu-id="4fa76-346">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="4fa76-347">Si vous avez indiqué un chemin, accédez à l’emplacement du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="4fa76-347">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="4fa76-348">Ouvrez le fichier journal à l’aide du bouton représentant un crayon à côté du nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="4fa76-348">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="4fa76-349">Désactivez la journalisation du débogage, une fois la résolution des problèmes effectuée :</span><span class="sxs-lookup"><span data-stu-id="4fa76-349">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="4fa76-350">Pour désactiver la journalisation de débogage améliorée, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fa76-350">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="4fa76-351">Supprimez `<handlerSettings>` du fichier *web.config* localement, puis redéployez l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-351">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="4fa76-352">Utilisez la console Kudu pour modifier le fichier *web.config* et supprimer la section `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-352">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="4fa76-353">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="4fa76-353">Save the file.</span></span>

<span data-ttu-id="4fa76-354">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="4fa76-354">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="4fa76-355">Si le journal de débogage n’est pas désactivé, cela peut entraîner une défaillance de l’application ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-355">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="4fa76-356">Il n’existe aucune limite à la taille du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="4fa76-356">There's no limit on log file size.</span></span> <span data-ttu-id="4fa76-357">Utilisez uniquement la journalisation de débogage pour résoudre les problèmes de démarrage d’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-357">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="4fa76-358">Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="4fa76-358">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4fa76-359">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4fa76-359">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="4fa76-360">Application lente ou suspendue (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="4fa76-360">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="4fa76-361">Si une application répond lentement ou se bloque sur une requête, voir les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="4fa76-361">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="4fa76-362">Résoudre les problèmes de performances d’une application web lente dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa76-362">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="4fa76-363">Utiliser l’extension de site Outil de diagnostic des incidents pour capturer une image mémoire des problèmes d’exception intermittente ou de performances sur Azure Web App</span><span class="sxs-lookup"><span data-stu-id="4fa76-363">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="4fa76-364">Panneaux Monitoring</span><span class="sxs-lookup"><span data-stu-id="4fa76-364">Monitoring blades</span></span>

<span data-ttu-id="4fa76-365">Les panneaux Monitoring offrent une expérience de résolution des erreurs différente des méthodes décrites précédemment dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="4fa76-365">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="4fa76-366">Ils peuvent servir à diagnostiquer les erreurs de la série 500.</span><span class="sxs-lookup"><span data-stu-id="4fa76-366">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="4fa76-367">Vérifiez que les extensions ASP.NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="4fa76-367">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="4fa76-368">Si ce n’est pas le cas, installez-les manuellement :</span><span class="sxs-lookup"><span data-stu-id="4fa76-368">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="4fa76-369">Dans la section du panneau **OUTILS DE DÉVELOPPEMENT**, sélectionnez le panneau **Extensions**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-369">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="4fa76-370">Les **Extensions ASP.NET Core** devraient apparaître dans la liste.</span><span class="sxs-lookup"><span data-stu-id="4fa76-370">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="4fa76-371">Si les extensions ne sont pas installées, sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-371">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="4fa76-372">Choisissez les **Extensions ASP.NET Core** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="4fa76-372">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="4fa76-373">Sélectionnez **OK** pour accepter les conditions légales.</span><span class="sxs-lookup"><span data-stu-id="4fa76-373">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="4fa76-374">Sélectionnez **OK** sur le panneau **Ajouter une extension**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-374">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="4fa76-375">Un message contextuel d’information indique que les extensions sont bien installées.</span><span class="sxs-lookup"><span data-stu-id="4fa76-375">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="4fa76-376">Si la journalisation stdout n’est pas activée, suivez les étapes ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="4fa76-376">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="4fa76-377">Sur le Portail Azure, sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-377">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="4fa76-378">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="4fa76-378">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="4fa76-379">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="4fa76-379">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="4fa76-380">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-380">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="4fa76-381">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot** et faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.</span><span class="sxs-lookup"><span data-stu-id="4fa76-381">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="4fa76-382">Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-382">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="4fa76-383">Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-383">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="4fa76-384">Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="4fa76-384">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="4fa76-385">Maintenant, activez la journalisation des diagnostics :</span><span class="sxs-lookup"><span data-stu-id="4fa76-385">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="4fa76-386">Sur le Portail Azure, sélectionnez le panneau **Journaux de diagnostic**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-386">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="4fa76-387">Basculez **Journalisation des applications (système de fichiers)** et **Messages d’erreur détaillés** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-387">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="4fa76-388">Sélectionnez le bouton **Enregistrer** en haut du panneau.</span><span class="sxs-lookup"><span data-stu-id="4fa76-388">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="4fa76-389">Pour inclure le suivi des demandes ayant échoué, également appelé Mise en mémoire tampon des événements d’échec des demandes (FREB), basculez **Suivi des demandes ayant échoué** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-389">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="4fa76-390">Sélectionnez le panneau **Flux du journal** situé juste sous le panneau **Journaux de diagnostic** sur le portail.</span><span class="sxs-lookup"><span data-stu-id="4fa76-390">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="4fa76-391">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-391">Make a request to the app.</span></span>
1. <span data-ttu-id="4fa76-392">La cause de l’erreur est indiquée dans les données de flux de journal.</span><span class="sxs-lookup"><span data-stu-id="4fa76-392">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="4fa76-393">Veillez à désactiver la journalisation stdout une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="4fa76-393">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="4fa76-394">Pour afficher les journaux de suivi des demandes ayant échoué (journaux FREB) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-394">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="4fa76-395">Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="4fa76-395">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="4fa76-396">Sélectionnez **Journaux de suivi des demandes ayant échoué** dans la zone **OUTILS DE PRISE EN CHARGE** de la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="4fa76-396">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="4fa76-397">Voir la [section Traces des demandes ayant échoué de la rubrique Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) et [FAQ sur les performances des applications web dans Azure : comment activer le suivi des demandes ayant échoué ?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="4fa76-397">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="4fa76-398">Pour plus d’informations, voir [Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="4fa76-398">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="4fa76-399">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="4fa76-399">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="4fa76-400">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="4fa76-400">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="4fa76-401">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="4fa76-401">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4fa76-402">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4fa76-402">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="4fa76-403">Résoudre les problèmes sur IIS</span><span class="sxs-lookup"><span data-stu-id="4fa76-403">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="4fa76-404">Journal des événements d’application (IIS)</span><span class="sxs-lookup"><span data-stu-id="4fa76-404">Application Event Log (IIS)</span></span>

<span data-ttu-id="4fa76-405">Accédez au Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="4fa76-405">Access the Application Event Log:</span></span>

1. <span data-ttu-id="4fa76-406">Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez l’application **Observateur d’événements**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-406">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="4fa76-407">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="4fa76-407">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="4fa76-408">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-408">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="4fa76-409">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="4fa76-409">Search for errors associated with the failing app.</span></span> <span data-ttu-id="4fa76-410">Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-410">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="4fa76-411">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="4fa76-411">Run the app at a command prompt</span></span>

<span data-ttu-id="4fa76-412">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-412">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="4fa76-413">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="4fa76-413">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="4fa76-414">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="4fa76-414">Framework-dependent deployment</span></span>

<span data-ttu-id="4fa76-415">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-415">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="4fa76-416">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-416">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="4fa76-417">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-417">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="4fa76-418">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="4fa76-418">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="4fa76-419">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="4fa76-419">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="4fa76-420">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-420">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="4fa76-421">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-421">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="4fa76-422">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="4fa76-422">Self-contained deployment</span></span>

<span data-ttu-id="4fa76-423">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-423">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="4fa76-424">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-424">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="4fa76-425">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-425">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="4fa76-426">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="4fa76-426">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="4fa76-427">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="4fa76-427">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="4fa76-428">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-428">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="4fa76-429">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-429">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="4fa76-430">Journal stdout du module ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="4fa76-430">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="4fa76-431">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="4fa76-431">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="4fa76-432">Accédez au dossier de déploiement du site sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="4fa76-432">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="4fa76-433">Si le dossier *logs* n’est pas présent, créez-le.</span><span class="sxs-lookup"><span data-stu-id="4fa76-433">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="4fa76-434">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="4fa76-434">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="4fa76-435">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-435">Edit the *web.config* file.</span></span> <span data-ttu-id="4fa76-436">Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="4fa76-436">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="4fa76-437">Dans le chemin, `stdout` est le préfixe du nom du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="4fa76-437">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="4fa76-438">Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé.</span><span class="sxs-lookup"><span data-stu-id="4fa76-438">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="4fa76-439">Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-439">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="4fa76-440">Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-440">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="4fa76-441">Enregistrez le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="4fa76-441">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="4fa76-442">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-442">Make a request to the app.</span></span>
1. <span data-ttu-id="4fa76-443">Accédez au dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-443">Navigate to the *logs* folder.</span></span> <span data-ttu-id="4fa76-444">Recherchez et ouvrez le journal stdout le plus récent.</span><span class="sxs-lookup"><span data-stu-id="4fa76-444">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="4fa76-445">Déterminez si le journal contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="4fa76-445">Study the log for errors.</span></span>

<span data-ttu-id="4fa76-446">Désactivez la journalisation stdout, une fois les problèmes résolus :</span><span class="sxs-lookup"><span data-stu-id="4fa76-446">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="4fa76-447">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-447">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="4fa76-448">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-448">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="4fa76-449">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="4fa76-449">Save the file.</span></span>

<span data-ttu-id="4fa76-450">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="4fa76-450">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="4fa76-451">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="4fa76-451">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="4fa76-452">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="4fa76-452">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="4fa76-453">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="4fa76-453">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="4fa76-454">Pour plus d’informations, consultez [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="4fa76-454">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="4fa76-455">Journal de débogage du module ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="4fa76-455">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="4fa76-456">Ajoutez les paramètres de gestionnaire suivants au fichier *Web. config* de l’application pour activer ASP.net Core journal de débogage du module :</span><span class="sxs-lookup"><span data-stu-id="4fa76-456">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="4fa76-457">Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="4fa76-457">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="4fa76-458">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="4fa76-458">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="4fa76-459">Afficher la page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="4fa76-459">Enable the Developer Exception Page</span></span>

<span data-ttu-id="4fa76-460">La [variable d’environnement `ASPNETCORE_ENVIRONMENT` peut être ajoutée à Web. config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="4fa76-460">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="4fa76-461">Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="4fa76-461">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="4fa76-462">La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="4fa76-462">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="4fa76-463">Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="4fa76-463">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="4fa76-464">Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="4fa76-464">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="4fa76-465">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="4fa76-465">Obtain data from an app</span></span>

<span data-ttu-id="4fa76-466">Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal.</span><span class="sxs-lookup"><span data-stu-id="4fa76-466">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="4fa76-467">Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="4fa76-467">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="4fa76-468">Application lente ou bloquée (IIS)</span><span class="sxs-lookup"><span data-stu-id="4fa76-468">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="4fa76-469">Un *vidage sur incident* est un instantané de la mémoire du système et peut aider à déterminer la cause d’un incident d’application, d’un échec de démarrage ou d’une application lente.</span><span class="sxs-lookup"><span data-stu-id="4fa76-469">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="4fa76-470">L’application cesse de fonctionner ou rencontre une exception</span><span class="sxs-lookup"><span data-stu-id="4fa76-470">App crashes or encounters an exception</span></span>

<span data-ttu-id="4fa76-471">Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-471">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="4fa76-472">Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-472">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="4fa76-473">Le pool d’application doit disposer des accès d’écriture dans le dossier.</span><span class="sxs-lookup"><span data-stu-id="4fa76-473">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="4fa76-474">Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-474">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="4fa76-475">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="4fa76-475">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="4fa76-476">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="4fa76-476">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="4fa76-477">Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.</span><span class="sxs-lookup"><span data-stu-id="4fa76-477">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="4fa76-478">Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="4fa76-478">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="4fa76-479">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="4fa76-479">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="4fa76-480">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="4fa76-480">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="4fa76-481">Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement.</span><span class="sxs-lookup"><span data-stu-id="4fa76-481">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="4fa76-482">Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-482">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="4fa76-483">Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).</span><span class="sxs-lookup"><span data-stu-id="4fa76-483">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="4fa76-484">L’application se bloque, ne démarre pas ou s’exécute normalement</span><span class="sxs-lookup"><span data-stu-id="4fa76-484">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="4fa76-485">Quand une application *se bloque* (cesse de répondre mais ne se bloque pas), échoue pendant le démarrage ou s’exécute normalement, consultez [fichiers de vidage en mode utilisateur : choix de l’outil le mieux](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) adapté pour sélectionner un outil approprié pour produire le vidage.</span><span class="sxs-lookup"><span data-stu-id="4fa76-485">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="4fa76-486">Analyser le fichier dump</span><span class="sxs-lookup"><span data-stu-id="4fa76-486">Analyze the dump</span></span>

<span data-ttu-id="4fa76-487">Un fichier dump peut être analysé à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="4fa76-487">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="4fa76-488">Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).</span><span class="sxs-lookup"><span data-stu-id="4fa76-488">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="4fa76-489">Effacer les caches de package</span><span class="sxs-lookup"><span data-stu-id="4fa76-489">Clear package caches</span></span>

<span data-ttu-id="4fa76-490">Parfois, une application opérationnelle échoue immédiatement après la mise à niveau de la kit SDK .NET Core sur l’ordinateur de développement ou de la modification des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-490">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="4fa76-491">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="4fa76-491">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="4fa76-492">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fa76-492">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="4fa76-493">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="4fa76-493">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="4fa76-494">Effacez les caches de package en exécutant `dotnet nuget locals all --clear` à partir d’une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="4fa76-494">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="4fa76-495">L’effacement des caches de package peut également être effectué à l’aide de l’outil [NuGet. exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="4fa76-495">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="4fa76-496">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="4fa76-496">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="4fa76-497">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="4fa76-497">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="4fa76-498">Supprimez tous les fichiers du dossier de déploiement sur le serveur avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="4fa76-498">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fa76-499">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4fa76-499">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="4fa76-500">Documentation Azure</span><span class="sxs-lookup"><span data-stu-id="4fa76-500">Azure documentation</span></span>

* [<span data-ttu-id="4fa76-501">Application Insights pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4fa76-501">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="4fa76-502">Section débogage à distance des applications Web de dépanner une application Web dans Azure App Service à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fa76-502">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="4fa76-503">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa76-503">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="4fa76-504">Guide pratique pour surveiller des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa76-504">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="4fa76-505">Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fa76-505">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="4fa76-506">Résoudre les erreurs HTTP « 502 Passerelle incorrecte » et « 503 Service non disponible » dans des applications web Azure</span><span class="sxs-lookup"><span data-stu-id="4fa76-506">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="4fa76-507">Résoudre les problèmes de performances d’une application web lente dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4fa76-507">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="4fa76-508">FAQ sur les performances des applications web dans Azure</span><span class="sxs-lookup"><span data-stu-id="4fa76-508">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="4fa76-509">Bac à sable des applications web Azure (limitations de l’exécution du runtime App Service)</span><span class="sxs-lookup"><span data-stu-id="4fa76-509">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="4fa76-510">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span><span class="sxs-lookup"><span data-stu-id="4fa76-510">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="4fa76-511">Documentation de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fa76-511">Visual Studio documentation</span></span>

* [<span data-ttu-id="4fa76-512">Débogage à distance ASP.NET Core sur IIS dans Azure dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4fa76-512">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="4fa76-513">Débogage à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4fa76-513">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="4fa76-514">Apprendre à déboguer avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fa76-514">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="4fa76-515">Documentation Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4fa76-515">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="4fa76-516">Débogage avec Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4fa76-516">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
