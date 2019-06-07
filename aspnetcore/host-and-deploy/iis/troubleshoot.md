---
title: Résoudre les problèmes liés à ASP.NET Core sur IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Internet Information Services (IIS) d’applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/12/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: e4c93459f2030c7c0a55ea90e0cc8c8d30b76c51
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470463"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="e2952-103">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="e2952-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="e2952-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e2952-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e2952-105">Cet article fournit des instructions sur la façon de diagnostiquer un problème de démarrage d’application ASP.NET Core dans le cas d’un hébergement avec [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="e2952-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="e2952-106">Les informations contenues dans cet article s’appliquent à l’hébergement dans IIS sur Windows Server et Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="e2952-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="e2952-107">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="e2952-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="e2952-108">Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* ou un *échec de démarrage 500.30* qui se produit pendant un débogage local.</span><span class="sxs-lookup"><span data-stu-id="e2952-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="e2952-109">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="e2952-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="e2952-110">Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* qui se produit pendant un débogage local.</span><span class="sxs-lookup"><span data-stu-id="e2952-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="e2952-111">Rubriques de dépannage supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="e2952-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="e2952-112"><xref:host-and-deploy/azure-apps/troubleshoot> Bien qu’App Service utilise le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et IIS pour héberger des applications, consultez la rubrique dédiée pour obtenir des instructions qui concernent spécifiquement App Service.</span><span class="sxs-lookup"><span data-stu-id="e2952-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="e2952-113"><xref:fundamentals/error-handling> Découvrez comment gérer les erreurs dans les applications ASP.NET Core pendant le développement sur un système local.</span><span class="sxs-lookup"><span data-stu-id="e2952-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="e2952-114">[Apprendre à déboguer avec Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) Cette rubrique présente les fonctionnalités du débogueur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2952-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="e2952-115">[Débogage avec Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Découvrez la prise en charge du débogage intégrée à Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e2952-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="e2952-116">Erreurs de démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="e2952-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="e2952-117">Échec de processus 502.5</span><span class="sxs-lookup"><span data-stu-id="e2952-117">502.5 Process Failure</span></span>

<span data-ttu-id="e2952-118">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-118">The worker process fails.</span></span> <span data-ttu-id="e2952-119">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-119">The app doesn't start.</span></span>

<span data-ttu-id="e2952-120">Le module ASP.NET Core tente, en vain, de démarrer le processus dotnet backend.</span><span class="sxs-lookup"><span data-stu-id="e2952-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="e2952-121">Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="e2952-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="e2952-122">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="e2952-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="e2952-123">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="e2952-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="e2952-124">Le *framework partagé* est le jeu d’assemblys (fichiers *.dll*) qui sont installés sur l’ordinateur et référencés par un métapaquet comme `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e2952-124">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="e2952-125">La référence de métapaquet peut spécifier une version minimale requise.</span><span class="sxs-lookup"><span data-stu-id="e2952-125">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="e2952-126">Pour plus d’informations, consultez [Le framework partagé](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="e2952-126">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="e2952-127">La page d’erreur d’un *échec de processus 502.5* est retournée quand une erreur de configuration d’hébergement ou d’application entraîne l’échec du processus de travail :</span><span class="sxs-lookup"><span data-stu-id="e2952-127">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Fenêtre de navigateur affichant la page d’échec de processus 502.5](troubleshoot/_static/process-failure-page.png)

::: moniker range="= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="e2952-129">500.30 - Échec du démarrage in-process</span><span class="sxs-lookup"><span data-stu-id="e2952-129">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="e2952-130">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-130">The worker process fails.</span></span> <span data-ttu-id="e2952-131">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-131">The app doesn't start.</span></span>

<span data-ttu-id="e2952-132">Le module ASP.NET Core tente, en vain, de démarrer le CLR .NET Core in-process.</span><span class="sxs-lookup"><span data-stu-id="e2952-132">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="e2952-133">Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="e2952-133">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="e2952-134">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="e2952-134">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="e2952-135">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="e2952-135">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="e2952-136">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="e2952-136">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="e2952-137">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-137">The worker process fails.</span></span> <span data-ttu-id="e2952-138">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-138">The app doesn't start.</span></span>

<span data-ttu-id="e2952-139">Le module ASP.NET Core ne peut pas trouver le CLR .NET Core et le gestionnaire de requêtes in-process (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="e2952-139">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="e2952-140">Vérifiez que :</span><span class="sxs-lookup"><span data-stu-id="e2952-140">Check that:</span></span>

* <span data-ttu-id="e2952-141">l’application cible le package NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) ou le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ;</span><span class="sxs-lookup"><span data-stu-id="e2952-141">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="e2952-142">la version du framework partagé ASP.NET Core que l’application cible est installée sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="e2952-142">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="e2952-143">500.0 - Échec de chargement du gestionnaire out-of-process</span><span class="sxs-lookup"><span data-stu-id="e2952-143">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="e2952-144">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-144">The worker process fails.</span></span> <span data-ttu-id="e2952-145">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-145">The app doesn't start.</span></span>

<span data-ttu-id="e2952-146">Le module ASP.NET Core ne peut pas trouver le gestionnaire de requêtes d’hébergement out-of-process.</span><span class="sxs-lookup"><span data-stu-id="e2952-146">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="e2952-147">Vérifiez que le fichier *aspnetcorev2_outofprocess.dll* est présent dans un sous-dossier en regard de *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="e2952-147">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="e2952-148">500.31 - Échec de la recherche de dépendances natives par ANCM</span><span class="sxs-lookup"><span data-stu-id="e2952-148">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="e2952-149">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-149">The worker process fails.</span></span> <span data-ttu-id="e2952-150">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-150">The app doesn't start.</span></span>

<span data-ttu-id="e2952-151">Le module ASP.NET Core tente de démarrer le runtime .NET Core in-process, mais sans succès.</span><span class="sxs-lookup"><span data-stu-id="e2952-151">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="e2952-152">La cause la plus courante de cet échec de démarrage est liée à la non-installation du runtime `Microsoft.NETCore.App` ou `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e2952-152">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="e2952-153">Si l’application est déployée pour cibler ASP.NET Core 3.0, et si cette version n’existe pas sur la machine, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="e2952-153">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="e2952-154">Voici un exemple de message d’erreur :</span><span class="sxs-lookup"><span data-stu-id="e2952-154">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="e2952-155">Le message d’erreur liste toutes les versions installées de .NET Core ainsi que la version demandée par l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-155">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="e2952-156">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="e2952-156">To fix this error, either:</span></span>

* <span data-ttu-id="e2952-157">Installez la version appropriée de .NET Core sur la machine.</span><span class="sxs-lookup"><span data-stu-id="e2952-157">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="e2952-158">Changez l’application pour cibler une version de .NET Core présente sur la machine.</span><span class="sxs-lookup"><span data-stu-id="e2952-158">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="e2952-159">Publiez l’application sous forme de [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="e2952-159">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="e2952-160">Durant l’exécution en phase de développement (la variable d’environnement `ASPNETCORE_ENVIRONMENT` a la valeur `Development`), l’erreur spécifique est écrite dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="e2952-160">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="e2952-161">La cause d’un échec de démarrage du processus se trouve également dans le [Journal des événements de l’application](#application-event-log).</span><span class="sxs-lookup"><span data-stu-id="e2952-161">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="e2952-162">500.32 - Échec du chargement de la dll par ANCM</span><span class="sxs-lookup"><span data-stu-id="e2952-162">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="e2952-163">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-163">The worker process fails.</span></span> <span data-ttu-id="e2952-164">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-164">The app doesn't start.</span></span>

<span data-ttu-id="e2952-165">Le plus souvent, cette erreur provient du fait que l’application est publiée pour une architecture de processeur incompatible.</span><span class="sxs-lookup"><span data-stu-id="e2952-165">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="e2952-166">Si le processus de travail s’exécute en tant qu’application 32 bits et si l’application a été publiée pour cibler une architecture 64 bits, cette erreur se produit.</span><span class="sxs-lookup"><span data-stu-id="e2952-166">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="e2952-167">Pour corriger cette erreur, vous avez le choix entre plusieurs possibilités :</span><span class="sxs-lookup"><span data-stu-id="e2952-167">To fix this error, either:</span></span>

* <span data-ttu-id="e2952-168">Republiez l’application pour la même architecture de processeur que celle du processus de travail.</span><span class="sxs-lookup"><span data-stu-id="e2952-168">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="e2952-169">Publiez l’application sous forme de [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="e2952-169">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="e2952-170">500.33 - Échec du chargement du gestionnaire de requêtes ANCM</span><span class="sxs-lookup"><span data-stu-id="e2952-170">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="e2952-171">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-171">The worker process fails.</span></span> <span data-ttu-id="e2952-172">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-172">The app doesn't start.</span></span>

<span data-ttu-id="e2952-173">L’application n’a pas référencé le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e2952-173">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="e2952-174">Seules les applications ciblant le framework `Microsoft.AspNetCore.App` peuvent être hébergées par le module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2952-174">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the ASP.NET Core Module.</span></span>

<span data-ttu-id="e2952-175">Pour corriger cette erreur, vérifiez que l’application cible le framework `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e2952-175">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="e2952-176">Examinez le fichier `.runtimeconfig.json` pour vérifier le framework ciblé par l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-176">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="e2952-177">500.34 - Modèles d’hébergement mixtes ANCM non pris en charge</span><span class="sxs-lookup"><span data-stu-id="e2952-177">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="e2952-178">Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="e2952-178">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="e2952-179">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="e2952-179">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="e2952-180">500.35 - Applications in-process multiples dans le même processus ANCM</span><span class="sxs-lookup"><span data-stu-id="e2952-180">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="e2952-181">Le processus de travail ne peut pas exécuter à la fois une application in-process et une application out-of-process dans le même processus.</span><span class="sxs-lookup"><span data-stu-id="e2952-181">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="e2952-182">Pour corriger cette erreur, exécutez les applications dans des pools d’applications IIS distincts.</span><span class="sxs-lookup"><span data-stu-id="e2952-182">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="e2952-183">500.36 - Échec de chargement du gestionnaire out-of-process ANCM</span><span class="sxs-lookup"><span data-stu-id="e2952-183">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="e2952-184">Le gestionnaire de requêtes out-of-process, *aspnetcorev2_outofprocess.dll*, ne se trouve pas aux côtés du fichier *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="e2952-184">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="e2952-185">Cela indique une installation endommagée du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2952-185">This indicates a corrupted installation of the ASP.NET Core Module.</span></span>

<span data-ttu-id="e2952-186">Pour corriger cette erreur, réparez l’installation du [bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (pour IIS) ou Visual Studio (pour IIS Express).</span><span class="sxs-lookup"><span data-stu-id="e2952-186">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="e2952-187">500.37 - Échec du démarrage d’ANCM dans le délai imparti</span><span class="sxs-lookup"><span data-stu-id="e2952-187">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="e2952-188">ANCM n’a pas pu démarrer dans le délai imparti spécifié.</span><span class="sxs-lookup"><span data-stu-id="e2952-188">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="e2952-189">Par défaut, le délai d’expiration est de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="e2952-189">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="e2952-190">Cette erreur peut se produire quand un grand nombre d’applications démarrent sur la même machine.</span><span class="sxs-lookup"><span data-stu-id="e2952-190">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="e2952-191">Recherchez les pics d’utilisation du processeur/de la mémoire sur le serveur durant le démarrage.</span><span class="sxs-lookup"><span data-stu-id="e2952-191">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="e2952-192">Vous devrez peut-être décaler le processus de démarrage de plusieurs applications.</span><span class="sxs-lookup"><span data-stu-id="e2952-192">You may need to stagger the startup process of multiple apps.</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="e2952-193">500.30 - Échec du démarrage in-process</span><span class="sxs-lookup"><span data-stu-id="e2952-193">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="e2952-194">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-194">The worker process fails.</span></span> <span data-ttu-id="e2952-195">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-195">The app doesn't start.</span></span>

<span data-ttu-id="e2952-196">Le module ASP.NET Core tente de démarrer le runtime .NET Core in-process, mais sans succès.</span><span class="sxs-lookup"><span data-stu-id="e2952-196">The ASP.NET Core Module attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="e2952-197">Vous pouvez généralement déterminer la cause d’un échec de démarrage de processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="e2952-197">The cause of a process startup failure is usually determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="e2952-198">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="e2952-198">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="e2952-199">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="e2952-199">The worker process fails.</span></span> <span data-ttu-id="e2952-200">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="e2952-200">The app doesn't start.</span></span>

<span data-ttu-id="e2952-201">Une erreur inconnue s’est produite durant le chargement des composants du module ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2952-201">An unknown error occurred loading ASP.NET Core Module components.</span></span> <span data-ttu-id="e2952-202">Effectuez l’une des actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2952-202">Take one of the following actions:</span></span>

* <span data-ttu-id="e2952-203">Contactez le [Support Microsoft](https://support.microsoft.com/oas/default.aspx?prid=15832) (sélectionnez **Outils de développement**, puis **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="e2952-203">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="e2952-204">Posez une question sur Stack Overflow.</span><span class="sxs-lookup"><span data-stu-id="e2952-204">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="e2952-205">Signalez un problème sur notre [dépôt GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="e2952-205">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="e2952-206">Erreur de serveur interne 500</span><span class="sxs-lookup"><span data-stu-id="e2952-206">500 Internal Server Error</span></span>

<span data-ttu-id="e2952-207">L’application démarre, mais une erreur empêche le serveur de répondre à la requête.</span><span class="sxs-lookup"><span data-stu-id="e2952-207">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="e2952-208">Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="e2952-208">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="e2952-209">La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="e2952-209">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="e2952-210">Le Journal des événements de l’application indique généralement qu’elle a démarré normalement.</span><span class="sxs-lookup"><span data-stu-id="e2952-210">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="e2952-211">Du point de vue du serveur, c’est exact.</span><span class="sxs-lookup"><span data-stu-id="e2952-211">From the server's perspective, that's correct.</span></span> <span data-ttu-id="e2952-212">L’application a démarré, mais elle ne peut pas générer de réponse valide.</span><span class="sxs-lookup"><span data-stu-id="e2952-212">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="e2952-213">[Exécutez l’application depuis une invite de commandes](#run-the-app-at-a-command-prompt) sur le serveur ou [activez le journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log) pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="e2952-213">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="e2952-214">Échec du démarrage de l’application (code d’erreur « 0x800700c1 »)</span><span class="sxs-lookup"><span data-stu-id="e2952-214">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="e2952-215">L’application n’a pas pu démarrer, car l’assembly de l’application ( *.dll*) n’a pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="e2952-215">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="e2952-216">Cette erreur se produit lorsqu’il existe une incompatibilité au niveau du nombre de bits entre l’application publiée et le processus w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="e2952-216">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="e2952-217">Vérifiez que le paramètre 32 bits du pool d’applications est correct :</span><span class="sxs-lookup"><span data-stu-id="e2952-217">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="e2952-218">Sélectionnez le pool d’applications parmi les **Pools d’applications** IIS Manager.</span><span class="sxs-lookup"><span data-stu-id="e2952-218">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="e2952-219">Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="e2952-219">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="e2952-220">Définissez **Activer les applications 32 bits** :</span><span class="sxs-lookup"><span data-stu-id="e2952-220">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="e2952-221">Si vous déployez une application 32 bits (x86), définissez la valeur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="e2952-221">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="e2952-222">Si vous déployez une application 64 bits (x64), définissez la valeur sur `False`.</span><span class="sxs-lookup"><span data-stu-id="e2952-222">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="e2952-223">Réinitialisation de la connexion</span><span class="sxs-lookup"><span data-stu-id="e2952-223">Connection reset</span></span>

<span data-ttu-id="e2952-224">Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**.</span><span class="sxs-lookup"><span data-stu-id="e2952-224">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="e2952-225">C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="e2952-225">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="e2952-226">Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client.</span><span class="sxs-lookup"><span data-stu-id="e2952-226">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="e2952-227">La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.</span><span class="sxs-lookup"><span data-stu-id="e2952-227">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="e2952-228">Limites de démarrage par défaut</span><span class="sxs-lookup"><span data-stu-id="e2952-228">Default startup limits</span></span>

<span data-ttu-id="e2952-229">Le module ASP.NET Core est configuré avec une valeur *startupTimeLimit* par défaut de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="e2952-229">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="e2952-230">Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus.</span><span class="sxs-lookup"><span data-stu-id="e2952-230">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="e2952-231">Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="e2952-231">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="e2952-232">Résoudre les erreurs de démarrage des applications</span><span class="sxs-lookup"><span data-stu-id="e2952-232">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="e2952-233">Activer le journal de débogage du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2952-233">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="e2952-234">Ajoutez les paramètres de gestionnaire suivants au fichier *web.config* de l’application pour activer les journaux de débogage du module ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="e2952-234">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="e2952-235">Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="e2952-235">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="e2952-236">Journal des événements de l'application</span><span class="sxs-lookup"><span data-stu-id="e2952-236">Application Event Log</span></span>

<span data-ttu-id="e2952-237">Accédez au Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="e2952-237">Access the Application Event Log:</span></span>

1. <span data-ttu-id="e2952-238">Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez l’application **Observateur d’événements**.</span><span class="sxs-lookup"><span data-stu-id="e2952-238">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="e2952-239">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="e2952-239">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="e2952-240">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-240">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="e2952-241">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="e2952-241">Search for errors associated with the failing app.</span></span> <span data-ttu-id="e2952-242">Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.</span><span class="sxs-lookup"><span data-stu-id="e2952-242">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="e2952-243">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="e2952-243">Run the app at a command prompt</span></span>

<span data-ttu-id="e2952-244">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-244">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="e2952-245">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="e2952-245">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="e2952-246">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="e2952-246">Framework-dependent deployment</span></span>

<span data-ttu-id="e2952-247">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="e2952-247">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="e2952-248">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="e2952-248">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="e2952-249">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="e2952-249">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="e2952-250">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="e2952-250">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="e2952-251">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="e2952-251">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="e2952-252">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="e2952-252">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="e2952-253">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-253">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="e2952-254">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="e2952-254">Self-contained deployment</span></span>

<span data-ttu-id="e2952-255">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="e2952-255">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="e2952-256">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-256">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="e2952-257">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="e2952-257">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="e2952-258">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="e2952-258">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="e2952-259">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="e2952-259">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="e2952-260">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="e2952-260">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="e2952-261">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-261">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="e2952-262">Journal stdout du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e2952-262">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="e2952-263">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="e2952-263">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="e2952-264">Accédez au dossier de déploiement du site sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="e2952-264">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="e2952-265">Si le dossier *logs* n’est pas présent, créez-le.</span><span class="sxs-lookup"><span data-stu-id="e2952-265">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="e2952-266">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="e2952-266">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="e2952-267">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="e2952-267">Edit the *web.config* file.</span></span> <span data-ttu-id="e2952-268">Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="e2952-268">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="e2952-269">Dans le chemin, `stdout` est le préfixe du nom du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="e2952-269">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="e2952-270">Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé.</span><span class="sxs-lookup"><span data-stu-id="e2952-270">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="e2952-271">Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="e2952-271">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="e2952-272">Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="e2952-272">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="e2952-273">Enregistrez le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="e2952-273">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="e2952-274">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-274">Make a request to the app.</span></span>
1. <span data-ttu-id="e2952-275">Accédez au dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="e2952-275">Navigate to the *logs* folder.</span></span> <span data-ttu-id="e2952-276">Recherchez et ouvrez le journal stdout le plus récent.</span><span class="sxs-lookup"><span data-stu-id="e2952-276">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="e2952-277">Déterminez si le journal contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="e2952-277">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2952-278">Désactivez la journalisation stdout une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="e2952-278">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="e2952-279">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="e2952-279">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="e2952-280">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="e2952-280">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="e2952-281">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="e2952-281">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="e2952-282">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="e2952-282">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="e2952-283">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="e2952-283">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="e2952-284">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="e2952-284">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="e2952-285">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="e2952-285">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="e2952-286">Afficher la page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="e2952-286">Enable the Developer Exception Page</span></span>

<span data-ttu-id="e2952-287">Vous pouvez ajouter la [variable d’environnement `ASPNETCORE_ENVIRONMENT` au fichier web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="e2952-287">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="e2952-288">Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="e2952-288">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="e2952-289">La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="e2952-289">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="e2952-290">Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="e2952-290">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="e2952-291">Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="e2952-291">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="e2952-292">Erreurs de démarrage courantes</span><span class="sxs-lookup"><span data-stu-id="e2952-292">Common startup errors</span></span>

<span data-ttu-id="e2952-293">Consultez <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="e2952-293">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="e2952-294">La plupart des problèmes courants qui empêchent le démarrage de l’application sont traités dans la rubrique de référence.</span><span class="sxs-lookup"><span data-stu-id="e2952-294">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="e2952-295">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="e2952-295">Obtain data from an app</span></span>

<span data-ttu-id="e2952-296">Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal.</span><span class="sxs-lookup"><span data-stu-id="e2952-296">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="e2952-297">Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="e2952-297">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="e2952-298">Créer un fichier dump</span><span class="sxs-lookup"><span data-stu-id="e2952-298">Create a dump</span></span>

<span data-ttu-id="e2952-299">Un fichier *dump* est un instantané de la mémoire système et peut aider à déterminer la cause de l’arrêt d’une application, d’un échec de démarrage ou de la lenteur d’une application.</span><span class="sxs-lookup"><span data-stu-id="e2952-299">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="e2952-300">L’application cesse de fonctionner ou rencontre une exception</span><span class="sxs-lookup"><span data-stu-id="e2952-300">App crashes or encounters an exception</span></span>

<span data-ttu-id="e2952-301">Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :</span><span class="sxs-lookup"><span data-stu-id="e2952-301">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="e2952-302">Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="e2952-302">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="e2952-303">Le pool d’application doit disposer des accès d’écriture dans le dossier.</span><span class="sxs-lookup"><span data-stu-id="e2952-303">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="e2952-304">Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="e2952-304">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="e2952-305">Si l’application utilise le [modèle d’hébergement in-process](xref:fundamentals/servers/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="e2952-305">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="e2952-306">Si l’application utilise le [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="e2952-306">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="e2952-307">Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.</span><span class="sxs-lookup"><span data-stu-id="e2952-307">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="e2952-308">Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="e2952-308">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="e2952-309">Si l’application utilise le [modèle d’hébergement in-process](xref:fundamentals/servers/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="e2952-309">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="e2952-310">Si l’application utilise le [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="e2952-310">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="e2952-311">Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement.</span><span class="sxs-lookup"><span data-stu-id="e2952-311">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="e2952-312">Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.</span><span class="sxs-lookup"><span data-stu-id="e2952-312">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="e2952-313">Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).</span><span class="sxs-lookup"><span data-stu-id="e2952-313">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="e2952-314">L’application se bloque, ne démarre pas ou s’exécute normalement</span><span class="sxs-lookup"><span data-stu-id="e2952-314">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="e2952-315">Lorsqu’une application *se bloque* (ne répond plus mais ne s’arrête pas), ne démarre pas ou s’exécute normalement, consultez [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Fichiers dump en mode utilisateur : choisir le meilleur outil) afin de sélectionner l’outil adapté pour produire le fichier dump.</span><span class="sxs-lookup"><span data-stu-id="e2952-315">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="e2952-316">Analyser le fichier dump</span><span class="sxs-lookup"><span data-stu-id="e2952-316">Analyze the dump</span></span>

<span data-ttu-id="e2952-317">Un fichier dump peut être analysé à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="e2952-317">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="e2952-318">Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).</span><span class="sxs-lookup"><span data-stu-id="e2952-318">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="e2952-319">Débogage distant</span><span class="sxs-lookup"><span data-stu-id="e2952-319">Remote debugging</span></span>

<span data-ttu-id="e2952-320">Consultez [Déboguer à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) dans la documentation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2952-320">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="e2952-321">Application Insights</span><span class="sxs-lookup"><span data-stu-id="e2952-321">Application Insights</span></span>

<span data-ttu-id="e2952-322">[Application Insights](/azure/application-insights/) fournit des données de télémétrie des applications hébergées par IIS, y compris des fonctionnalités de journalisation des erreurs et de création de rapports.</span><span class="sxs-lookup"><span data-stu-id="e2952-322">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="e2952-323">Application Insights peut uniquement générer des rapports sur les erreurs qui surviennent après le démarrage de l’application quand les fonctionnalités de journalisation de l’application sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="e2952-323">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="e2952-324">Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="e2952-324">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="e2952-325">Conseils supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e2952-325">Additional advice</span></span>

<span data-ttu-id="e2952-326">Parfois, une application opérationnelle échoue après la mise à niveau du SDK .NET Core sur l’ordinateur de développement ou des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-326">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="e2952-327">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="e2952-327">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="e2952-328">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="e2952-328">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="e2952-329">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="e2952-329">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="e2952-330">Effacez les caches de package aux emplacements *%UserProfile%\\.nuget\\packages* et *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="e2952-330">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="e2952-331">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="e2952-331">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="e2952-332">Vérifiez que le déploiement préalable sur le serveur a été supprimé complètement avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="e2952-332">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="e2952-333">Un moyen pratique pour effacer les caches de package consiste à exécuter `dotnet nuget locals all --clear` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="e2952-333">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="e2952-334">Vous pouvez également effacer les caches de package en utilisant l’outil [nuget.exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="e2952-334">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="e2952-335">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="e2952-335">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2952-336">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="e2952-336">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
