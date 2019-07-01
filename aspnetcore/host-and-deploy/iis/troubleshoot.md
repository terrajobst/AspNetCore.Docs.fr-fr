---
title: Résoudre les problèmes liés à ASP.NET Core sur IIS
author: guardrex
description: Découvrez comment diagnostiquer les problèmes liés aux déploiements Internet Information Services (IIS) d’applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313657"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="3c234-103">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="3c234-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="3c234-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3c234-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3c234-105">Cet article fournit des instructions sur la façon de diagnostiquer un problème de démarrage d’application ASP.NET Core dans le cas d’un hébergement avec [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="3c234-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="3c234-106">Les informations contenues dans cet article s’appliquent à l’hébergement dans IIS sur Windows Server et Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="3c234-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="3c234-107">Rubriques de dépannage supplémentaires :</span><span class="sxs-lookup"><span data-stu-id="3c234-107">Additional troubleshooting topics:</span></span>

* <span data-ttu-id="3c234-108">Azure App Service utilise également le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et IIS pour héberger des applications.</span><span class="sxs-lookup"><span data-stu-id="3c234-108">Azure App Service also uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps.</span></span> <span data-ttu-id="3c234-109">Pour des conseils de résolution des problèmes qui se rapportent spécifiquement à Azure App Service, consultez <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="3c234-109">For troubleshooting advice that pertains specifically to Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
* <span data-ttu-id="3c234-110"><xref:fundamentals/error-handling> Couvre comment gérer les erreurs dans des applications ASP.NET Core pendant le développement sur un système local.</span><span class="sxs-lookup"><span data-stu-id="3c234-110"><xref:fundamentals/error-handling> covers how to handle errors in ASP.NET Core apps during development on a local system.</span></span>
* <span data-ttu-id="3c234-111">[Apprendre à déboguer avec Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) présente les fonctionnalités du débogueur Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c234-111">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) introduces the features of the Visual Studio debugger.</span></span>
* <span data-ttu-id="3c234-112">[Débogage avec Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) décrit la prise en charge du débogage intégré à Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3c234-112">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) describes the debugging support built into Visual Studio Code.</span></span>

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="3c234-113">Résoudre les erreurs de démarrage des applications</span><span class="sxs-lookup"><span data-stu-id="3c234-113">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="3c234-114">Activer le journal de débogage du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c234-114">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="3c234-115">Ajoutez les paramètres de gestionnaire suivants au fichier *web.config* de l’application pour activer les journaux de débogage du module ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="3c234-115">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="3c234-116">Vérifiez que le chemin spécifié pour le journal existe, et que l’identité du pool d’applications dispose des autorisations en écriture pour l’emplacement.</span><span class="sxs-lookup"><span data-stu-id="3c234-116">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="3c234-117">Journal des événements de l'application</span><span class="sxs-lookup"><span data-stu-id="3c234-117">Application Event Log</span></span>

<span data-ttu-id="3c234-118">Accédez au Journal des événements de l’application :</span><span class="sxs-lookup"><span data-stu-id="3c234-118">Access the Application Event Log:</span></span>

1. <span data-ttu-id="3c234-119">Ouvrez le menu Démarrer, recherchez **Observateur d’événements**, puis sélectionnez l’application **Observateur d’événements**.</span><span class="sxs-lookup"><span data-stu-id="3c234-119">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="3c234-120">Dans **Observateur d’événements**, ouvrez le nœud **Journaux Windows**.</span><span class="sxs-lookup"><span data-stu-id="3c234-120">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="3c234-121">Sélectionnez **Application** pour ouvrir le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-121">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="3c234-122">Recherchez les erreurs liées à l’application défectueuse.</span><span class="sxs-lookup"><span data-stu-id="3c234-122">Search for errors associated with the failing app.</span></span> <span data-ttu-id="3c234-123">Les erreurs sont signalées par une valeur *IIS AspNetCore Module* (Module AspNetCore IIS) ou *IIS Express AspNetCore Module* (Module AspNetCore IIS Express) dans la colonne *Source*.</span><span class="sxs-lookup"><span data-stu-id="3c234-123">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="3c234-124">Exécuter l’application depuis une invite de commandes</span><span class="sxs-lookup"><span data-stu-id="3c234-124">Run the app at a command prompt</span></span>

<span data-ttu-id="3c234-125">De nombreuses erreurs de démarrage ne produisent pas d’informations utiles dans le Journal des événements de l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-125">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="3c234-126">Vous pouvez trouver la cause de certaines erreurs en exécutant l’application depuis une invite de commandes sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="3c234-126">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="3c234-127">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="3c234-127">Framework-dependent deployment</span></span>

<span data-ttu-id="3c234-128">Si l’application est un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) :</span><span class="sxs-lookup"><span data-stu-id="3c234-128">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="3c234-129">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’application en exécutant l’assembly de l’application avec *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="3c234-129">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="3c234-130">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="3c234-130">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="3c234-131">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="3c234-131">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="3c234-132">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="3c234-132">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="3c234-133">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="3c234-133">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="3c234-134">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-134">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="3c234-135">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="3c234-135">Self-contained deployment</span></span>

