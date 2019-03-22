---
title: Résoudre les problèmes liés à ASP.NET Core sur IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Internet Information Services (IIS) d’applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 1fa90737aadebe3f714c702fbce649629d79dcd4
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264550"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="a84df-103">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="a84df-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="a84df-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a84df-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a84df-105">Cet article fournit des instructions sur la façon de diagnostiquer un problème de démarrage d’application ASP.NET Core dans le cas d’un hébergement avec [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="a84df-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="a84df-106">Les informations contenues dans cet article s’appliquent à l’hébergement dans IIS sur Windows Server et Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="a84df-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a84df-107">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="a84df-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="a84df-108">Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* ou un *échec de démarrage 500.30* qui se produit pendant un débogage local.</span><span class="sxs-lookup"><span data-stu-id="a84df-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a84df-109">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="a84df-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="a84df-110">Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* qui se produit pendant un débogage local.</span><span class="sxs-lookup"><span data-stu-id="a84df-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="a84df-111">Rubriques de dépannage supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="a84df-111">Additional troubleshooting topics:</span></span>

<span data-ttu-id="a84df-112"><xref:host-and-deploy/azure-apps/troubleshoot> Bien qu’App Service utilise le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et IIS pour héberger des applications, consultez la rubrique dédiée pour obtenir des instructions qui concernent spécifiquement App Service.</span><span class="sxs-lookup"><span data-stu-id="a84df-112"><xref:host-and-deploy/azure-apps/troubleshoot> Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<span data-ttu-id="a84df-113"><xref:fundamentals/error-handling> Découvrez comment gérer les erreurs dans les applications ASP.NET Core pendant le développement sur un système local.</span><span class="sxs-lookup"><span data-stu-id="a84df-113"><xref:fundamentals/error-handling> Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

<span data-ttu-id="a84df-114">[Apprendre à déboguer avec Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) Cette rubrique présente les fonctionnalités du débogueur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a84df-114">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) This topic introduces the features of the Visual Studio debugger.</span></span>

<span data-ttu-id="a84df-115">[Débogage avec Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Découvrez la prise en charge du débogage intégrée à Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a84df-115">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="a84df-116">Erreurs de démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="a84df-116">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="a84df-117">Échec de processus 502.5</span><span class="sxs-lookup"><span data-stu-id="a84df-117">502.5 Process Failure</span></span>

<span data-ttu-id="a84df-118">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a84df-118">The worker process fails.</span></span> <span data-ttu-id="a84df-119">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a84df-119">The app doesn't start.</span></span>

<span data-ttu-id="a84df-120">Le module ASP.NET Core tente, en vain, de démarrer le processus dotnet backend.</span><span class="sxs-lookup"><span data-stu-id="a84df-120">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="a84df-121">Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="a84df-121">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="a84df-122">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="a84df-122">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="a84df-123">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="a84df-123">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="a84df-124">La page d’erreur d’un *échec de processus 502.5* est retournée quand une erreur de configuration d’hébergement ou d’application entraîne l’échec du processus de travail :</span><span class="sxs-lookup"><span data-stu-id="a84df-124">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Fenêtre de navigateur affichant la page d’échec de processus 502.5](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="a84df-126">500.30 - Échec du démarrage in-process</span><span class="sxs-lookup"><span data-stu-id="a84df-126">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="a84df-127">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a84df-127">The worker process fails.</span></span> <span data-ttu-id="a84df-128">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a84df-128">The app doesn't start.</span></span>

<span data-ttu-id="a84df-129">Le module ASP.NET Core tente, en vain, de démarrer le CLR .NET Core in-process.</span><span class="sxs-lookup"><span data-stu-id="a84df-129">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="a84df-130">Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="a84df-130">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="a84df-131">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="a84df-131">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="a84df-132">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="a84df-132">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="a84df-133">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="a84df-133">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="a84df-134">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a84df-134">The worker process fails.</span></span> <span data-ttu-id="a84df-135">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a84df-135">The app doesn't start.</span></span>

<span data-ttu-id="a84df-136">Le module ASP.NET Core ne peut pas trouver le CLR .NET Core et le gestionnaire de requêtes in-process (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="a84df-136">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="a84df-137">Vérifiez que :</span><span class="sxs-lookup"><span data-stu-id="a84df-137">Check that:</span></span>

* <span data-ttu-id="a84df-138">l’application cible le package NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) ou le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ;</span><span class="sxs-lookup"><span data-stu-id="a84df-138">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="a84df-139">la version du framework partagé ASP.NET Core que l’application cible est installée sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="a84df-139">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="a84df-140">500.0 - Échec de chargement du gestionnaire out-of-process</span><span class="sxs-lookup"><span data-stu-id="a84df-140">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="a84df-141">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="a84df-141">The worker process fails.</span></span> <span data-ttu-id="a84df-142">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="a84df-142">The app doesn't start.</span></span>

<span data-ttu-id="a84df-143">Le module ASP.NET Core ne peut pas trouver le gestionnaire de requêtes d’hébergement out-of-process.</span><span class="sxs-lookup"><span data-stu-id="a84df-143">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="a84df-144">Vérifiez que le fichier *aspnetcorev2_outofprocess.dll* est présent dans un sous-dossier en regard de *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="a84df-144">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="a84df-145">Erreur de serveur interne 500</span><span class="sxs-lookup"><span data-stu-id="a84df-145">500 Internal Server Error</span></span>

<span data-ttu-id="a84df-146">L’application démarre, mais une erreur empêche le serveur de répondre à la requête.</span><span class="sxs-lookup"><span data-stu-id="a84df-146">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="a84df-147">Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="a84df-147">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="a84df-148">La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="a84df-148">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="a84df-149">Le Journal des événements de l’application indique généralement qu’elle a démarré normalement.</span><span class="sxs-lookup"><span data-stu-id="a84df-149">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="a84df-150">Du point de vue du serveur, c’est exact.</span><span class="sxs-lookup"><span data-stu-id="a84df-150">From the server's perspective, that's correct.</span></span> <span data-ttu-id="a84df-151">L’application a démarré, mais elle ne peut pas générer de réponse valide.</span><span class="sxs-lookup"><span data-stu-id="a84df-151">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="a84df-152">[Exécutez l’application depuis une invite de commandes](#run-the-app-at-a-command-prompt) sur le serveur ou [activez le journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log) pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="a84df-152">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="a84df-153">Échec du démarrage de l’application (code d’erreur « 0x800700c1 »)</span><span class="sxs-lookup"><span data-stu-id="a84df-153">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="a84df-154">L’application n’a pas pu démarrer, car l’assembly de l’application (*.dll*) n’a pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="a84df-154">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="a84df-155">Cette erreur se produit lorsqu’il existe une incompatibilité au niveau du nombre de bits entre l’application publiée et le processus w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="a84df-155">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="a84df-156">Vérifiez que le paramètre 32 bits du pool d’applications est correct :</span><span class="sxs-lookup"><span data-stu-id="a84df-156">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="a84df-157">Sélectionnez le pool d’applications parmi les **Pools d’applications** IIS Manager.</span><span class="sxs-lookup"><span data-stu-id="a84df-157">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="a84df-158">Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="a84df-158">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="a84df-159">Définissez **Activer les applications 32 bits** :</span><span class="sxs-lookup"><span data-stu-id="a84df-159">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="a84df-160">Si vous déployez une application 32 bits (x86), définissez la valeur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="a84df-160">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="a84df-161">Si vous déployez une application 64 bits (x64), définissez la valeur sur `False`.</span><span class="sxs-lookup"><span data-stu-id="a84df-161">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="a84df-162">Réinitialisation de la connexion</span><span class="sxs-lookup"><span data-stu-id="a84df-162">Connection reset</span></span>

<span data-ttu-id="a84df-163">Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**.</span><span class="sxs-lookup"><span data-stu-id="a84df-163">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="a84df-164">C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="a84df-164">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="a84df-165">Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client.</span><span class="sxs-lookup"><span data-stu-id="a84df-165">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="a84df-166">La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.</span><span class="sxs-lookup"><span data-stu-id="a84df-166">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="a84df-167">Limites de démarrage par défaut</span><span class="sxs-lookup"><span data-stu-id="a84df-167">Default startup limits</span></span>

<span data-ttu-id="a84df-168">Le module ASP.NET Core est configuré avec une valeur *startupTimeLimit* par défaut de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="a84df-168">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="a84df-169">Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus.</span><span class="sxs-lookup"><span data-stu-id="a84df-169">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="a84df-170">Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="a84df-170">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="a84df-171">Résoudre les erreurs de démarrage des applications</span><span class="sxs-lookup"><span data-stu-id="a84df-171">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="a84df-172">Activer le journal de débogage du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a84df-172">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="a84df-173">Ajoutez les paramètres de gestionnaire suivants au fichier *web.config* de l’application pour activer les journaux de débogage du module ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="a84df-173">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="a84df-174">Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="a84df-174">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="a84df-175">Journal des événements de l'application</span><span class="sxs-lookup"><span data-stu-id="a84df-175">Application Event Log</span></span>

<span data-ttu-id="a84df-176">Accédez au Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="a84df-176">Access the Application Event Log:</span></span>

1. <span data-ttu-id="a84df-177">Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez l’application **Observateur d’événements**.</span><span class="sxs-lookup"><span data-stu-id="a84df-177">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="a84df-178">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="a84df-178">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="a84df-179">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-179">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="a84df-180">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="a84df-180">Search for errors associated with the failing app.</span></span> <span data-ttu-id="a84df-181">Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.</span><span class="sxs-lookup"><span data-stu-id="a84df-181">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="a84df-182">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="a84df-182">Run the app at a command prompt</span></span>

<span data-ttu-id="a84df-183">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-183">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="a84df-184">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="a84df-184">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="a84df-185">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="a84df-185">Framework-dependent deployment</span></span>

<span data-ttu-id="a84df-186">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="a84df-186">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="a84df-187">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="a84df-187">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="a84df-188">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="a84df-188">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="a84df-189">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="a84df-189">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="a84df-190">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="a84df-190">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="a84df-191">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="a84df-191">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="a84df-192">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-192">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="a84df-193">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="a84df-193">Self-contained deployment</span></span>

<span data-ttu-id="a84df-194">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="a84df-194">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="a84df-195">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-195">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="a84df-196">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="a84df-196">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="a84df-197">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="a84df-197">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="a84df-198">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="a84df-198">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="a84df-199">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="a84df-199">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="a84df-200">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-200">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="a84df-201">Journal stdout du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a84df-201">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="a84df-202">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="a84df-202">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="a84df-203">Accédez au dossier de déploiement du site sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="a84df-203">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="a84df-204">Si le dossier *logs* n’est pas présent, créez-le.</span><span class="sxs-lookup"><span data-stu-id="a84df-204">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="a84df-205">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="a84df-205">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="a84df-206">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a84df-206">Edit the *web.config* file.</span></span> <span data-ttu-id="a84df-207">Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="a84df-207">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="a84df-208">Dans le chemin, `stdout` est le préfixe du nom du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="a84df-208">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="a84df-209">Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé.</span><span class="sxs-lookup"><span data-stu-id="a84df-209">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="a84df-210">Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="a84df-210">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="a84df-211">Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="a84df-211">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="a84df-212">Enregistrez le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="a84df-212">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="a84df-213">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-213">Make a request to the app.</span></span>
1. <span data-ttu-id="a84df-214">Accédez au dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="a84df-214">Navigate to the *logs* folder.</span></span> <span data-ttu-id="a84df-215">Recherchez et ouvrez le journal stdout le plus récent.</span><span class="sxs-lookup"><span data-stu-id="a84df-215">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="a84df-216">Déterminez si le journal contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="a84df-216">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a84df-217">Désactivez la journalisation stdout une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="a84df-217">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="a84df-218">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="a84df-218">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="a84df-219">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="a84df-219">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="a84df-220">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="a84df-220">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="a84df-221">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="a84df-221">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="a84df-222">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="a84df-222">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="a84df-223">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="a84df-223">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a84df-224">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a84df-224">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="a84df-225">Afficher la page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="a84df-225">Enable the Developer Exception Page</span></span>

<span data-ttu-id="a84df-226">Vous pouvez ajouter la [variable d’environnement `ASPNETCORE_ENVIRONMENT` au fichier web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="a84df-226">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="a84df-227">Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="a84df-227">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="a84df-228">La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="a84df-228">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="a84df-229">Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="a84df-229">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="a84df-230">Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="a84df-230">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="a84df-231">Erreurs de démarrage courantes</span><span class="sxs-lookup"><span data-stu-id="a84df-231">Common startup errors</span></span>

<span data-ttu-id="a84df-232">Consultez <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="a84df-232">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="a84df-233">La plupart des problèmes courants qui empêchent le démarrage de l’application sont traités dans la rubrique de référence.</span><span class="sxs-lookup"><span data-stu-id="a84df-233">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="a84df-234">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="a84df-234">Obtain data from an app</span></span>

<span data-ttu-id="a84df-235">Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal.</span><span class="sxs-lookup"><span data-stu-id="a84df-235">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="a84df-236">Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="a84df-236">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="a84df-237">Créer un fichier dump</span><span class="sxs-lookup"><span data-stu-id="a84df-237">Create a dump</span></span>

<span data-ttu-id="a84df-238">Un fichier *dump* est un instantané de la mémoire système et peut aider à déterminer la cause de l’arrêt d’une application, d’un échec de démarrage ou de la lenteur d’une application.</span><span class="sxs-lookup"><span data-stu-id="a84df-238">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="a84df-239">L’application cesse de fonctionner ou rencontre une exception</span><span class="sxs-lookup"><span data-stu-id="a84df-239">App crashes or encounters an exception</span></span>

<span data-ttu-id="a84df-240">Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :</span><span class="sxs-lookup"><span data-stu-id="a84df-240">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="a84df-241">Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="a84df-241">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="a84df-242">Le pool d’application doit disposer des accès d’écriture dans le dossier.</span><span class="sxs-lookup"><span data-stu-id="a84df-242">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="a84df-243">Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="a84df-243">Run the [EnableDumps PowerShell script](https://github.com/aspnet/Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="a84df-244">Si l’application utilise le [modèle d’hébergement in-process](xref:fundamentals/servers/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="a84df-244">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="a84df-245">Si l’application utilise le [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="a84df-245">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="a84df-246">Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.</span><span class="sxs-lookup"><span data-stu-id="a84df-246">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="a84df-247">Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="a84df-247">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="a84df-248">Si l’application utilise le [modèle d’hébergement in-process](xref:fundamentals/servers/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="a84df-248">If the app uses the [in-process hosting model](xref:fundamentals/servers/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="a84df-249">Si l’application utilise le [modèle d’hébergement out-of-process](xref:fundamentals/servers/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="a84df-249">If the app uses the [out-of-process hosting model](xref:fundamentals/servers/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="a84df-250">Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement.</span><span class="sxs-lookup"><span data-stu-id="a84df-250">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="a84df-251">Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.</span><span class="sxs-lookup"><span data-stu-id="a84df-251">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="a84df-252">Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).</span><span class="sxs-lookup"><span data-stu-id="a84df-252">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="a84df-253">L’application se bloque, ne démarre pas ou s’exécute normalement</span><span class="sxs-lookup"><span data-stu-id="a84df-253">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="a84df-254">Lorsqu’une application *se bloque* (ne répond plus mais ne s’arrête pas), ne démarre pas ou s’exécute normalement, consultez [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Fichiers dump en mode utilisateur : choisir le meilleur outil) afin de sélectionner l’outil adapté pour produire le fichier dump.</span><span class="sxs-lookup"><span data-stu-id="a84df-254">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="a84df-255">Analyser le fichier dump</span><span class="sxs-lookup"><span data-stu-id="a84df-255">Analyze the dump</span></span>

<span data-ttu-id="a84df-256">Un fichier dump peut être analysé à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="a84df-256">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="a84df-257">Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).</span><span class="sxs-lookup"><span data-stu-id="a84df-257">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="a84df-258">Débogage distant</span><span class="sxs-lookup"><span data-stu-id="a84df-258">Remote debugging</span></span>

<span data-ttu-id="a84df-259">Consultez [Déboguer à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) dans la documentation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a84df-259">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="a84df-260">Application Insights</span><span class="sxs-lookup"><span data-stu-id="a84df-260">Application Insights</span></span>

<span data-ttu-id="a84df-261">[Application Insights](/azure/application-insights/) fournit des données de télémétrie des applications hébergées par IIS, y compris des fonctionnalités de journalisation des erreurs et de création de rapports.</span><span class="sxs-lookup"><span data-stu-id="a84df-261">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="a84df-262">Application Insights peut uniquement générer des rapports sur les erreurs qui surviennent après le démarrage de l’application quand les fonctionnalités de journalisation de l’application sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="a84df-262">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="a84df-263">Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="a84df-263">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="a84df-264">Conseils supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a84df-264">Additional advice</span></span>

<span data-ttu-id="a84df-265">Parfois, une application opérationnelle échoue après la mise à niveau du SDK .NET Core sur l’ordinateur de développement ou des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-265">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="a84df-266">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="a84df-266">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="a84df-267">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="a84df-267">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="a84df-268">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="a84df-268">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="a84df-269">Effacez les caches de package aux emplacements *%UserProfile%\\.nuget\\packages* et *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="a84df-269">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="a84df-270">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="a84df-270">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="a84df-271">Vérifiez que le déploiement préalable sur le serveur a été supprimé complètement avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="a84df-271">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="a84df-272">Un moyen pratique pour effacer les caches de package consiste à exécuter `dotnet nuget locals all --clear` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="a84df-272">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="a84df-273">Vous pouvez également effacer les caches de package en utilisant l’outil [nuget.exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="a84df-273">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="a84df-274">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="a84df-274">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a84df-275">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a84df-275">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
