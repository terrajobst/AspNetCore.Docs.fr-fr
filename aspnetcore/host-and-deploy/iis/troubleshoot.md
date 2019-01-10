---
title: Résoudre les problèmes liés à ASP.NET Core sur IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Internet Information Services (IIS) d’applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 68fcd578c051ae9ba6234cad0465a7ef42f1ed14
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637688"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="c064a-103">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="c064a-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="c064a-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c064a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c064a-105">Cet article fournit des instructions sur la façon de diagnostiquer un problème de démarrage d’application ASP.NET Core dans le cas d’un hébergement avec [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="c064a-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="c064a-106">Les informations contenues dans cet article s’appliquent à l’hébergement dans IIS sur Windows Server et Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="c064a-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c064a-107">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="c064a-107">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="c064a-108">Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* ou un *échec de démarrage 500.30* qui se produit pendant un débogage local.</span><span class="sxs-lookup"><span data-stu-id="c064a-108">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="c064a-109">Dans Visual Studio, un projet ASP.NET Core est par défaut hébergé sur [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) pendant une opération de débogage.</span><span class="sxs-lookup"><span data-stu-id="c064a-109">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="c064a-110">Vous pouvez suivre les conseils indiqués dans cette rubrique pour résoudre un *échec de processus 502.5* qui se produit pendant un débogage local.</span><span class="sxs-lookup"><span data-stu-id="c064a-110">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

<span data-ttu-id="c064a-111">Rubriques de dépannage supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="c064a-111">Additional troubleshooting topics:</span></span>

<xref:host-and-deploy/azure-apps/troubleshoot>  
<span data-ttu-id="c064a-112">Bien qu’App Service utilise le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et IIS pour héberger des applications, consultez la rubrique dédiée pour obtenir des instructions qui concernent spécifiquement App Service.</span><span class="sxs-lookup"><span data-stu-id="c064a-112">Although App Service uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps, see the dedicated topic for instructions specific to App Service.</span></span>

<xref:fundamentals/error-handling>  
<span data-ttu-id="c064a-113">Découvrez comment gérer les erreurs dans les applications ASP.NET Core pendant le développement sur un système local.</span><span class="sxs-lookup"><span data-stu-id="c064a-113">Discover how to handle errors in ASP.NET Core apps during development on a local system.</span></span>

[<span data-ttu-id="c064a-114">Apprendre à déboguer avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c064a-114">Learn to debug using Visual Studio</span></span>](/visualstudio/debugger/getting-started-with-the-debugger)  
<span data-ttu-id="c064a-115">Cette rubrique présente les fonctionnalités du débogueur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c064a-115">This topic introduces the features of the Visual Studio debugger.</span></span>

[<span data-ttu-id="c064a-116">Débogage avec Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c064a-116">Debugging with Visual Studio Code</span></span>](https://code.visualstudio.com/docs/editor/debugging)  
<span data-ttu-id="c064a-117">Découvrez la prise en charge du débogage intégrée à Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c064a-117">Learn about the debugging support built into Visual Studio Code.</span></span>

## <a name="app-startup-errors"></a><span data-ttu-id="c064a-118">Erreurs de démarrage de l’application</span><span class="sxs-lookup"><span data-stu-id="c064a-118">App startup errors</span></span>

### <a name="5025-process-failure"></a><span data-ttu-id="c064a-119">Échec de processus 502.5</span><span class="sxs-lookup"><span data-stu-id="c064a-119">502.5 Process Failure</span></span>

<span data-ttu-id="c064a-120">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="c064a-120">The worker process fails.</span></span> <span data-ttu-id="c064a-121">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="c064a-121">The app doesn't start.</span></span>

<span data-ttu-id="c064a-122">Le module ASP.NET Core tente, en vain, de démarrer le processus dotnet backend.</span><span class="sxs-lookup"><span data-stu-id="c064a-122">The ASP.NET Core Module attempts to start the backend dotnet process but it fails to start.</span></span> <span data-ttu-id="c064a-123">Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="c064a-123">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="c064a-124">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="c064a-124">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="c064a-125">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="c064a-125">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

<span data-ttu-id="c064a-126">La page d’erreur d’un *échec de processus 502.5* est retournée quand une erreur de configuration d’hébergement ou d’application entraîne l’échec du processus de travail :</span><span class="sxs-lookup"><span data-stu-id="c064a-126">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

![Fenêtre de navigateur affichant la page d’échec de processus 502.5](troubleshoot/_static/process-failure-page.png)

::: moniker range=">= aspnetcore-2.2"

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="c064a-128">500.30 - Échec du démarrage in-process</span><span class="sxs-lookup"><span data-stu-id="c064a-128">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="c064a-129">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="c064a-129">The worker process fails.</span></span> <span data-ttu-id="c064a-130">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="c064a-130">The app doesn't start.</span></span>

<span data-ttu-id="c064a-131">Le module ASP.NET Core tente, en vain, de démarrer le CLR .NET Core in-process.</span><span class="sxs-lookup"><span data-stu-id="c064a-131">The ASP.NET Core Module attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="c064a-132">Vous pouvez généralement déterminer la cause d’un échec de démarrage du processus à partir des entrées du [Journal des événements de l’application](#application-event-log) et du [journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="c064a-132">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span> 

<span data-ttu-id="c064a-133">Une condition d’échec courante est une application mal configurée qui cible une version du framework partagé ASP.NET Core non présente.</span><span class="sxs-lookup"><span data-stu-id="c064a-133">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="c064a-134">Vérifiez les versions du framework partagé ASP.NET Core qui sont installées sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="c064a-134">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="c064a-135">500.0 - Échec de chargement du gestionnaire in-process</span><span class="sxs-lookup"><span data-stu-id="c064a-135">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="c064a-136">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="c064a-136">The worker process fails.</span></span> <span data-ttu-id="c064a-137">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="c064a-137">The app doesn't start.</span></span>

<span data-ttu-id="c064a-138">Le module ASP.NET Core ne peut pas trouver le CLR .NET Core et le gestionnaire de requêtes in-process (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="c064a-138">The ASP.NET Core Module fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="c064a-139">Vérifiez que :</span><span class="sxs-lookup"><span data-stu-id="c064a-139">Check that:</span></span>

* <span data-ttu-id="c064a-140">l’application cible le package NuGet [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) ou le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ;</span><span class="sxs-lookup"><span data-stu-id="c064a-140">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="c064a-141">la version du framework partagé ASP.NET Core que l’application cible est installée sur l’ordinateur cible.</span><span class="sxs-lookup"><span data-stu-id="c064a-141">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="c064a-142">500.0 - Échec de chargement du gestionnaire out-of-process</span><span class="sxs-lookup"><span data-stu-id="c064a-142">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="c064a-143">Le processus de travail échoue.</span><span class="sxs-lookup"><span data-stu-id="c064a-143">The worker process fails.</span></span> <span data-ttu-id="c064a-144">L’application ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="c064a-144">The app doesn't start.</span></span>

<span data-ttu-id="c064a-145">Le module ASP.NET Core ne peut pas trouver le gestionnaire de requêtes d’hébergement out-of-process.</span><span class="sxs-lookup"><span data-stu-id="c064a-145">The ASP.NET Core Module fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="c064a-146">Vérifiez que le fichier *aspnetcorev2_outofprocess.dll* est présent dans un sous-dossier en regard de *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="c064a-146">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span> 

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="c064a-147">Erreur de serveur interne 500</span><span class="sxs-lookup"><span data-stu-id="c064a-147">500 Internal Server Error</span></span>

<span data-ttu-id="c064a-148">L’application démarre, mais une erreur empêche le serveur de répondre à la requête.</span><span class="sxs-lookup"><span data-stu-id="c064a-148">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="c064a-149">Cette erreur se produit dans le code de l’application pendant le démarrage ou durant la création d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="c064a-149">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="c064a-150">La réponse peut être dépourvue de contenu ou apparaître sous la forme d’une *erreur de serveur interne 500* dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="c064a-150">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="c064a-151">Le Journal des événements de l’application indique généralement qu’elle a démarré normalement.</span><span class="sxs-lookup"><span data-stu-id="c064a-151">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="c064a-152">Du point de vue du serveur, c’est exact.</span><span class="sxs-lookup"><span data-stu-id="c064a-152">From the server's perspective, that's correct.</span></span> <span data-ttu-id="c064a-153">L’application a démarré, mais elle ne peut pas générer de réponse valide.</span><span class="sxs-lookup"><span data-stu-id="c064a-153">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="c064a-154">[Exécutez l’application depuis une invite de commandes](#run-the-app-at-a-command-prompt) sur le serveur ou [activez le journal stdout du module ASP.NET Core](#aspnet-core-module-stdout-log) pour résoudre le problème.</span><span class="sxs-lookup"><span data-stu-id="c064a-154">[Run the app at a command prompt](#run-the-app-at-a-command-prompt) on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="c064a-155">Échec du démarrage de l’application (code d’erreur « 0x800700c1 »)</span><span class="sxs-lookup"><span data-stu-id="c064a-155">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="c064a-156">L’application n’a pas pu démarrer, car l’assembly de l’application (*.dll*) n’a pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="c064a-156">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="c064a-157">Cette erreur se produit lorsqu’il existe une incompatibilité au niveau du nombre de bits entre l’application publiée et le processus w3wp/iisexpress.</span><span class="sxs-lookup"><span data-stu-id="c064a-157">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="c064a-158">Vérifiez que le paramètre 32 bits du pool d’applications est correct :</span><span class="sxs-lookup"><span data-stu-id="c064a-158">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="c064a-159">Sélectionnez le pool d’applications parmi les **Pools d’applications** IIS Manager.</span><span class="sxs-lookup"><span data-stu-id="c064a-159">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="c064a-160">Sélectionnez **Paramètres avancés** sous **Modifier le pool d’applications** dans le volet **Actions**.</span><span class="sxs-lookup"><span data-stu-id="c064a-160">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="c064a-161">Définissez **Activer les applications 32 bits** :</span><span class="sxs-lookup"><span data-stu-id="c064a-161">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="c064a-162">Si vous déployez une application 32 bits (x86), définissez la valeur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="c064a-162">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="c064a-163">Si vous déployez une application 64 bits (x64), définissez la valeur sur `False`.</span><span class="sxs-lookup"><span data-stu-id="c064a-163">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="c064a-164">Réinitialisation de la connexion</span><span class="sxs-lookup"><span data-stu-id="c064a-164">Connection reset</span></span>

<span data-ttu-id="c064a-165">Si une erreur se produit après l’envoi des en-têtes, il est trop tard pour que le serveur puisse envoyer une **erreur de serveur interne 500**.</span><span class="sxs-lookup"><span data-stu-id="c064a-165">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="c064a-166">C’est souvent le cas quand une erreur se produit pendant la sérialisation des objets complexes d’une réponse.</span><span class="sxs-lookup"><span data-stu-id="c064a-166">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="c064a-167">Ce type d’erreur apparaît en tant qu’erreur de *réinitialisation de la connexion* sur le client.</span><span class="sxs-lookup"><span data-stu-id="c064a-167">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="c064a-168">La [journalisation des applications](xref:fundamentals/logging/index) peut aider à le résoudre.</span><span class="sxs-lookup"><span data-stu-id="c064a-168">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="c064a-169">Limites de démarrage par défaut</span><span class="sxs-lookup"><span data-stu-id="c064a-169">Default startup limits</span></span>

<span data-ttu-id="c064a-170">Le module ASP.NET Core est configuré avec une valeur *startupTimeLimit* par défaut de 120 secondes.</span><span class="sxs-lookup"><span data-stu-id="c064a-170">The ASP.NET Core Module is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="c064a-171">Quand cette valeur par défaut est conservée, une application peut mettre jusqu’à deux minutes à démarrer avant que le module ne consigne un échec de processus.</span><span class="sxs-lookup"><span data-stu-id="c064a-171">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="c064a-172">Pour plus d’informations sur la configuration du module, voir [Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="c064a-172">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="c064a-173">Résoudre les erreurs de démarrage des applications</span><span class="sxs-lookup"><span data-stu-id="c064a-173">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="c064a-174">Activer le journal de débogage du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c064a-174">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="c064a-175">Ajoutez les paramètres de gestionnaire suivants au fichier *web.config* de l’application pour activer les journaux de débogage du module ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="c064a-175">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="c064a-176">Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="c064a-176">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="c064a-177">Journal des événements de l'application</span><span class="sxs-lookup"><span data-stu-id="c064a-177">Application Event Log</span></span>

<span data-ttu-id="c064a-178">Accédez au Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="c064a-178">Access the Application Event Log:</span></span>

1. <span data-ttu-id="c064a-179">Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez l’application **Observateur d’événements**.</span><span class="sxs-lookup"><span data-stu-id="c064a-179">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="c064a-180">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="c064a-180">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="c064a-181">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-181">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="c064a-182">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="c064a-182">Search for errors associated with the failing app.</span></span> <span data-ttu-id="c064a-183">Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.</span><span class="sxs-lookup"><span data-stu-id="c064a-183">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="c064a-184">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="c064a-184">Run the app at a command prompt</span></span>

<span data-ttu-id="c064a-185">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-185">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="c064a-186">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="c064a-186">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="c064a-187">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="c064a-187">Framework-dependent deployment</span></span>

<span data-ttu-id="c064a-188">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="c064a-188">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="c064a-189">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="c064a-189">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="c064a-190">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="c064a-190">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="c064a-191">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="c064a-191">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="c064a-192">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="c064a-192">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="c064a-193">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="c064a-193">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="c064a-194">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-194">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="c064a-195">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="c064a-195">Self-contained deployment</span></span>

<span data-ttu-id="c064a-196">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="c064a-196">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="c064a-197">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-197">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="c064a-198">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="c064a-198">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="c064a-199">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="c064a-199">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="c064a-200">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="c064a-200">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="c064a-201">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="c064a-201">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="c064a-202">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-202">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="c064a-203">Journal stdout du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c064a-203">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="c064a-204">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="c064a-204">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="c064a-205">Accédez au dossier de déploiement du site sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="c064a-205">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="c064a-206">Si le dossier *logs* n’est pas présent, créez-le.</span><span class="sxs-lookup"><span data-stu-id="c064a-206">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="c064a-207">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="c064a-207">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="c064a-208">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="c064a-208">Edit the *web.config* file.</span></span> <span data-ttu-id="c064a-209">Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="c064a-209">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="c064a-210">Dans le chemin, `stdout` est le préfixe du nom du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="c064a-210">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="c064a-211">Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé.</span><span class="sxs-lookup"><span data-stu-id="c064a-211">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="c064a-212">Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="c064a-212">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span> 
1. <span data-ttu-id="c064a-213">Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="c064a-213">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="c064a-214">Enregistrez le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="c064a-214">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="c064a-215">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-215">Make a request to the app.</span></span>
1. <span data-ttu-id="c064a-216">Accédez au dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="c064a-216">Navigate to the *logs* folder.</span></span> <span data-ttu-id="c064a-217">Recherchez et ouvrez le journal stdout le plus récent.</span><span class="sxs-lookup"><span data-stu-id="c064a-217">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="c064a-218">Déterminez si le journal contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="c064a-218">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c064a-219">Désactivez la journalisation stdout une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="c064a-219">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="c064a-220">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="c064a-220">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="c064a-221">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="c064a-221">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="c064a-222">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="c064a-222">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="c064a-223">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="c064a-223">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="c064a-224">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="c064a-224">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="c064a-225">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="c064a-225">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="c064a-226">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="c064a-226">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="c064a-227">Afficher la page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="c064a-227">Enable the Developer Exception Page</span></span>

<span data-ttu-id="c064a-228">Vous pouvez ajouter la [variable d’environnement `ASPNETCORE_ENVIRONMENT` au fichier web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="c064a-228">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="c064a-229">Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="c064a-229">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="c064a-230">La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="c064a-230">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="c064a-231">Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="c064a-231">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="c064a-232">Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="c064a-232">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="c064a-233">Erreurs de démarrage courantes</span><span class="sxs-lookup"><span data-stu-id="c064a-233">Common startup errors</span></span>

<span data-ttu-id="c064a-234">Consultez <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="c064a-234">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="c064a-235">La plupart des problèmes courants qui empêchent le démarrage de l’application sont traités dans la rubrique de référence.</span><span class="sxs-lookup"><span data-stu-id="c064a-235">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="c064a-236">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="c064a-236">Obtain data from an app</span></span>

<span data-ttu-id="c064a-237">Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal.</span><span class="sxs-lookup"><span data-stu-id="c064a-237">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="c064a-238">Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="c064a-238">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="slow-or-hanging-app"></a><span data-ttu-id="c064a-239">Application lente ou bloquée</span><span class="sxs-lookup"><span data-stu-id="c064a-239">Slow or hanging app</span></span>

<span data-ttu-id="c064a-240">Quand une application répond lentement ou se bloque sur une requête, obtenez un [fichier de vidage](/visualstudio/debugger/using-dump-files) afin de l’analyser.</span><span class="sxs-lookup"><span data-stu-id="c064a-240">When an app responds slowly or hangs on a request, obtain and analyze a [dump file](/visualstudio/debugger/using-dump-files).</span></span> <span data-ttu-id="c064a-241">Les fichiers de vidage peuvent être obtenus à l’aide des outils suivants :</span><span class="sxs-lookup"><span data-stu-id="c064a-241">Dump files can be obtained using any of the following tools:</span></span>

* [<span data-ttu-id="c064a-242">ProcDump</span><span class="sxs-lookup"><span data-stu-id="c064a-242">ProcDump</span></span>](/sysinternals/downloads/procdump)
* [<span data-ttu-id="c064a-243">DebugDiag</span><span class="sxs-lookup"><span data-stu-id="c064a-243">DebugDiag</span></span>](https://www.microsoft.com/download/details.aspx?id=49924)
* <span data-ttu-id="c064a-244">WinDbg : [Télécharger les outils de débogage pour Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Débogage à l’aide de WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span><span class="sxs-lookup"><span data-stu-id="c064a-244">WinDbg: [Download Debugging tools for Windows](https://developer.microsoft.com/windows/hardware/download-windbg), [Debugging Using WinDbg](/windows-hardware/drivers/debugger/debugging-using-windbg)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="c064a-245">Débogage distant</span><span class="sxs-lookup"><span data-stu-id="c064a-245">Remote debugging</span></span>

<span data-ttu-id="c064a-246">Consultez [Déboguer à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) dans la documentation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c064a-246">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="c064a-247">Application Insights</span><span class="sxs-lookup"><span data-stu-id="c064a-247">Application Insights</span></span>

<span data-ttu-id="c064a-248">[Application Insights](/azure/application-insights/) fournit des données de télémétrie des applications hébergées par IIS, y compris des fonctionnalités de journalisation des erreurs et de création de rapports.</span><span class="sxs-lookup"><span data-stu-id="c064a-248">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="c064a-249">Application Insights peut uniquement générer des rapports sur les erreurs qui surviennent après le démarrage de l’application quand les fonctionnalités de journalisation de l’application sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="c064a-249">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="c064a-250">Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="c064a-250">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="c064a-251">Conseils supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c064a-251">Additional advice</span></span>

<span data-ttu-id="c064a-252">Parfois, une application opérationnelle échoue après la mise à niveau du SDK .NET Core sur l’ordinateur de développement ou des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-252">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="c064a-253">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="c064a-253">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="c064a-254">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="c064a-254">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="c064a-255">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="c064a-255">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="c064a-256">Effacez les caches de package aux emplacements *%UserProfile%\\.nuget\\packages* et *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="c064a-256">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="c064a-257">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="c064a-257">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="c064a-258">Vérifiez que le déploiement préalable sur le serveur a été supprimé complètement avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="c064a-258">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="c064a-259">Un moyen pratique pour effacer les caches de package consiste à exécuter `dotnet nuget locals all --clear` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="c064a-259">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="c064a-260">Vous pouvez également effacer les caches de package en utilisant l’outil [nuget.exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="c064a-260">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="c064a-261">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="c064a-261">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c064a-262">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="c064a-262">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