<span data-ttu-id="3c234-136">Si l’application est un [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) :</span><span class="sxs-lookup"><span data-stu-id="3c234-136">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="3c234-137">Depuis une invite de commandes, accédez au dossier de déploiement et exécutez l’exécutable de l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-137">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="3c234-138">Dans la commande suivante, substituez le nom de l’assembly de l’application à \<assembly_name>: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="3c234-138">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="3c234-139">La sortie de console de l’application est écrite dans la fenêtre de console, affichant toutes les erreurs éventuelles.</span><span class="sxs-lookup"><span data-stu-id="3c234-139">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="3c234-140">Si les erreurs se produisent pendant qu’une requête est adressée à l’application, effectuez une requête en direction de l’hôte et du port sur lequel Kestrel écoute.</span><span class="sxs-lookup"><span data-stu-id="3c234-140">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="3c234-141">À l’aide de l’hôte et du port par défaut, faites une requête en direction de `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="3c234-141">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="3c234-142">Si l’application répond normalement à l’adresse de point de terminaison Kestrel, le problème est probablement lié à la configuration de l’hébergement plutôt qu’à l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-142">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="3c234-143">Journal stdout du module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c234-143">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="3c234-144">Pour activer et afficher les journaux stdout :</span><span class="sxs-lookup"><span data-stu-id="3c234-144">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="3c234-145">Accédez au dossier de déploiement du site sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="3c234-145">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="3c234-146">Si le dossier *logs* n’est pas présent, créez-le.</span><span class="sxs-lookup"><span data-stu-id="3c234-146">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="3c234-147">Pour obtenir des instructions sur l’activation de MSBuild et créer le dossier *logs* dans le déploiement automatiquement, consultez la rubrique [Structure des répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="3c234-147">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="3c234-148">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3c234-148">Edit the *web.config* file.</span></span> <span data-ttu-id="3c234-149">Définissez **stdoutLogEnabled** sur `true` et faites pointer le chemin **stdoutLogFile** vers le dossier *logs* (par exemple, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="3c234-149">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="3c234-150">Dans le chemin, `stdout` est le préfixe du nom du fichier journal.</span><span class="sxs-lookup"><span data-stu-id="3c234-150">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="3c234-151">Un horodatage, un ID de processus et une extension de fichier sont ajoutés automatiquement quand le journal est créé.</span><span class="sxs-lookup"><span data-stu-id="3c234-151">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="3c234-152">Si `stdout` est utilisé comme préfixe du nom de fichier, un fichier journal classique porte le nom *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="3c234-152">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="3c234-153">Vérifiez que l’identité de votre pool d’applications dispose des autorisations d’écriture sur le dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="3c234-153">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="3c234-154">Enregistrez le fichier *web.config* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="3c234-154">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="3c234-155">Adressez une requête à l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-155">Make a request to the app.</span></span>
1. <span data-ttu-id="3c234-156">Accédez au dossier *logs*.</span><span class="sxs-lookup"><span data-stu-id="3c234-156">Navigate to the *logs* folder.</span></span> <span data-ttu-id="3c234-157">Recherchez et ouvrez le journal stdout le plus récent.</span><span class="sxs-lookup"><span data-stu-id="3c234-157">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="3c234-158">Déterminez si le journal contient des erreurs.</span><span class="sxs-lookup"><span data-stu-id="3c234-158">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c234-159">Désactivez la journalisation stdout une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="3c234-159">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="3c234-160">Modifiez le fichier *web.config*.</span><span class="sxs-lookup"><span data-stu-id="3c234-160">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="3c234-161">Définissez **stdoutLogEnabled** sur `false`.</span><span class="sxs-lookup"><span data-stu-id="3c234-161">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="3c234-162">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="3c234-162">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="3c234-163">Si vous ne désactivez pas le journal stdout, l’application ou le serveur risque d’échouer.</span><span class="sxs-lookup"><span data-stu-id="3c234-163">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="3c234-164">Il n’existe aucune limite quant à la taille du fichier journal ou au nombre de fichiers journaux créés.</span><span class="sxs-lookup"><span data-stu-id="3c234-164">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="3c234-165">Pour les opérations de journalisation courantes dans une application ASP.NET Core, utilisez une bibliothèque de journalisation qui limite la taille du fichier journal et applique une rotation aux journaux.</span><span class="sxs-lookup"><span data-stu-id="3c234-165">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="3c234-166">Pour plus d’informations, voir [Fournisseurs de journalisation tiers](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="3c234-166">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="3c234-167">Afficher la page d’exception de développeur</span><span class="sxs-lookup"><span data-stu-id="3c234-167">Enable the Developer Exception Page</span></span>

<span data-ttu-id="3c234-168">Vous pouvez ajouter la [variable d’environnement `ASPNETCORE_ENVIRONMENT` au fichier web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) pour exécuter l’application dans l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="3c234-168">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="3c234-169">Tant que l’environnement n’est pas substitué dans le démarrage de l’application par `UseEnvironment` sur le générateur de l’hôte, la définition de la variable d’environnement permet à la [page d’exception de développeur](xref:fundamentals/error-handling) d’apparaître quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="3c234-169">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

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

<span data-ttu-id="3c234-170">La définition de la variable d’environnement `ASPNETCORE_ENVIRONMENT` est recommandée uniquement en vue d’une utilisation sur les serveurs de préproduction et de test qui ne sont pas exposés à Internet.</span><span class="sxs-lookup"><span data-stu-id="3c234-170">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="3c234-171">Supprimez la variable d’environnement du fichier *web.config* une fois la résolution des problèmes effectuée.</span><span class="sxs-lookup"><span data-stu-id="3c234-171">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="3c234-172">Pour plus d’informations sur la définition des variables d’environnement dans *web.config*, consultez la section sur [l’élément enfant environmentVariables d’aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="3c234-172">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="3c234-173">Erreurs de démarrage courantes</span><span class="sxs-lookup"><span data-stu-id="3c234-173">Common startup errors</span></span>

<span data-ttu-id="3c234-174">Consultez <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="3c234-174">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="3c234-175">La plupart des problèmes courants qui empêchent le démarrage de l’application sont traités dans la rubrique de référence.</span><span class="sxs-lookup"><span data-stu-id="3c234-175">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="3c234-176">Obtenir des données à partir d’une application</span><span class="sxs-lookup"><span data-stu-id="3c234-176">Obtain data from an app</span></span>

<span data-ttu-id="3c234-177">Si une application est capable de répondre aux requêtes, obtenez des informations sur une requête, une connexion et d’autres informations supplémentaires à partir d’une application à l’aide de l’intergiciel en ligne terminal.</span><span class="sxs-lookup"><span data-stu-id="3c234-177">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="3c234-178">Pour obtenir des informations supplémentaires ainsi qu'un code d'exemple, consultez <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="3c234-178">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="3c234-179">Créer un fichier dump</span><span class="sxs-lookup"><span data-stu-id="3c234-179">Create a dump</span></span>

<span data-ttu-id="3c234-180">Un fichier *dump* est un instantané de la mémoire système et peut aider à déterminer la cause de l’arrêt d’une application, d’un échec de démarrage ou de la lenteur d’une application.</span><span class="sxs-lookup"><span data-stu-id="3c234-180">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="3c234-181">L’application cesse de fonctionner ou rencontre une exception</span><span class="sxs-lookup"><span data-stu-id="3c234-181">App crashes or encounters an exception</span></span>

<span data-ttu-id="3c234-182">Obtenez un fichier dump et analysez-le depuis le [Rapport d'erreurs Windows](/windows/desktop/wer/windows-error-reporting) :</span><span class="sxs-lookup"><span data-stu-id="3c234-182">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="3c234-183">Créez un dossier pour accueillir les fichiers d’image mémoire dans `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="3c234-183">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="3c234-184">Le pool d’application doit disposer des accès d’écriture dans le dossier.</span><span class="sxs-lookup"><span data-stu-id="3c234-184">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="3c234-185">Exécutez le [script PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="3c234-185">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="3c234-186">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="3c234-186">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="3c234-187">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="3c234-187">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="3c234-188">Exécutez l’application en reproduisant les conditions ayant entraîné l’incident.</span><span class="sxs-lookup"><span data-stu-id="3c234-188">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="3c234-189">Une fois l’incident survenu, exécutez le [script PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1) :</span><span class="sxs-lookup"><span data-stu-id="3c234-189">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="3c234-190">Si l’application utilise le [modèle d’hébergement in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), exécutez le script pour *w3wp.exe* :</span><span class="sxs-lookup"><span data-stu-id="3c234-190">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="3c234-191">Si l’application utilise le [modèle d’hébergement out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), exécutez le script pour *dotnet.exe* :</span><span class="sxs-lookup"><span data-stu-id="3c234-191">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="3c234-192">Après l’arrêt de l’application et après avoir terminé la collection dump, l’application peut être fermée normalement.</span><span class="sxs-lookup"><span data-stu-id="3c234-192">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="3c234-193">Le script PowerShell configure le rapport d’erreurs Windows pour collecter jusqu'à cinq fichiers dump par application.</span><span class="sxs-lookup"><span data-stu-id="3c234-193">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="3c234-194">Les fichiers dump d’incident peuvent occuper plus d’espace disque (jusqu’à plusieurs gigaoctets par fichier).</span><span class="sxs-lookup"><span data-stu-id="3c234-194">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="3c234-195">L’application se bloque, ne démarre pas ou s’exécute normalement</span><span class="sxs-lookup"><span data-stu-id="3c234-195">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="3c234-196">Lorsqu’une application *se bloque* (ne répond plus mais ne s’arrête pas), ne démarre pas ou s’exécute normalement, consultez [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (Fichiers dump en mode utilisateur : choisir le meilleur outil) afin de sélectionner l’outil adapté pour produire le fichier dump.</span><span class="sxs-lookup"><span data-stu-id="3c234-196">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="3c234-197">Analyser le fichier dump</span><span class="sxs-lookup"><span data-stu-id="3c234-197">Analyze the dump</span></span>

<span data-ttu-id="3c234-198">Un fichier dump peut être analysé à l’aide de plusieurs approches.</span><span class="sxs-lookup"><span data-stu-id="3c234-198">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="3c234-199">Pour plus d’informations, consultez [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analyser un fichier dump en mode utilisateur).</span><span class="sxs-lookup"><span data-stu-id="3c234-199">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="3c234-200">Débogage distant</span><span class="sxs-lookup"><span data-stu-id="3c234-200">Remote debugging</span></span>

<span data-ttu-id="3c234-201">Consultez [Déboguer à distance ASP.NET Core sur un ordinateur IIS distant dans Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) dans la documentation de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c234-201">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="3c234-202">Application Insights</span><span class="sxs-lookup"><span data-stu-id="3c234-202">Application Insights</span></span>

<span data-ttu-id="3c234-203">[Application Insights](/azure/application-insights/) fournit des données de télémétrie des applications hébergées par IIS, y compris des fonctionnalités de journalisation des erreurs et de création de rapports.</span><span class="sxs-lookup"><span data-stu-id="3c234-203">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="3c234-204">Application Insights peut uniquement générer des rapports sur les erreurs qui surviennent après le démarrage de l’application quand les fonctionnalités de journalisation de l’application sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="3c234-204">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="3c234-205">Pour plus d’informations, voir [Application Insights pour ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="3c234-205">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="3c234-206">Conseils supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3c234-206">Additional advice</span></span>

<span data-ttu-id="3c234-207">Parfois, une application opérationnelle échoue après la mise à niveau du SDK .NET Core sur l’ordinateur de développement ou des versions de package dans l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-207">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="3c234-208">Dans certains cas, les packages incohérents peuvent bloquer une application quand vous effectuez des mises à niveau majeures.</span><span class="sxs-lookup"><span data-stu-id="3c234-208">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="3c234-209">Vous pouvez résoudre la plupart de ces problèmes en suivant les instructions suivantes :</span><span class="sxs-lookup"><span data-stu-id="3c234-209">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="3c234-210">Supprimez les dossiers *bin* et *obj*.</span><span class="sxs-lookup"><span data-stu-id="3c234-210">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="3c234-211">Effacez les caches de package aux emplacements *%UserProfile%\\.nuget\\packages* et *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="3c234-211">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="3c234-212">Restaurez et regénérez le projet.</span><span class="sxs-lookup"><span data-stu-id="3c234-212">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="3c234-213">Vérifiez que le déploiement préalable sur le serveur a été supprimé complètement avant de redéployer l’application.</span><span class="sxs-lookup"><span data-stu-id="3c234-213">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="3c234-214">Un moyen pratique pour effacer les caches de package consiste à exécuter `dotnet nuget locals all --clear` à partir d’une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="3c234-214">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="3c234-215">Vous pouvez également effacer les caches de package en utilisant l’outil [nuget.exe](https://www.nuget.org/downloads) et en exécutant la commande `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="3c234-215">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="3c234-216">*NuGet.exe* n’étant pas une installation fournie avec le système d’exploitation de bureau Windows, il doit être obtenu séparément à partir du [site web de NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="3c234-216">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c234-217">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3c234-217">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
