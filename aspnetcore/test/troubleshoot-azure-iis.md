---
title: Résoudre les problèmes de ASP.NET Core sur Azure App Service et IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Azure App Service et Internet Information Services (IIS) des applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2019
uid: test/troubleshoot-azure-iis
ms.openlocfilehash: 46d4a11c594844e059fa8555255d7321f7b48631
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308792"
---
# <a name="troubleshoot-aspnet-core-on-azure-app-service-and-iis"></a><span data-ttu-id="e9ca3-103">Résoudre les problèmes de ASP.NET Core sur Azure App Service et IIS</span><span class="sxs-lookup"><span data-stu-id="e9ca3-103">Troubleshoot ASP.NET Core on Azure App Service and IIS</span></span>

<span data-ttu-id="e9ca3-104">Par [Luke Latham](https://github.com/guardrex) et [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-104">By [Luke Latham](https://github.com/guardrex) and [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="e9ca3-105">Cet article fournit des informations sur les erreurs de démarrage d’application courantes et des instructions sur la façon de diagnostiquer les erreurs quand une application est déployée sur Azure App Service ou IIS:</span><span class="sxs-lookup"><span data-stu-id="e9ca3-105">This article provides information on common app startup errors and instructions on how to diagnose errors when an app is deployed to Azure App Service or IIS:</span></span>

[<span data-ttu-id="e9ca3-106">Erreurs de démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="e9ca3-106">App startup errors</span></span>](#app-startup-errors)  
<span data-ttu-id="e9ca3-107">Explique les scénarios courants de code d’état HTTP de démarrage.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-107">Explains common startup HTTP status code scenarios.</span></span>

[<span data-ttu-id="e9ca3-108">Résoudre les problèmes sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e9ca3-108">Troubleshoot on Azure App Service</span></span>](#troubleshoot-on-azure-app-service)  
<span data-ttu-id="e9ca3-109">Fournit des conseils de dépannage pour les applications déployées sur Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-109">Provides troubleshooting advice for apps deployed to Azure App Service.</span></span>

[<span data-ttu-id="e9ca3-110">Résoudre les problèmes sur IIS</span><span class="sxs-lookup"><span data-stu-id="e9ca3-110">Troubleshoot on IIS</span></span>](#troubleshoot-on-iis)  
<span data-ttu-id="e9ca3-111">Fournit des conseils de dépannage pour les applications déployées sur IIS ou exécutées localement sur IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-111">Provides troubleshooting advice for apps deployed to IIS or running on IIS Express locally.</span></span> <span data-ttu-id="e9ca3-112">Ce guide s’applique aux déploiements Windows Server et Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-112">The guidance applies to both Windows Server and Windows desktop deployments.</span></span>

[<span data-ttu-id="e9ca3-113">Effacer les caches de package</span><span class="sxs-lookup"><span data-stu-id="e9ca3-113">Clear package caches</span></span>](#clear-package-caches)  
<span data-ttu-id="e9ca3-114">Explique ce qu’il faut faire quand des packages incohérents interrompent une application lors des mises à niveau majeures ou de la modification des versions de package.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-114">Explains what to do when incoherent packages break an app when performing major upgrades or changing package versions.</span></span>

[<span data-ttu-id="e9ca3-115">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e9ca3-115">Additional resources</span></span>](#additional-resources)  
<span data-ttu-id="e9ca3-116">Répertorie des rubriques supplémentaires sur la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-116">Lists additional troubleshooting topics.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="e9ca3-117">Erreurs de démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="e9ca3-117">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e9ca3-118">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-118">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="e9ca3-119">Un *échec de processus 502,5* ou un *échec de démarrage de 500,30* qui se produit lorsque le débogage local peut être diagnostiqué à l’aide des conseils de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-119">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e9ca3-120">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-120">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="e9ca3-121">Un *échec de processus 502,5* qui se produit lorsque le débogage local peut être diagnostiqué à l’aide des conseils de cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-121">A *502.5 Process Failure* that occurs when debugging locally can be diagnosed using the advice in this topic.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="e9ca3-122">Erreur de serveur interne 500</span><span class="sxs-lookup"><span data-stu-id="e9ca3-122">500 Internal Server Error</span></span>

<span data-ttu-id="e9ca3-123">L’application démarre, mais une erreur empêche le serveur de répondre à la requête.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-123">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="e9ca3-124">Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-124">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="e9ca3-125">La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-125">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="e9ca3-126">Le Journal des événements de l’application indique généralement qu’elle a démarré normalement.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-126">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="e9ca3-127">Du point de vue du serveur, c’est exact.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-127">From the server's perspective, that's correct.</span></span> <span data-ttu-id="e9ca3-128">L’application a démarré, mais elle ne parvient pas à générer une réponse valide.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-128">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="e9ca3-129">Exécutez l’application à partir d’une invite de commandes sur le serveur ou activez le journal stdout du module ASP.NET Core pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-129">Run the app at a command prompt on the server or enable the ASP.NET Core Module stdout log to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="e9ca3-130">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="e9ca3-130">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="e9ca3-131">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-131">The worker process fails.</span></span> <span data-ttu-id="e9ca3-132">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-132">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-133">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) ne parvient pas à trouver le CLR .net Core et à rechercher le gestionnaire de demandes in-process (*aspnetcorev2_inprocess. dll*).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-133">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="e9ca3-134">Vérifiez que :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-134">Check that:</span></span>

* <span data-ttu-id="e9ca3-135">l’application cible le package NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) ou le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ;</span><span class="sxs-lookup"><span data-stu-id="e9ca3-135">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="e9ca3-136">la version du framework partagé ASP.NET Core que l’application cible est installée sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-136">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="e9ca3-137">500.0 - Échec de chargement du gestionnaire out-of-process</span><span class="sxs-lookup"><span data-stu-id="e9ca3-137">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="e9ca3-138">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-138">The worker process fails.</span></span> <span data-ttu-id="e9ca3-139">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-139">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-140">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) ne parvient pas à trouver le gestionnaire de demandes d’hébergement hors processus.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-140">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="e9ca3-141">Vérifiez que le fichier *aspnetcorev2_outofprocess.dll* est présent dans un sous-dossier en regard de *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-141">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="e9ca3-142">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="e9ca3-142">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="e9ca3-143">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-143">The worker process fails.</span></span> <span data-ttu-id="e9ca3-144">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-144">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-145">Une erreur inconnue s’est produite lors du chargement des composants du [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) .</span><span class="sxs-lookup"><span data-stu-id="e9ca3-145">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="e9ca3-146">Effectuez l’une des actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-146">Take one of the following actions:</span></span>

* <span data-ttu-id="e9ca3-147">Contactez le [Support Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) (sélectionnez **Outils de développement**, puis **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-147">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="e9ca3-148">Posez une question sur Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-148">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="e9ca3-149">Signalez un problème sur notre [dépôt GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-149">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="e9ca3-150">500.30 - Échec du démarrage in-process</span><span class="sxs-lookup"><span data-stu-id="e9ca3-150">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="e9ca3-151">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-151">The worker process fails.</span></span> <span data-ttu-id="e9ca3-152">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-152">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-153">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) tente de démarrer le CLR .net Core in-process, mais il ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-153">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="e9ca3-154">La cause d’un échec de démarrage d’un processus peut généralement être déterminée à partir des entrées dans le journal des événements d’application et le journal stdout du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-154">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="e9ca3-155">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-155">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="e9ca3-156">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-156">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="e9ca3-157">500.31 - Échec de la recherche de dépendances natives par ANCM</span><span class="sxs-lookup"><span data-stu-id="e9ca3-157">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="e9ca3-158">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-158">The worker process fails.</span></span> <span data-ttu-id="e9ca3-159">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-159">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-160">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) tente de démarrer le Runtime .net Core in-process, mais il ne parvient pas à démarrer.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-160">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="e9ca3-161">La cause la plus courante de cet échec de démarrage est liée à la non-installation du runtime `Microsoft.NETCore.App` ou `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-161">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="e9ca3-162">Si l’application est déployée pour cibler ASP.NET Core 3.0, et si cette version n’existe pas sur la machine, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-162">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="e9ca3-163">Voici un exemple de message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-163">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="e9ca3-164">Le message d’erreur liste toutes les versions installées de .NET Core ainsi que la version demandée par l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-164">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="e9ca3-165">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-165">To fix this error, either:</span></span>

* <span data-ttu-id="e9ca3-166">Installez la version appropriée de .NET Core sur la machine.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-166">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="e9ca3-167">Changez l’application pour cibler une version de .NET Core présente sur la machine.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-167">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="e9ca3-168">Publiez l’application sous forme de [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-168">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="e9ca3-169">Durant l’exécution en phase de développement (la variable d’environnement `ASPNETCORE_ENVIRONMENT` a la valeur `Development`), l’erreur spécifique est écrite dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-169">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="e9ca3-170">La cause de l’échec du démarrage d’un processus se trouve également dans le journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-170">The cause of a process startup failure is also found in the Application Event Log.</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="e9ca3-171">500.32 - Échec du chargement de la dll par ANCM</span><span class="sxs-lookup"><span data-stu-id="e9ca3-171">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="e9ca3-172">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-172">The worker process fails.</span></span> <span data-ttu-id="e9ca3-173">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-173">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-174">Le plus souvent, cette erreur provient du fait que l’application est publiée pour une architecture de processeur incompatible.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-174">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="e9ca3-175">Si le processus de travail s’exécute en tant qu’application 32 bits et si l’application a été publiée pour cibler une architecture 64 bits, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-175">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="e9ca3-176">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-176">To fix this error, either:</span></span>

* <span data-ttu-id="e9ca3-177">Republiez l’application pour la même architecture de processeur que celle du processus de travail.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-177">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="e9ca3-178">Publiez l’application sous forme de [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-178">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="e9ca3-179">500.33 - Échec du chargement du gestionnaire de requêtes ANCM</span><span class="sxs-lookup"><span data-stu-id="e9ca3-179">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="e9ca3-180">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-180">The worker process fails.</span></span> <span data-ttu-id="e9ca3-181">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-181">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-182">L’application n’a pas référencé le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-182">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="e9ca3-183">Seules les applications ciblant le `Microsoft.AspNetCore.App` Framework peuvent être hébergées par le [module ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-183">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="e9ca3-184">Pour corriger cette erreur, vérifiez que l’application cible le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-184">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="e9ca3-185">Examinez le fichier `.runtimeconfig.json` pour vérifier le framework ciblé par l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-185">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="e9ca3-186">500.34 - Modèles d’hébergement mixtes ANCM non pris en charge</span><span class="sxs-lookup"><span data-stu-id="e9ca3-186">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="e9ca3-187">Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-187">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="e9ca3-188">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-188">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="e9ca3-189">500.35 - Applications in-process multiples dans le même processus ANCM</span><span class="sxs-lookup"><span data-stu-id="e9ca3-189">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="e9ca3-190">Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-190">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="e9ca3-191">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-191">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="e9ca3-192">500.36 - Échec de chargement du gestionnaire out-of-process ANCM</span><span class="sxs-lookup"><span data-stu-id="e9ca3-192">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="e9ca3-193">Le gestionnaire de requêtes out-of-process, *aspnetcorev2_outofprocess.dll*, ne se trouve pas aux côtés du fichier *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-193">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="e9ca3-194">Cela indique une installation endommagée du [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-194">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="e9ca3-195">Pour corriger cette erreur, réparez l’installation du [bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (pour IIS) ou Visual Studio (pour IIS Express).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-195">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="e9ca3-196">500.37 - Échec du démarrage d’ANCM dans le délai imparti</span><span class="sxs-lookup"><span data-stu-id="e9ca3-196">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="e9ca3-197">ANCM n’a pas pu démarrer dans le délai imparti spécifié.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-197">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="e9ca3-198">Par défaut, le délai d’expiration est de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-198">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="e9ca3-199">Cette erreur peut se produire quand un grand nombre d’applications démarrent sur la même machine.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-199">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="e9ca3-200">Recherchez les pics d’utilisation du processeur/de la mémoire sur le serveur durant le démarrage.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-200">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="e9ca3-201">Vous devrez peut-être décaler le processus de démarrage de plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-201">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="e9ca3-202">Échec de processus 502.5</span><span class="sxs-lookup"><span data-stu-id="e9ca3-202">502.5 Process Failure</span></span>

<span data-ttu-id="e9ca3-203">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-203">The worker process fails.</span></span> <span data-ttu-id="e9ca3-204">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-204">The app doesn't start.</span></span>

<span data-ttu-id="e9ca3-205">Le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) tente, en vain, de démarrer le processus de travail.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-205">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="e9ca3-206">La cause d’un échec de démarrage d’un processus peut généralement être déterminée à partir des entrées dans le journal des événements d’application et le journal stdout du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-206">The cause of a process startup failure can usually be determined from entries in the Application Event Log and the ASP.NET Core Module stdout log.</span></span>

<span data-ttu-id="e9ca3-207">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-207">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="e9ca3-208">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-208">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="e9ca3-209">Le *framework partagé* est le jeu d’assemblys (fichiers *.dll*) qui sont installés sur l’ordinateur et référencés par un métapaquet comme `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-209">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="e9ca3-210">La référence de métapaquet peut spécifier une version minimale requise.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-210">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="e9ca3-211">Pour plus d’informations, consultez [Le framework partagé](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-211">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="e9ca3-212">La page d’erreur d’un *échec de processus 502.5* est retournée quand une erreur de configuration d’hébergement ou d’application entraîne l’échec du processus de travail :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-212">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="e9ca3-213">Échec du démarrage de l’application (code d’erreur « 0x800700c1 »)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-213">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="e9ca3-214">L’application n’a pas pu démarrer, car l’assembly de l’application ( *.dll*) n’a pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-214">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="e9ca3-215">Cette erreur se produit lorsqu’il existe une incompatibilité au niveau du nombre de bits entre l’application publiée et le processus w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-215">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="e9ca3-216">Vérifiez que le paramètre 32 bits du pool d’applications est correct :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-216">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="e9ca3-217">Sélectionnez le pool d’applications parmi les **Pools d’applications** IIS Manager.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-217">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="e9ca3-218">Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-218">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="e9ca3-219">Définissez **Activer les applications 32 bits** :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-219">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="e9ca3-220">Si vous déployez une application 32 bits (x86), définissez la valeur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-220">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="e9ca3-221">Si vous déployez une application 64 bits (x64), définissez la valeur sur `False`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-221">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="e9ca3-222">Réinitialisation de la connexion</span><span class="sxs-lookup"><span data-stu-id="e9ca3-222">Connection reset</span></span>

<span data-ttu-id="e9ca3-223">Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-223">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="e9ca3-224">C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-224">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="e9ca3-225">Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-225">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="e9ca3-226">La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-226">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

### <a name="default-startup-limits"></a><span data-ttu-id="e9ca3-227">Limites de démarrage par défaut</span><span class="sxs-lookup"><span data-stu-id="e9ca3-227">Default startup limits</span></span>

<span data-ttu-id="e9ca3-228">Le [Module ASP.net Core](xref:host-and-deploy/aspnet-core-module) est configuré avec un *startupTimeLimit* par défaut de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-228">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="e9ca3-229">Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-229">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="e9ca3-230">Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-230">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-on-azure-app-service"></a><span data-ttu-id="e9ca3-231">Résoudre les problèmes sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e9ca3-231">Troubleshoot on Azure App Service</span></span>

[!INCLUDE [Azure App Service Preview Notice](~/includes/azure-apps-preview-notice.md)]

### <a name="application-event-log-azure-app-service"></a><span data-ttu-id="e9ca3-232">Journal des événements d’application (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-232">Application Event Log (Azure App Service)</span></span>

<span data-ttu-id="e9ca3-233">Pour accéder au Journal des événements de l’application, utilisez le panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-233">To access the Application Event Log, use the **Diagnose and solve problems** blade in the Azure portal:</span></span>

1. <span data-ttu-id="e9ca3-234">Dans le Portail Azure, ouvrez l’application dans **App Services**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-234">In the Azure portal, open the app in **App Services**.</span></span>
1. <span data-ttu-id="e9ca3-235">Sélectionnez **Diagnostiquer et résoudre les problèmes**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-235">Select **Diagnose and solve problems**.</span></span>
1. <span data-ttu-id="e9ca3-236">Sélectionnez le titre **Outils de diagnostic**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-236">Select the **Diagnostic Tools** heading.</span></span>
1. <span data-ttu-id="e9ca3-237">Sous **Outils de support**, sélectionnez le bouton **Événements d’application**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-237">Under **Support Tools**, select the **Application Events** button.</span></span>
1. <span data-ttu-id="e9ca3-238">Examinez la dernière erreur fournie par l’entrée *IIS AspNetCoreModule* ou *IIS AspNetCoreModule V2* dans la colonne **Source**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-238">Examine the latest error provided by the *IIS AspNetCoreModule* or *IIS AspNetCoreModule V2* entry in the **Source** column.</span></span>

<span data-ttu-id="e9ca3-239">En dehors de la solution consistant à utiliser le panneau **Diagnostiquer et résoudre les problèmes**, vous pouvez examiner directement le fichier Journal des événements de l’application avec [Kudu](https://github.com/projectkudu/kudu/wiki) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-239">An alternative to using the **Diagnose and solve problems** blade is to examine the Application Event Log file directly using [Kudu](https://github.com/projectkudu/kudu/wiki):</span></span>

1. <span data-ttu-id="e9ca3-240">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-240">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="e9ca3-241">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="e9ca3-241">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="e9ca3-242">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-242">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="e9ca3-243">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-243">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="e9ca3-244">Ouvrez le dossier **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-244">Open the **LogFiles** folder.</span></span>
1. <span data-ttu-id="e9ca3-245">Sélectionnez l’icône en forme de crayon à côté du fichier *eventlog.xml*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-245">Select the pencil icon next to the *eventlog.xml* file.</span></span>
1. <span data-ttu-id="e9ca3-246">Examinez le journal.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-246">Examine the log.</span></span> <span data-ttu-id="e9ca3-247">Allez en bas du journal pour voir les événements les plus récents.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-247">Scroll to the bottom of the log to see the most recent events.</span></span>

### <a name="run-the-app-in-the-kudu-console"></a><span data-ttu-id="e9ca3-248">Exécuter l’application dans la console Kudu</span><span class="sxs-lookup"><span data-stu-id="e9ca3-248">Run the app in the Kudu console</span></span>

<span data-ttu-id="e9ca3-249">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-249">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="e9ca3-250">Vous pouvez exécuter l’application dans la console d’exécution à distance [Kudu](https://github.com/projectkudu/kudu/wiki) pour détecter l’erreur :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-250">You can run the app in the [Kudu](https://github.com/projectkudu/kudu/wiki) Remote Execution Console to discover the error:</span></span>

1. <span data-ttu-id="e9ca3-251">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-251">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="e9ca3-252">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="e9ca3-252">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="e9ca3-253">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-253">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="e9ca3-254">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-254">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>

#### <a name="test-a-32-bit-x86-app"></a><span data-ttu-id="e9ca3-255">Tester une application 32 bits (x86)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-255">Test a 32-bit (x86) app</span></span>

<span data-ttu-id="e9ca3-256">**Version actuelle**</span><span class="sxs-lookup"><span data-stu-id="e9ca3-256">**Current release**</span></span>

1. `cd d:\home\site\wwwroot`
1. <span data-ttu-id="e9ca3-257">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-257">Run the app:</span></span>
   * <span data-ttu-id="e9ca3-258">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-258">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

     ```console
     dotnet .\{ASSEMBLY NAME}.dll
     ```

   * <span data-ttu-id="e9ca3-259">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-259">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

     ```console
     {ASSEMBLY NAME}.exe
     ```

<span data-ttu-id="e9ca3-260">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-260">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="e9ca3-261">**Déploiement dépendant du Framework s’exécutant sur une version préliminaire**</span><span class="sxs-lookup"><span data-stu-id="e9ca3-261">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="e9ca3-262">*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x86) requise.*</span><span class="sxs-lookup"><span data-stu-id="e9ca3-262">*Requires installing the ASP.NET Core {VERSION} (x86) Runtime site extension.*</span></span>

1. <span data-ttu-id="e9ca3-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` est la version du runtime).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-263">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x32` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="e9ca3-264">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-264">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="e9ca3-265">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-265">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

#### <a name="test-a-64-bit-x64-app"></a><span data-ttu-id="e9ca3-266">Tester une application 64 bits (x64)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-266">Test a 64-bit (x64) app</span></span>

<span data-ttu-id="e9ca3-267">**Version actuelle**</span><span class="sxs-lookup"><span data-stu-id="e9ca3-267">**Current release**</span></span>

* <span data-ttu-id="e9ca3-268">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) 64 bits (x64) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-268">If the app is a 64-bit (x64) [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>
  1. `cd D:\Program Files\dotnet`
  1. <span data-ttu-id="e9ca3-269">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-269">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>
* <span data-ttu-id="e9ca3-270">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-270">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>
  1. `cd D:\home\site\wwwroot`
  1. <span data-ttu-id="e9ca3-271">Exécutez l'application : `{ASSEMBLY NAME}.exe`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-271">Run the app: `{ASSEMBLY NAME}.exe`</span></span>

<span data-ttu-id="e9ca3-272">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-272">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

<span data-ttu-id="e9ca3-273">**Déploiement dépendant du Framework s’exécutant sur une version préliminaire**</span><span class="sxs-lookup"><span data-stu-id="e9ca3-273">**Framework-dependent deployment running on a preview release**</span></span>

<span data-ttu-id="e9ca3-274">*Installation de l’extension de site du runtime ASP.NET Core {VERSION} (x64) requise.*</span><span class="sxs-lookup"><span data-stu-id="e9ca3-274">*Requires installing the ASP.NET Core {VERSION} (x64) Runtime site extension.*</span></span>

1. <span data-ttu-id="e9ca3-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` est la version du runtime).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-275">`cd D:\home\SiteExtensions\AspNetCoreRuntime.{X.Y}.x64` (`{X.Y}` is the runtime version)</span></span>
1. <span data-ttu-id="e9ca3-276">Exécutez l'application : `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-276">Run the app: `dotnet \home\site\wwwroot\{ASSEMBLY NAME}.dll`</span></span>

<span data-ttu-id="e9ca3-277">La sortie de console de l’application, affichant toutes les erreurs éventuelles, est transmise à la console Kudu.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-277">The console output from the app, showing any errors, is piped to the Kudu console.</span></span>

### <a name="aspnet-core-module-stdout-log-azure-app-service"></a><span data-ttu-id="e9ca3-278">Journal stdout du module ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-278">ASP.NET Core Module stdout log (Azure App Service)</span></span>

<span data-ttu-id="e9ca3-279">Le journal stdout du module ASP.NET Core enregistre souvent des messages d’erreur utiles et absents du Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-279">The ASP.NET Core Module stdout log often records useful error messages not found in the Application Event Log.</span></span> <span data-ttu-id="e9ca3-280">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-280">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="e9ca3-281">Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-281">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="e9ca3-282">Sous **SÉLECTIONNER UNE CATÉGORIE DE PROBLÈME**, sélectionnez le bouton **Application web en panne**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-282">Under **SELECT PROBLEM CATEGORY**, select the **Web App Down** button.</span></span>
1. <span data-ttu-id="e9ca3-283">Sous **Solutions suggérées** > **Activer la redirection de journaux stdout**, sélectionnez le bouton **Ouvrir la console Kudu pour modifier web.config**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-283">Under **Suggested Solutions** > **Enable Stdout Log Redirection**, select the button to **Open Kudu Console to edit Web.Config**.</span></span>
1. <span data-ttu-id="e9ca3-284">Dans la **Console de diagnostic** Kudu, ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-284">In the Kudu **Diagnostic Console**, open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="e9ca3-285">Faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-285">Scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="e9ca3-286">Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-286">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="e9ca3-287">Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-287">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="e9ca3-288">Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-288">Select **Save** to save the updated *web.config* file.</span></span>
1. <span data-ttu-id="e9ca3-289">Adressez une demande à l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-289">Make a request to the app.</span></span>
1. <span data-ttu-id="e9ca3-290">Revenez sur le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-290">Return to the Azure portal.</span></span> <span data-ttu-id="e9ca3-291">Sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-291">Select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="e9ca3-292">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="e9ca3-292">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="e9ca3-293">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-293">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="e9ca3-294">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-294">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="e9ca3-295">Sélectionnez le dossier **LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-295">Select the **LogFiles** folder.</span></span>
1. <span data-ttu-id="e9ca3-296">Inspectez la colonne **Modifié** et sélectionnez l’icône en forme de crayon pour modifier le journal stdout avec la date de dernière modification.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-296">Inspect the **Modified** column and select the pencil icon to edit the stdout log with the latest modification date.</span></span>
1. <span data-ttu-id="e9ca3-297">Lorsque le fichier journal s’ouvre, l’erreur s’affiche.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-297">When the log file opens, the error is displayed.</span></span>

<span data-ttu-id="e9ca3-298">Désactivez la journalisation stdout, une fois les problèmes résolus :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-298">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="e9ca3-299">Dans la **Console de diagnostic** Kudu, revenez au chemin d’accès **site** > **wwwroot** pour faire apparaître le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-299">In the Kudu **Diagnostic Console**, return to the path **site** > **wwwroot** to reveal the *web.config* file.</span></span> <span data-ttu-id="e9ca3-300">Ouvrez à nouveau le fichier **web.config** en sélectionnant l’icône en forme de crayon.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-300">Open the **web.config** file again by selecting the pencil icon.</span></span>
1. <span data-ttu-id="e9ca3-301">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-301">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="e9ca3-302">Sélectionnez **Enregistrer** pour enregistrer le fichier.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-302">Select **Save** to save the file.</span></span>

<span data-ttu-id="e9ca3-303">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-303">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="e9ca3-304">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-304">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="e9ca3-305">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-305">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="e9ca3-306">N’utilisez la journalisation stdout que pour résoudre les problèmes de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-306">Only use stdout logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="e9ca3-307">Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-307">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e9ca3-308">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-308">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-azure-app-service"></a><span data-ttu-id="e9ca3-309">Journal de débogage du module ASP.NET Core (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-309">ASP.NET Core Module debug log (Azure App Service)</span></span>

<span data-ttu-id="e9ca3-310">Le journal de débogage du module ASP.NET Core fournit une journalisation supplémentaire, plus approfondie, à partir du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-310">The ASP.NET Core Module debug log provides additional, deeper logging from the ASP.NET Core Module.</span></span> <span data-ttu-id="e9ca3-311">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-311">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="e9ca3-312">Pour activer le journal de diagnostic amélioré, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-312">To enable the enhanced diagnostic log, perform either of the following:</span></span>
   * <span data-ttu-id="e9ca3-313">Suivez les instructions indiquées dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) afin de configurer l’application pour une journalisation des diagnostics améliorée.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-313">Follow the instructions in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to configure the app for an enhanced diagnostic logging.</span></span> <span data-ttu-id="e9ca3-314">Redéployez l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-314">Redeploy the app.</span></span>
   * <span data-ttu-id="e9ca3-315">Ajoutez le `<handlerSettings>` présenté dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) au fichier *web.config* de l’application en production à l’aide de la console Kudu :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-315">Add the `<handlerSettings>` shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) to the live app's *web.config* file using the Kudu console:</span></span>
     1. <span data-ttu-id="e9ca3-316">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-316">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="e9ca3-317">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="e9ca3-317">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="e9ca3-318">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-318">The Kudu console opens in a new browser tab or window.</span></span>
     1. <span data-ttu-id="e9ca3-319">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-319">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
     1. <span data-ttu-id="e9ca3-320">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-320">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="e9ca3-321">Modifiez le fichier *web.config* en sélectionnant le bouton représentant un crayon.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-321">Edit the *web.config* file by selecting the pencil button.</span></span> <span data-ttu-id="e9ca3-322">Ajoutez la section `<handlerSettings>` comme indiqué dans [Journaux de diagnostic améliorés](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-322">Add the `<handlerSettings>` section as shown in [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="e9ca3-323">Sélectionnez le bouton **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-323">Select the **Save** button.</span></span>
1. <span data-ttu-id="e9ca3-324">Ouvrez les **Outils avancés** dans la zone **Outils de développement**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-324">Open **Advanced Tools** in the **Development Tools** area.</span></span> <span data-ttu-id="e9ca3-325">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="e9ca3-325">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="e9ca3-326">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-326">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="e9ca3-327">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-327">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="e9ca3-328">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-328">Open the folders to the path **site** > **wwwroot**.</span></span> <span data-ttu-id="e9ca3-329">Si vous n’avez pas indiqué de chemin pour le fichier *aspnetcore-debug.log*, le fichier apparaît dans la liste.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-329">If you didn't supply a path for the *aspnetcore-debug.log* file, the file appears in the list.</span></span> <span data-ttu-id="e9ca3-330">Si vous avez indiqué un chemin, accédez à l’emplacement du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-330">If you supplied a path, navigate to the location of the log file.</span></span>
1. <span data-ttu-id="e9ca3-331">Ouvrez le fichier journal à l’aide du bouton représentant un crayon à côté du nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-331">Open the log file with the pencil button next to the file name.</span></span>

<span data-ttu-id="e9ca3-332">Désactivez la journalisation du débogage, une fois la résolution des problèmes effectuée :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-332">Disable debug logging when troubleshooting is complete:</span></span>

<span data-ttu-id="e9ca3-333">Pour désactiver la journalisation de débogage améliorée, effectuez l’une des opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-333">To disable the enhanced debug log, perform either of the following:</span></span>

* <span data-ttu-id="e9ca3-334">Supprimez `<handlerSettings>` du fichier *web.config* localement, puis redéployez l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-334">Remove the `<handlerSettings>` from the *web.config* file locally and redeploy the app.</span></span>
* <span data-ttu-id="e9ca3-335">Utilisez la console Kudu pour modifier le fichier *web.config* et supprimer la section `<handlerSettings>`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-335">Use the Kudu console to edit the *web.config* file and remove the `<handlerSettings>` section.</span></span> <span data-ttu-id="e9ca3-336">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-336">Save the file.</span></span>

<span data-ttu-id="e9ca3-337">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-337">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

> [!WARNING]
> <span data-ttu-id="e9ca3-338">Si le journal de débogage n’est pas désactivé, cela peut entraîner une défaillance de l’application ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-338">Failure to disable the debug log can lead to app or server failure.</span></span> <span data-ttu-id="e9ca3-339">Il n’existe aucune limite à la taille du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-339">There's no limit on log file size.</span></span> <span data-ttu-id="e9ca3-340">Utilisez uniquement la journalisation de débogage pour résoudre les problèmes de démarrage d’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-340">Only use debug logging to troubleshoot app startup problems.</span></span>
>
> <span data-ttu-id="e9ca3-341">Pour la journalisation générale dans une application ASP.NET Core après le démarrage, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-341">For general logging in an ASP.NET Core app after startup, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e9ca3-342">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-342">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker-end

### <a name="slow-or-hanging-app-azure-app-service"></a><span data-ttu-id="e9ca3-343">Application lente ou suspendue (Azure App Service)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-343">Slow or hanging app (Azure App Service)</span></span>

<span data-ttu-id="e9ca3-344">Si une application répond lentement ou se bloque sur une requête, voir les articles suivants :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-344">When an app responds slowly or hangs on a request, see the following articles:</span></span>

* [<span data-ttu-id="e9ca3-345">Résoudre les problèmes de performances d’une application web lente dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e9ca3-345">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="e9ca3-346">Utiliser l’extension de site Outil de diagnostic des incidents pour capturer une image mémoire des problèmes d’exception intermittente ou de performances sur Azure Web App</span><span class="sxs-lookup"><span data-stu-id="e9ca3-346">Use Crash Diagnoser Site Extension to Capture Dump for Intermittent Exception issues or performance issues on Azure Web App</span></span>](https://blogs.msdn.microsoft.com/asiatech/2015/12/28/use-crash-diagnoser-site-extension-to-capture-dump-for-intermittent-exception-issues-or-performance-issues-on-azure-web-app/)

### <a name="monitoring-blades"></a><span data-ttu-id="e9ca3-347">Panneaux Monitoring</span><span class="sxs-lookup"><span data-stu-id="e9ca3-347">Monitoring blades</span></span>

<span data-ttu-id="e9ca3-348">Les panneaux Monitoring offrent une expérience de résolution des erreurs différente des méthodes décrites précédemment dans la rubrique.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-348">Monitoring blades provide an alternative troubleshooting experience to the methods described earlier in the topic.</span></span> <span data-ttu-id="e9ca3-349">Ils peuvent servir à diagnostiquer les erreurs de la série 500.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-349">These blades can be used to diagnose 500-series errors.</span></span>

<span data-ttu-id="e9ca3-350">Vérifiez que les extensions ASP.NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-350">Confirm that the ASP.NET Core Extensions are installed.</span></span> <span data-ttu-id="e9ca3-351">Si ce n’est pas le cas, installez-les manuellement :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-351">If the extensions aren't installed, install them manually:</span></span>

1. <span data-ttu-id="e9ca3-352">Dans la section du panneau **OUTILS DE DÉVELOPPEMENT**, sélectionnez le panneau **Extensions**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-352">In the **DEVELOPMENT TOOLS** blade section, select the **Extensions** blade.</span></span>
1. <span data-ttu-id="e9ca3-353">Les **Extensions ASP.NET Core** devraient apparaître dans la liste.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-353">The **ASP.NET Core Extensions** should appear in the list.</span></span>
1. <span data-ttu-id="e9ca3-354">Si les extensions ne sont pas installées, sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-354">If the extensions aren't installed, select the **Add** button.</span></span>
1. <span data-ttu-id="e9ca3-355">Choisissez les **Extensions ASP.NET Core** dans la liste.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-355">Choose the **ASP.NET Core Extensions** from the list.</span></span>
1. <span data-ttu-id="e9ca3-356">Sélectionnez **OK** pour accepter les conditions légales.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-356">Select **OK** to accept the legal terms.</span></span>
1. <span data-ttu-id="e9ca3-357">Sélectionnez **OK** sur le panneau **Ajouter une extension**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-357">Select **OK** on the **Add extension** blade.</span></span>
1. <span data-ttu-id="e9ca3-358">Un message contextuel d’information indique que les extensions sont bien installées.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-358">An informational pop-up message indicates when the extensions are successfully installed.</span></span>

<span data-ttu-id="e9ca3-359">Si la journalisation stdout n’est pas activée, suivez les étapes ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-359">If stdout logging isn't enabled, follow these steps:</span></span>

1. <span data-ttu-id="e9ca3-360">Sur le Portail Azure, sélectionnez le panneau **Outils avancés** dans la zone **OUTILS DE DÉVELOPPEMENT**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-360">In the Azure portal, select the **Advanced Tools** blade in the **DEVELOPMENT TOOLS** area.</span></span> <span data-ttu-id="e9ca3-361">Sélectionnez le bouton **Atteindre&rarr;** .</span><span class="sxs-lookup"><span data-stu-id="e9ca3-361">Select the **Go&rarr;** button.</span></span> <span data-ttu-id="e9ca3-362">La console Kudu s’ouvre dans un nouvel onglet ou une nouvelle fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-362">The Kudu console opens in a new browser tab or window.</span></span>
1. <span data-ttu-id="e9ca3-363">Dans la barre de navigation en haut de la page, ouvrez **Console de débogage** et sélectionnez **CMD**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-363">Using the navigation bar at the top of the page, open **Debug console** and select **CMD**.</span></span>
1. <span data-ttu-id="e9ca3-364">Ouvrez les dossiers sur le chemin d’accès **site** > **wwwroot** et faites défiler la page jusqu’à révéler le fichier *web.config* en bas de la liste.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-364">Open the folders to the path **site** > **wwwroot** and scroll down to reveal the *web.config* file at the bottom of the list.</span></span>
1. <span data-ttu-id="e9ca3-365">Cliquez sur l’icône en forme de crayon à côté du fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-365">Click the pencil icon next to the *web.config* file.</span></span>
1. <span data-ttu-id="e9ca3-366">Définissez **stdoutLogEnabled** sur `true` et remplacez le chemin **stdoutLogFile** par : `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-366">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to: `\\?\%home%\LogFiles\stdout`.</span></span>
1. <span data-ttu-id="e9ca3-367">Sélectionnez **Enregistrer** pour enregistrer le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-367">Select **Save** to save the updated *web.config* file.</span></span>

<span data-ttu-id="e9ca3-368">Maintenant, activez la journalisation des diagnostics :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-368">Proceed to activate diagnostic logging:</span></span>

1. <span data-ttu-id="e9ca3-369">Sur le Portail Azure, sélectionnez le panneau **Journaux de diagnostic**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-369">In the Azure portal, select the **Diagnostics logs** blade.</span></span>
1. <span data-ttu-id="e9ca3-370">Basculez **Journalisation des applications (système de fichiers)** et **Messages d’erreur détaillés** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-370">Select the **On** switch for **Application Logging (Filesystem)** and **Detailed error messages**.</span></span> <span data-ttu-id="e9ca3-371">Sélectionnez le bouton **Enregistrer** en haut du panneau.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-371">Select the **Save** button at the top of the blade.</span></span>
1. <span data-ttu-id="e9ca3-372">Pour inclure le suivi des demandes ayant échoué, également appelé Mise en mémoire tampon des événements d’échec des demandes (FREB), basculez **Suivi des demandes ayant échoué** sur **Activé**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-372">To include failed request tracing, also known as Failed Request Event Buffering (FREB) logging, select the **On** switch for **Failed request tracing**.</span></span>
1. <span data-ttu-id="e9ca3-373">Sélectionnez le panneau **Flux du journal** situé juste sous le panneau **Journaux de diagnostic** sur le portail.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-373">Select the **Log stream** blade, which is listed immediately under the **Diagnostics logs** blade in the portal.</span></span>
1. <span data-ttu-id="e9ca3-374">Adressez une demande à l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-374">Make a request to the app.</span></span>
1. <span data-ttu-id="e9ca3-375">La cause de l’erreur est indiquée dans les données de flux de journal.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-375">Within the log stream data, the cause of the error is indicated.</span></span>

<span data-ttu-id="e9ca3-376">Veillez à désactiver la journalisation stdout une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-376">Be sure to disable stdout logging when troubleshooting is complete.</span></span>

<span data-ttu-id="e9ca3-377">Pour afficher les journaux de suivi des demandes ayant échoué (journaux FREB) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-377">To view the failed request tracing logs (FREB logs):</span></span>

1. <span data-ttu-id="e9ca3-378">Accédez au panneau **Diagnostiquer et résoudre les problèmes** du Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-378">Navigate to the **Diagnose and solve problems** blade in the Azure portal.</span></span>
1. <span data-ttu-id="e9ca3-379">Sélectionnez **Journaux de suivi des demandes ayant échoué** dans la zone **OUTILS DE PRISE EN CHARGE** de la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-379">Select **Failed Request Tracing Logs** from the **SUPPORT TOOLS** area of the sidebar.</span></span>

<span data-ttu-id="e9ca3-380">Voir la [section Traces des demandes ayant échoué de la rubrique Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) et [FAQ sur les performances des applications web dans Azure : comment activer le suivi des demandes ayant échoué ?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-380">See [Failed request traces section of the Enable diagnostics logging for web apps in Azure App Service topic](/azure/app-service/web-sites-enable-diagnostic-log#failed-request-traces) and the [Application performance FAQs for Web Apps in Azure: How do I turn on failed request tracing?](/azure/app-service/app-service-web-availability-performance-application-issues-faq#how-do-i-turn-on-failed-request-tracing) for more information.</span></span>

<span data-ttu-id="e9ca3-381">Pour plus d’informations, voir [Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-381">For more information, see [Enable diagnostics logging for web apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log).</span></span>

> [!WARNING]
> <span data-ttu-id="e9ca3-382">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-382">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="e9ca3-383">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-383">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="e9ca3-384">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-384">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e9ca3-385">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-385">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="troubleshoot-on-iis"></a><span data-ttu-id="e9ca3-386">Résoudre les problèmes sur IIS</span><span class="sxs-lookup"><span data-stu-id="e9ca3-386">Troubleshoot on IIS</span></span>

### <a name="application-event-log-iis"></a><span data-ttu-id="e9ca3-387">Journal des événements d’application (IIS)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-387">Application Event Log (IIS)</span></span>

<span data-ttu-id="e9ca3-388">Accédez au Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-388">Access the Application Event Log:</span></span>

1. <span data-ttu-id="e9ca3-389">Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez l’application **Observateur d’événements**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-389">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="e9ca3-390">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-390">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="e9ca3-391">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-391">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="e9ca3-392">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-392">Search for errors associated with the failing app.</span></span> <span data-ttu-id="e9ca3-393">Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-393">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="e9ca3-394">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="e9ca3-394">Run the app at a command prompt</span></span>

<span data-ttu-id="e9ca3-395">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-395">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="e9ca3-396">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-396">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="e9ca3-397">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="e9ca3-397">Framework-dependent deployment</span></span>

<span data-ttu-id="e9ca3-398">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-398">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="e9ca3-399">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-399">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="e9ca3-400">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-400">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="e9ca3-401">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-401">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="e9ca3-402">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-402">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="e9ca3-403">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-403">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="e9ca3-404">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-404">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="e9ca3-405">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="e9ca3-405">Self-contained deployment</span></span>

<span data-ttu-id="e9ca3-406">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-406">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="e9ca3-407">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-407">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="e9ca3-408">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-408">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="e9ca3-409">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-409">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="e9ca3-410">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-410">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="e9ca3-411">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-411">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="e9ca3-412">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-412">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log-iis"></a><span data-ttu-id="e9ca3-413">Journal stdout du module ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-413">ASP.NET Core Module stdout log (IIS)</span></span>

<span data-ttu-id="e9ca3-414">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-414">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="e9ca3-415">Accédez au dossier de déploiement du site sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-415">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="e9ca3-416">Si le dossier *logs* n’est pas présent, créez-le.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-416">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="e9ca3-417">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-417">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="e9ca3-418">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-418">Edit the *web.config* file.</span></span> <span data-ttu-id="e9ca3-419">Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-419">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="e9ca3-420">Dans le chemin, `stdout` est le préfixe du nom du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-420">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="e9ca3-421">Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-421">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="e9ca3-422">Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-422">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="e9ca3-423">Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-423">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="e9ca3-424">Enregistrez le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-424">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="e9ca3-425">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-425">Make a request to the app.</span></span>
1. <span data-ttu-id="e9ca3-426">Accédez au dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-426">Navigate to the *logs* folder.</span></span> <span data-ttu-id="e9ca3-427">Recherchez et ouvrez le journal stdout le plus récent.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-427">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="e9ca3-428">Déterminez si le journal contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-428">Study the log for errors.</span></span>

<span data-ttu-id="e9ca3-429">Désactivez la journalisation stdout, une fois les problèmes résolus :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-429">Disable stdout logging when troubleshooting is complete:</span></span>

1. <span data-ttu-id="e9ca3-430">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-430">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="e9ca3-431">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-431">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="e9ca3-432">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-432">Save the file.</span></span>

<span data-ttu-id="e9ca3-433">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-433">For more information, see <xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection>.</span></span>

> [!WARNING]
> <span data-ttu-id="e9ca3-434">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-434">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="e9ca3-435">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-435">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="e9ca3-436">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-436">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e9ca3-437">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-437">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="aspnet-core-module-debug-log-iis"></a><span data-ttu-id="e9ca3-438">Journal de débogage du module ASP.NET Core (IIS)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-438">ASP.NET Core Module debug log (IIS)</span></span>

<span data-ttu-id="e9ca3-439">Ajoutez les paramètres de gestionnaire suivants au fichier *Web. config* de l’application pour activer ASP.net Core journal de débogage du module:</span><span class="sxs-lookup"><span data-stu-id="e9ca3-439">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug log:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="e9ca3-440">Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-440">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

<span data-ttu-id="e9ca3-441">Pour plus d'informations, consultez <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-441">For more information, see <xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs>.</span></span>

::: moniker-end

### <a name="enable-the-developer-exception-page"></a><span data-ttu-id="e9ca3-442">Afficher la page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="e9ca3-442">Enable the Developer Exception Page</span></span>

<span data-ttu-id="e9ca3-443">Vous pouvez ajouter la [variable d’environnement `ASPNETCORE_ENVIRONMENT` au fichier web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-443">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="e9ca3-444">Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-444">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="e9ca3-445">La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-445">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="e9ca3-446">Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-446">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="e9ca3-447">Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-447">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

### <a name="obtain-data-from-an-app"></a><span data-ttu-id="e9ca3-448">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="e9ca3-448">Obtain data from an app</span></span>

<span data-ttu-id="e9ca3-449">Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-449">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="e9ca3-450">Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-450">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

### <a name="slow-or-hanging-app-iis"></a><span data-ttu-id="e9ca3-451">Application lente ou bloquée (IIS)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-451">Slow or hanging app (IIS)</span></span>

<span data-ttu-id="e9ca3-452">Un *vidage sur incident* est un instantané de la mémoire du système et peut aider à déterminer la cause d’un incident d’application, d’un échec de démarrage ou d’une application lente.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-452">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="e9ca3-453">L’application cesse de fonctionner ou rencontre une exception</span><span class="sxs-lookup"><span data-stu-id="e9ca3-453">App crashes or encounters an exception</span></span>

<span data-ttu-id="e9ca3-454">Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-454">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="e9ca3-455">Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-455">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="e9ca3-456">Le pool d’application doit disposer des accès d’écriture dans le dossier.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-456">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="e9ca3-457">Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-457">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="e9ca3-458">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-458">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="e9ca3-459">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-459">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="e9ca3-460">Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-460">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="e9ca3-461">Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-461">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/test/troubleshoot-azure-iis/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="e9ca3-462">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-462">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="e9ca3-463">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-463">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="e9ca3-464">Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-464">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="e9ca3-465">Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-465">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="e9ca3-466">Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-466">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="e9ca3-467">L’application se bloque, ne démarre pas ou s’exécute normalement</span><span class="sxs-lookup"><span data-stu-id="e9ca3-467">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="e9ca3-468">Lorsqu’une application *se bloque* (ne répond plus mais ne s’arrête pas), ne démarre pas ou s’exécute normalement, consultez [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Fichiers dump en mode utilisateur : choisir le meilleur outil) afin de sélectionner l’outil adapté pour produire le fichier dump.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-468">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="e9ca3-469">Analyser le fichier dump</span><span class="sxs-lookup"><span data-stu-id="e9ca3-469">Analyze the dump</span></span>

<span data-ttu-id="e9ca3-470">Un fichier dump peut être analysé à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-470">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="e9ca3-471">Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-471">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="clear-package-caches"></a><span data-ttu-id="e9ca3-472">Effacer les caches de package</span><span class="sxs-lookup"><span data-stu-id="e9ca3-472">Clear package caches</span></span>

<span data-ttu-id="e9ca3-473">Parfois, une application opérationnelle échoue immédiatement après la mise à niveau de la kit SDK .NET Core sur l’ordinateur de développement ou de la modification des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-473">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="e9ca3-474">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-474">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="e9ca3-475">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e9ca3-475">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="e9ca3-476">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-476">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="e9ca3-477">Effacez les caches de package en `dotnet nuget locals all --clear` exécutant à partir d’une interface de commande.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-477">Clear the package caches by executing `dotnet nuget locals all --clear` from a command shell.</span></span>

   <span data-ttu-id="e9ca3-478">L’effacement des caches de package peut également être effectué à l’aide de l’outil [NuGet. exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-478">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="e9ca3-479">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="e9ca3-479">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="e9ca3-480">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-480">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="e9ca3-481">Supprimez tous les fichiers du dossier de déploiement sur le serveur avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="e9ca3-481">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9ca3-482">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e9ca3-482">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/aspnet-core-module>

### <a name="azure-documentation"></a><span data-ttu-id="e9ca3-483">Documentation Azure</span><span class="sxs-lookup"><span data-stu-id="e9ca3-483">Azure documentation</span></span>

* [<span data-ttu-id="e9ca3-484">Application Insights pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9ca3-484">Application Insights for ASP.NET Core</span></span>](/azure/application-insights/app-insights-asp-net-core)
* [<span data-ttu-id="e9ca3-485">Section débogage à distance des applications Web de dépanner une application Web dans Azure App Service à l’aide de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9ca3-485">Remote debugging web apps section of Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)
* [<span data-ttu-id="e9ca3-486">Vue d’ensemble des diagnostics Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e9ca3-486">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)
* [<span data-ttu-id="e9ca3-487">Guide pratique pour surveiller des applications dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e9ca3-487">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)
* [<span data-ttu-id="e9ca3-488">Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9ca3-488">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="e9ca3-489">Résoudre les erreurs HTTP « 502 Passerelle incorrecte » et « 503 Service non disponible » dans des applications web Azure</span><span class="sxs-lookup"><span data-stu-id="e9ca3-489">Troubleshoot HTTP errors of "502 bad gateway" and "503 service unavailable" in your Azure web apps</span></span>](/azure/app-service/app-service-web-troubleshoot-http-502-http-503)
* [<span data-ttu-id="e9ca3-490">Résoudre les problèmes de performances d’une application web lente dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e9ca3-490">Troubleshoot slow web app performance issues in Azure App Service</span></span>](/azure/app-service/app-service-web-troubleshoot-performance-degradation)
* [<span data-ttu-id="e9ca3-491">FAQ sur les performances des applications web dans Azure</span><span class="sxs-lookup"><span data-stu-id="e9ca3-491">Application performance FAQs for Web Apps in Azure</span></span>](/azure/app-service/app-service-web-availability-performance-application-issues-faq)
* [<span data-ttu-id="e9ca3-492">Bac à sable des applications web Azure (limitations de l’exécution du runtime App Service)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-492">Azure Web App sandbox (App Service runtime execution limitations)</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)
* [<span data-ttu-id="e9ca3-493">Azure Friday : Azure App Service Diagnostic and Troubleshooting Experience (vidéo de 12 minutes)</span><span class="sxs-lookup"><span data-stu-id="e9ca3-493">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)

### <a name="visual-studio-documentation"></a><span data-ttu-id="e9ca3-494">Documentation de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9ca3-494">Visual Studio documentation</span></span>

* [<span data-ttu-id="e9ca3-495">Débogage à distance ASP.NET Core sur IIS dans Azure dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e9ca3-495">Remote Debug ASP.NET Core on IIS in Azure in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-azure)
* [<span data-ttu-id="e9ca3-496">Débogage à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e9ca3-496">Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017</span></span>](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer)
* [<span data-ttu-id="e9ca3-497">Apprendre à déboguer avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e9ca3-497">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)

### <a name="visual-studio-code-documentation"></a><span data-ttu-id="e9ca3-498">Documentation Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e9ca3-498">Visual Studio Code documentation</span></span>

* [<span data-ttu-id="e9ca3-499">Débogage avec Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e9ca3-499">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)
